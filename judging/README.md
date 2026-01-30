# Hackathon Judging Guide

This guide explains how to use Claude Code to judge hackathon submissions.

## Quick Start

1. **Run the judge command with your name**
   ```
   /judge [your-name]
   ```
   Example: `/judge ross`

2. **Wait for parallel analysis**
   - Claude will spawn agents to analyze ALL participants simultaneously
   - Each participant gets a full 5-perspective analysis

3. **Review the generated reports**
   - Individual reports: `judging/reviews/[your-name]-ai-review/[participant].md`
   - Summary with rankings: `judging/reviews/[your-name]-ai-review/summary.md`

---

## Folder Structure

```
judging/
├── README.md              # This file
├── scoring-matrix.md      # Detailed scoring criteria
├── participants/          # Cloned participant repos (snapshot at submission)
│   ├── agentarium-ws/
│   ├── antisocial-ws/
│   └── ...
└── reviews/
    └── [name]-ai-review/  # Judge review folders
        ├── agentarium.md
        ├── antisocial.md
        └── summary.md
```

---

## The /judge Command

The `/judge` command orchestrates a complete judging session:

1. Creates your review folder: `judging/reviews/[name]-ai-review/`
2. Lists all participants from `judging/participants/`
3. Spawns parallel agents (one per participant) that each run 5 specialist analyses:
   - **Claude Code Analyst** - 1A Compliance + 1B Sophistication
   - **Swift/macOS Analyst** - 2A Architecture + 2B Code Quality (Swift)
   - **Python/API Analyst** - 2A Architecture + 2B Code Quality (Python)
   - **GitHub Analyst** - 1A Compliance + 2D Documentation
   - **DevOps Analyst** - 1C Guardrails + 2C Functionality
4. Generates a summary with rankings after all agents complete

---

## Scoring Categories

### Rules Categories (6 points max)

| Category | Max | What to Look For |
|----------|-----|------------------|
| **1A. Compliance** | 2 | Co-authored commits, customized CLAUDE.md |
| **1B. Sophistication** | 3 | Context evolution (1), Sub-agents (1), Planning files (1) |
| **1C. Guardrails** | 1 | Pre-commit hooks, linting, tests configured |

### Quality Categories (6 points max)

| Category | Max | What to Look For |
|----------|-----|------------------|
| **2A. Architecture** | 2 | Clean separation, proper patterns |
| **2B. Code Quality** | 2 | Type hints, error handling, modern patterns |
| **2C. Functionality** | 1 | Working app, Docker, tests pass |
| **2D. Documentation** | 1 | README, CLAUDE.md, API docs |

### Creativity (8 points - Human Judged)

Not scored by AI. Human judges award 0-8 points for:
- Innovation and uniqueness
- Ambition of the project
- Polish and presentation
- Real-world usefulness

---

## Hackathon Rules Compliance

**CRITICAL**: Verify each submission follows the rules:

- **Python backend** (FastAPI) - REQUIRED
- **SwiftUI macOS client** OR **Electron desktop app** - REQUIRED
- Must be a **desktop application**, NOT a web-only app
- Having additional components (web app) ON TOP OF SwiftUI/Electron is OK

Non-compliant submissions should receive reduced scores.

---

## Individual Agent Commands

For ad-hoc single-participant analysis, you can run individual specialist agents:

```
/judge-claude-code judging/participants/[name]-ws
/judge-swift judging/participants/[name]-ws
/judge-python judging/participants/[name]-ws
/judge-github judging/participants/[name]-ws
/judge-devops judging/participants/[name]-ws
```

---

## Tips for Judges

1. **Context Evolution is Key**
   - Look for git commits that MODIFIED CLAUDE.md after initial setup
   - A customized template created once does NOT count as evolution
   - Best submissions show learnings added during development

2. **Guardrails Must Be Configured**
   - Listing `ruff` in dependencies is NOT enough
   - Need actual `.pre-commit-config.yaml` or `ruff.toml`
   - Empty test directories don't count

3. **Check Submodules**
   - Most projects have `apps/macos-client/` and `services/api/` as submodules
   - Run `git submodule update --init --recursive` if needed

4. **Trust but Verify**
   - AI scores are a starting point
   - Read a few key files yourself for important decisions
   - Creativity scoring is entirely human judgment

---

## Participant List

Current participants in `judging/participants/`:
- agentarium
- antisocial
- anttuii
- crucible-collective
- csaba-ombori-hack
- hackathon-app
- hackathon-lukas
- kafeel
- make-homer-proud-timer
- mini-mate-1
- neatdog
- oculog
- phystone
- pulsync
- spotdrop
- team-awesome
- team-bellard
- teamfred
- your-3d-print-decision-buddy

---

## Example Session

```bash
# Start Claude Code in the terminal-velocity-temp directory
cd ~/Code/terminal-velocity-temp
claude

# Run the judge command
> /judge ross

# Wait for all agents to complete...
# Review the summary
> Read judging/reviews/ross-ai-review/summary.md

# Check individual reports as needed
> Read judging/reviews/ross-ai-review/kafeel.md
```
