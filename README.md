# Terminal Velocity

Welcome to the Ombori Hackathon! This repo contains everything you need to get started.

**Why "Terminal Velocity"?** We're going terminal-only. No IDE. Let Claude Code do the work. This hackathon is about increasing our output velocity with AI agents - and getting comfortable with a new way of building software.

---

## The Rules

| ✅ Allowed | ❌ Not Allowed |
|-----------|---------------|
| Claude Code in terminal | VS Code / any IDE |
| Xcode to edit project settings (not code) | Manually using terminal, git commit, brew, anything |
| Screenshots for feedback | Manually editing code files |
| gh CLI for git operations | Copy-pasting code snippets |
| Asking Claude questions | Using GitHub web interface |

**The goal:** Let Claude do everything. You describe, Claude builds.

---

## What You'll Build

Each team creates their own:
- **Swift CLI client** - macOS command-line app
- **FastAPI backend** - Python API with PostgreSQL
- **Claude Code workspace** - AI-powered development environment

## Getting Started

### 1. Create a folder for your project (this becomes your workspace)

```bash
mkdir ~/Desktop/my-team-name
cd ~/Desktop/my-team-name
```

### 2. Open Claude Code

```bash
claude
```

### 3. Paste the setup prompt

```
Paste the contents of setup.md into claude code
```

### 4. Answer the project name question

When asked, provide your team name (lowercase, hyphens ok). Example: `team-awesome`

### 5. Wait for setup to complete

Claude Code will automatically:
- Install any missing tools (Homebrew, Docker, Swift, Python, etc.)
- Create your project structure
- Set up 3 GitHub repos in the ombori-hackathon org
- Configure Claude Code agents for your workflow

## After Setup

Your workspace will have:
- `docker compose up -d` - Start the database
- `cd services/api && uv run fastapi dev` - Run the API
- `cd apps/macos-client && swift run` - Run the Swift client

Use `/architect` to design your first feature!

## Need Help?

- Check the generated `CLAUDE.md` in your workspace
- Use `/debugger` agent for troubleshooting
- Ask the hackathon organizers

Happy hacking!
