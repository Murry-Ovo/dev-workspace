# Claude Code status line setup

Paste this whole document as your first message to Claude Code on the target machine. Claude will create the script, edit settings, and tell you to restart.

---

Set up a Claude Code status line on this machine that shows the git branch and a coloured context-window usage bar.

Do all of the following:

1. **Check `jq` is installed.** Run `command -v jq`. If missing:
   - **Mac/Linux:** tell me to run `brew install jq` and stop until I confirm.
   - **Windows:** tell me to run `winget install jqlang.jq` and stop until I confirm. Note the install path — it will be needed in the script (see step 2).

2. **Create `~/.claude/scripts/statusline.sh`** with this exact content (use the Write tool, then `chmod +x` on it):

   > **Windows note:** Claude Code runs the statusline in non-interactive bash — `.bashrc` is never sourced, so jq won't be on PATH even if you added it there. The script exports the path directly. Replace the winget package path below with the actual path on this machine (find it with `ls /c/Users/$USER/AppData/Local/Microsoft/WinGet/Packages/jqlang*`). Delete the `export PATH` line entirely on Mac/Linux.

   ```bash
   #!/usr/bin/env bash
   set -euo pipefail

   # Windows only: add winget jq to PATH (non-interactive bash skips .bashrc)
   # Delete this line on Mac/Linux
   export PATH="$PATH:/c/Users/YOUR_USERNAME/AppData/Local/Microsoft/WinGet/Packages/jqlang.jq_Microsoft.Winget.Source_8wekyb3d8bbwe"

   if ! command -v jq &>/dev/null; then
       exit 0
   fi

   # Read JSON input from stdin
   input=$(cat)

   # Get git branch and worktree status
   branch=$(git branch --show-current 2>/dev/null)
   if [ -n "$branch" ]; then
       git_dir=$(git rev-parse --git-dir 2>/dev/null)
       common_dir=$(git rev-parse --git-common-dir 2>/dev/null)

       if [ "$git_dir" != "$common_dir" ]; then
           branch_display="${branch} (worktree)"
       else
           branch_display="${branch}"
       fi
   else
       branch_display=""
   fi

   # Extract context window data
   usage=$(echo "$input" | jq '.context_window.current_usage')

   # Only show context if we have usage data
   if [ "$usage" != "null" ]; then
       # Calculate current context usage (input + cache creation + cache read)
       current=$(echo "$usage" | jq '.input_tokens + .cache_creation_input_tokens + .cache_read_input_tokens')
       size=$(echo "$input" | jq '.context_window.context_window_size')

       if [ "$size" = "null" ] || [ "$size" -eq 0 ] 2>/dev/null; then
           exit 0
       fi

       # Calculate percentage
       pct=$((current * 100 / size))

       # Color code based on percentage
       # Green: 0-50%, Amber: 50-65%, Red: 65%+
       if [ "$pct" -lt 50 ]; then
           color="\033[32m" # Green
       elif [ "$pct" -lt 65 ]; then
           color="\033[33m" # Amber/Yellow
       else
           color="\033[31m" # Red
       fi

       reset="\033[0m"

       # Create visual bar with 10 boxes
       filled=$((pct / 10))
       bar=""
       for ((i = 0; i < 10; i++)); do
           if [ "$i" -lt "$filled" ]; then
               bar="${bar}█"
           else
               bar="${bar}░"
           fi
       done

       # Print branch and colored bar with percentage
       if [ -n "$branch_display" ]; then
           printf "%s ${color}${bar} %d%%${reset}" "$branch_display" "$pct"
       else
           printf "${color}${bar} %d%%${reset}" "$pct"
       fi
   elif [ -n "$branch_display" ]; then
       # No context data but we have branch info
       printf "%s" "$branch_display"
   fi
   ```

3. **Edit `~/.claude/settings.json`** to add this block at the top level (merge with whatever's already there — don't overwrite existing settings like `theme`):

   ```json
   "statusLine": {
     "type": "command",
     "command": "bash ~/.claude/scripts/statusline.sh"
   }
   ```

   If the file doesn't exist, create it with just that block wrapped in `{ ... }`.

4. **Confirm the result.** Tell me to restart Claude Code (or open a new session) to pick up the change. The status line shows: `<branch-name> ████░░░░░░ 27%` with the bar coloured green/amber/red depending on context fill.

Notes:
- The script is read-only — it reads JSON Claude Code pipes to stdin (containing `context_window.current_usage` and `context_window.context_window_size`) and prints to stdout. No side effects.
- If the bar doesn't appear after restart, most likely cause is `jq` not on PATH. On **Windows**, the script must export the winget install path directly (see step 2) — adding it to `.bashrc` won't work because Claude Code runs statusline in non-interactive bash which skips `.bashrc`.
- `statusLine` is a Claude Code feature, not a plugin. The whole config lives in `~/.claude/`.
