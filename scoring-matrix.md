# Hackathon Judging Scoring Matrix

## Overview
| Category | Points | Judges |
|----------|--------|--------|
| Following the Rules | 6 | Leads (Andreas, Kamil, Ross, Abdallah, Tom, Judit) |
| Codebase Quality | 6 | Leads |
| Creativity & Presentation | 8 | Everyone votes |
| **TOTAL** | **20** | |

**DO NOT SCORE:** "terminal-velocity" - this is the setup template repo

---

## Category 1: Following the Rules (6 points max)

### 1A. Compliance (0 - 2 points)
Did they follow the hackathon constraints?

| Score | Criteria |
|-------|----------|
| **2** | ~90%+ commits co-authored with Claude, gh CLI used for git ops, no IDE evidence |
| **1** | Majority co-authored but noticeable gaps, or minor IDE evidence |
| **0** | Clear evidence of manual editing, IDE usage, or most commits lack co-author |

**How to verify:**
- Check git log for `Co-Authored-By: Claude` on commits (expect ~90%+, not 100%)
- Look for commit patterns (IDE auto-saves, manual formatting changes)
- Check if gh CLI was used (PR descriptions, branch patterns)

---

### 1B. Claude Code Sophistication (0 - 3 points)

Award 1 point for each of the following (binary scoring):

#### Context Evolution & Distribution (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | Context evolved AND distributed smartly: lean claude.md with task-specific learnings pushed to agents/skills |
| **0** | No evolution from template OR all context bloated in claude.md (no agent delegation) |

**What to look for:**
- `.claude/commands/` for custom skills with embedded context
- Agent-specific markdown files with task learnings
- Lean claude.md that references agents rather than containing everything
- Evidence of iterative updates (git history of context files)

#### Sub-agents / Multi-agent Usage (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | Custom sub-agent configurations, clear delegation patterns |
| **0** | No sub-agent usage |

#### Planning Files / Spec Files (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | Evidence of plan mode usage (planning.md, spec files, architectural docs) |
| **0** | No evidence of plan-first approach |

---

### 1C. Guardrails & Automation (0 - 1 point)

| Score | Criteria |
|-------|----------|
| **1** | Pre-commit hooks, linting, formatters, or tests configured via Claude |
| **0** | No guardrails beyond default setup |

**What to look for:**
- `.pre-commit-config.yaml`
- Linting configs (swiftlint, pylint/ruff/black)
- Test files and test commands
- CI/CD workflows

---

## Category 2: Codebase Quality (6 points max)

### 2A. Architecture & Structure (0 - 2 points)

| Score | Criteria |
|-------|----------|
| **2** | Clean separation of concerns, proper workspace + submodule structure, logical file organization |
| **1** | Decent structure but some messiness |
| **0** | Chaotic, no clear organization |

**Check:**
- Workspace properly set up with macos/desktop and api submodules
- Clear separation between Swift frontend and Python backend
- Logical folder structure within each

---

### 2B. Code Quality (0 - 2 points)

| Score | Criteria |
|-------|----------|
| **2** | Clean code, proper error handling, no obvious bugs, follows language conventions |
| **1** | Functional but rough edges |
| **0** | Buggy, poor practices, doesn't follow conventions |

**Check:**
- Swift: proper optionals handling, SwiftUI best practices
- Python: type hints, proper exception handling, follows PEP8

---

### 2C. Functionality (0 - 1 point)

| Score | Criteria |
|-------|----------|
| **1** | App works end-to-end (frontend talks to backend, core features work) |
| **0** | Doesn't run or major broken features |

---

### 2D. Documentation (0 - 1 point)

| Score | Criteria |
|-------|----------|
| **1** | Good README, setup instructions, code comments where needed |
| **0** | No documentation or unusable README |

---

## Category 3: Creativity & Presentation (8 points max)

*Scored by everyone*

| Score | Criteria |
|-------|----------|
| **8** | Exceptional - wow factor, innovative use of constraints |
| **6** | Strong - creative approach, well presented |
| **4** | Good - solid work, decent presentation |
| **2** | Basic - meets minimum, minimal creativity |
| **0** | Poor - lacks effort or creativity |

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

| Participant | 1A<br>(2) | 1B<br>(3) | 1C<br>(1) | Rules<br>(6) | 2A<br>(2) | 2B<br>(2) | 2C<br>(1) | 2D<br>(1) | Quality<br>(6) | Creativity<br>(8) | TOTAL<br>(20) |
|-------------|-----------|-----------|-----------|--------------|-----------|-----------|-----------|-----------|----------------|-------------------|---------------|
| agentarium |        |        |        |               |        |        |        |        |                 |                    |                    |
| antisocial |        |        |        |               |        |        |        |        |                 |                    |                    |
| anttuii |        |        |        |               |        |        |        |        |                 |                    |                    |
| crucible-collective |        |        |        |               |        |        |        |        |                 |                    |                    |
| csaba-ombori-hack |        |        |        |               |        |        |        |        |                 |                    |                    |
| hackathon-app |        |        |        |               |        |        |        |        |                 |                    |                    |
| hackathon-lukas |        |        |        |               |        |        |        |        |                 |                    |                    |
| kafeel |        |        |        |               |        |        |        |        |                 |                    |                    |
| make-homer-proud-timer |        |        |        |               |        |        |        |        |                 |                    |                    |
| mini-mate-1 |        |        |        |               |        |        |        |        |                 |                    |                    |
| neatdog |        |        |        |               |        |        |        |        |                 |                    |                    |
| oculog |        |        |        |               |        |        |        |        |                 |                    |                    |
| phystone |        |        |        |               |        |        |        |        |                 |                    |                    |
| pulsync |        |        |        |               |        |        |        |        |                 |                    |                    |
| spotdrop |        |        |        |               |        |        |        |        |                 |                    |                    |
| team-awesome |        |        |        |               |        |        |        |        |                 |                    |                    |
| team-bellard |        |        |        |               |        |        |        |        |                 |                    |                    |
| teamfred |        |        |        |               |        |        |        |        |                 |                    |                    |
| your-3d-print-decision-buddy |        |        |        |               |        |        |        |        |                 |                    |                    |

---

## Tiebreakers (if needed)

- Creative use of Claude Code features not in template
- Evidence of iterative learning in claude.md
- Sophisticated error recovery patterns documented
- MCP server setup or advanced integrations
