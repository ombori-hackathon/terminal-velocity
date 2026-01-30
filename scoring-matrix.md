# Hackathon Judging Scoring Matrix

## Overview
| Category | Points | Judges |
|----------|--------|--------|
| Creativity & Presentation | 4 | Everyone votes |
| Following the Rules | 3 | Leads (Andreas, Kamil, Ross, Abdallah, Tom, Judit) |
| Codebase Quality | 3 | Leads |
| **TOTAL** | **10** | |

**DO NOT SCORE:** "terminal-velocity" - this is the setup template repo

---

## Category 1: Following the Rules (3 points max)

### 1A. Compliance (0 - 0.75 points)
Did they follow the hackathon constraints?

| Score | Criteria |
|-------|----------|
| **0.75** | ~90%+ commits co-authored with Claude, gh CLI used for git ops, no IDE evidence |
| **0.4** | Majority co-authored but noticeable gaps, or minor IDE evidence |
| **0** | Clear evidence of manual editing, IDE usage, or most commits lack co-author |

**How to verify:**
- Check git log for `Co-Authored-By: Claude` on commits (expect ~90%+, not 100%)
- Look for commit patterns (IDE auto-saves, manual formatting changes)
- Check if gh CLI was used (PR descriptions, branch patterns)

---

### 1B. Claude Code Sophistication (0 - 1.5 points)

#### Context Evolution & Distribution (0 - 0.6)

**Prerequisite:** Context must have actually evolved during the project. If claude.md and agents are unchanged from template, score 0 for this entire subsection.

| Score | Criteria |
|-------|----------|
| **0.6** | Context evolved AND distributed smartly: lean claude.md with task-specific learnings pushed to agents/skills (best practice) |
| **0.45** | Context evolved AND some distribution to agents, but claude.md still doing heavy lifting |
| **0.3** | Context evolved but all in claude.md (bloated main file, no agent delegation) |
| **0** | No evolution from template - learnings didn't feed back into context |

**What to look for:**
- `.claude/commands/` for custom skills with embedded context
- Agent-specific markdown files with task learnings
- Lean claude.md that references agents rather than containing everything
- Evidence of iterative updates (git history of context files)

#### Sub-agents / Multi-agent Usage (0 - 0.45)
| Score | Criteria |
|-------|----------|
| **0.45** | Custom sub-agent configurations, clear delegation patterns |
| **0.2** | Some sub-agent usage evident |
| **0** | No sub-agent usage |

#### Planning Files / Spec Files (0 - 0.45)
| Score | Criteria |
|-------|----------|
| **0.45** | Evidence of plan mode usage (planning.md, spec files, architectural docs) |
| **0.2** | Some planning artifacts |
| **0** | No evidence of plan-first approach |

---

### 1C. Guardrails & Automation (0 - 0.75 points)

| Score | Criteria |
|-------|----------|
| **0.75** | Pre-commit hooks, linting, formatters, tests - all configured via Claude |
| **0.4** | Some guardrails in place |
| **0** | No guardrails beyond default setup |

**What to look for:**
- `.pre-commit-config.yaml`
- Linting configs (swiftlint, pylint/ruff/black)
- Test files and test commands
- CI/CD workflows

---

## Category 2: Codebase Quality (3 points max)

### 2A. Architecture & Structure (0 - 0.9 points)

| Score | Criteria |
|-------|----------|
| **0.9** | Clean separation of concerns, proper workspace + submodule structure, logical file organization |
| **0.45** | Decent structure but some messiness |
| **0** | Chaotic, no clear organization |

**Check:**
- Workspace properly set up with macos/desktop and api submodules
- Clear separation between Swift frontend and Python backend
- Logical folder structure within each

---

### 2B. Code Quality (0 - 0.9 points)

| Score | Criteria |
|-------|----------|
| **0.9** | Clean code, proper error handling, no obvious bugs, follows language conventions |
| **0.45** | Functional but rough edges |
| **0** | Buggy, poor practices, doesn't follow conventions |

**Check:**
- Swift: proper optionals handling, SwiftUI best practices
- Python: type hints, proper exception handling, follows PEP8

---

### 2C. Functionality (0 - 0.6 points)

| Score | Criteria |
|-------|----------|
| **0.6** | App works end-to-end (frontend talks to backend, core features work) |
| **0.3** | Partially functional |
| **0** | Doesn't run or major broken features |

---

### 2D. Documentation & Maintainability (0 - 0.6 points)

| Score | Criteria |
|-------|----------|
| **0.6** | Good README, setup instructions, code comments where needed |
| **0.3** | Basic README or some documentation |
| **0** | No documentation |

---

## Quick Checklist

### Repo Structure
- [ ] Workspace with 2 submodules (macos/desktop, api)
- [ ] Each submodule has its own claude.md
- [ ] Root workspace has claude.md

### Git/GitHub
- [ ] Co-authored commits with Claude
- [ ] Feature branches used
- [ ] PRs created (not just direct pushes to main)
- [ ] Meaningful commit messages

### Claude Code Artifacts
- [ ] Context actually evolved (check git history of claude.md and agent files)
- [ ] Learnings fed back into context (not just boilerplate)
- [ ] Context distributed to agents/skills (not all in bloated claude.md)
- [ ] Custom commands in `.claude/commands/` with task-specific context
- [ ] Planning/spec markdown files present
- [ ] Sub-agent configurations with embedded learnings

### Guardrails
- [ ] Pre-commit hooks configured
- [ ] Linting setup
- [ ] Tests present
- [ ] Docker compose for local services

### Code Quality
- [ ] README exists and is helpful
- [ ] App runs successfully
- [ ] Frontend connects to backend
- [ ] Error handling present

---

## Rule Violation Policy

**Approach:** Deduct points proportionally, not full disqualification

| Violation Level | Action |
|-----------------|--------|
| Minor (~10-25% commits without co-author) | Small deduction in Compliance |
| Moderate (>25% without co-author, or some IDE evidence) | Larger deduction, still score sophistication |
| Major (majority manual commits, clearly didn't use Claude Code) | Heavy deductions across "Following Rules" |

---

## Scoring Sheet

| Participant | 1A (0.75) | 1B (1.5) | 1C (0.75) | **Rules (3)** | 2A (0.9) | 2B (0.9) | 2C (0.6) | 2D (0.6) | **Quality (3)** | **Leads Total (6)** |
|-------------|-----------|----------|-----------|---------------|----------|----------|----------|----------|-----------------|---------------------|
|             |           |          |           |               |          |          |          |          |                 |                     |
|             |           |          |           |               |          |          |          |          |                 |                     |
|             |           |          |           |               |          |          |          |          |                 |                     |
|             |           |          |           |               |          |          |          |          |                 |                     |
|             |           |          |           |               |          |          |          |          |                 |                     |

*Note: Creativity & Presentation (4 pts) scored separately by everyone. Final total out of 10.*

---

## Tiebreakers (if needed)

- Creative use of Claude Code features not in template
- Evidence of iterative learning in claude.md
- Sophisticated error recovery patterns documented
- MCP server setup or advanced integrations
