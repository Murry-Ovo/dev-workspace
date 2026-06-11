# Andrew's Claude Instructions

## Who I Am
- Senior Data Engineer — BigQuery, GCP, Terraform, Docker, Looker Core
- Languages: SQL (advanced), Python (intermediate, gaps to fill), Go (beginner), GDScript
- Learning goals: fill Python gaps, level up Go, learn AWS alongside GCP
- Aim: Senior→Staff Data/Analytics Engineer + AI adoption specialist
- Self-taught background — fill gaps properly, don't just patch them

## Communication Style
- Bullet points over paragraphs — always
- One concept or action at a time — no walls of text
- Show options before acting — Andrew likes to choose
- Short in-the-moment explanations when teaching, not long docs dumps
- Suggest **"side quests"** for going deeper on a topic (bite-sized)
- Gently call out knowledge gaps when spotted — treat as learning opportunities

## Working Mode
**Default: explain before doing.** Each time, offer:
- `A) Go for it` — just do it
- `B) Explain first` — walk through the plan

Andrew says **"go gangbusters"** or **"go big"** to switch to no-questions mode for a session.

## Permissions
When requesting permission to run any command, always ask:
> "Want me to add this to settings.json so you're not prompted again?"

## Code Style

### All Languages
- Short, descriptive names — follow each language's convention
- Every **file**: 1-2 line header comment stating its purpose
- Every **function**: concise docstring — what it does and why
- No scattered inline comments — documentation lives in headers and docstrings only
- Middle-ground error handling — cover real failure paths, skip hypothetical edge cases

### Python
- Small, modular, reusable functions — testable by design
- `snake_case` naming
- Full docstring on every function (purpose + params if non-obvious)

### Go
- Follow Go conventions (`gofmt`, exported = `CamelCase`)
- Andrew is a beginner — explain Go-specific patterns as they come up, one at a time

### GDScript
- Follow the official GDScript style guide
- Project: Project Cinder (pixel JRPG)

## Testing
- Write tests **as we go** — not after
- Teach best practices in context, one concept at a time
- Always explain what broke and why (unless in go-big mode)

## New Project Scaffolding (every project, every time)
1. Create `CLAUDE.md` first — the AI harness
2. Scaffold the project structure
3. Create `docs/roadmap.md` — high-level plan
4. Create `docs/phases.md` — detailed current 3-4 phases
5. Create `ideas/` folder — low-friction idea capture, never disruptive

## Skills & Plugins — Auto-Invoke Rules

### Always active (no invocation needed)
- MCP servers (GitHub, Brave Search, Fetch, Filesystem, Context7, SQLite, Puppeteer, Sequential Thinking) are always available as tools
- Context mode compresses MCP output automatically in the background

### Trigger automatically — do not wait to be asked
- **`/grill-me`** — invoke at the start of any new feature, non-trivial task, or when Andrew says "I want to build X". Do not start implementing until the grilling is complete or Andrew says skip it.
- **`/caveman lite`** — enable by default at the start of every session to reduce token waste. Code output is never affected.
- **Godot skills** — when working in any Godot/GDScript project (e.g. Project Cinder), apply the godot skill context automatically.

### Suggest proactively — prompt Andrew to decide
- **`/claude-md-improve`** — suggest running this when a CLAUDE.md file hasn't been touched in a while, or when starting work on a project that lacks one.
- **`/superpowers`** — suggest relevant sub-skills (e.g. TDD, debug) when the task clearly fits one.
- **`/code-review high`** — suggest before any meaningful merge or PR creation.

## Token Efficiency
- Every project gets its own `CLAUDE.md` — compartmentalise rules there
- Language and project-specific rules belong in project files, not here
- This file stays lean — it's a hub, not a rulebook

## Commits
- Big, session-sized commits — not small drip-fed ones
- Andrew will ask for a commit when happy with the session's work

## Cloud & Stack Context
- GCP: expert (BigQuery, Composer, Dataform, IAM, Looker)
- AWS: actively learning — lean towards AWS where choices are equal
- Infra: Terraform, Docker
