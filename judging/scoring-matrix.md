# Hackathon Judging Scoring Matrix

## Overview

| Category | Points | Scored By |
|----------|--------|-----------|
| Following the Rules | 6 | AI + Leads verify |
| Codebase Quality | 6 | AI + Leads verify |
| Git Workflow Bonus | +2 | AI |
| **AI Total** | **14** | |
| Creativity & Presentation | 8 | Everyone votes |
| **COMBINED TOTAL** | **22** | |

**DO NOT SCORE:** "terminal-velocity" - this is the setup template repo

---

## Category 1: Following the Rules (6 points max)

### 1A. Compliance - Co-authoring (0-2 points)

Did they follow the hackathon constraints?

**Calculation:**
- Exclude: Merge commits, initial template commits, submodule pointer updates
- Include: All other development commits

```
Effective % = Co-authored / (Total - Merges - Initial)
```

| Score | Criteria |
|-------|----------|
| **2** | 95%+ co-authoring AND no evidence of IDE editing |
| **1.5** | 85-94% co-authoring |
| **1** | 70-84% co-authoring |
| **0.5** | 50-69% co-authoring |
| **0** | <50% OR clear IDE/manual editing evidence |

**How to verify:**
- Check git log for `Co-Authored-By: Claude` on commits
- Look for commit patterns (IDE auto-saves, manual formatting changes)
- Check if gh CLI was used (PR descriptions, branch patterns)

**Red flags that reduce score:**
- Commits with formatting-only changes (IDE auto-format)
- Multiple rapid small commits (manual editing pattern)
- Commits without meaningful messages

---

### 1B. Claude Code Sophistication (0-3 points)

Award 1 point for each of the following (binary scoring):

#### Context Evolution (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | CLAUDE.md evolved with REAL learnings (not just template). Must show: specific gotchas, patterns discovered, or lessons learned during development. |
| **0** | Template-only content OR no meaningful evolution |

**To get this point, CLAUDE.md must contain SPECIFIC learnings like:**
- "We discovered that X pattern works better than Y because..."
- "Gotcha: When doing Z, make sure to..."
- NOT generic statements like "This project uses SwiftUI"

**What to look for:**
- `.claude/commands/` for custom skills with embedded context
- Agent-specific markdown files with task learnings
- Lean claude.md that references agents rather than containing everything
- Evidence of iterative updates (git history of context files)

#### Sub-agents / Multi-agent Usage (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | Custom agents in `.claude/agents/` with SPECIFIC roles and model assignments |
| **0** | No agents OR only copied template agents without customization |

#### Planning Files / Spec Files (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | Actual spec/planning files with REAL content (not empty templates) |
| **0** | No specs OR empty/template-only spec files |

---

### 1C. Guardrails & Automation (0-1 point)

Must have AT LEAST TWO of the following configured AND working:
- `.pre-commit-config.yaml` with actual hooks
- Linting config (swiftlint.yml, ruff.toml) with custom rules
- Test files that actually run (not just empty test files)
- CI/CD workflows in `.github/workflows/`

| Score | Criteria |
|-------|----------|
| **1** | 2+ guardrails properly configured |
| **0.5** | Only 1 guardrail configured |
| **0** | No guardrails or only empty configs |

---

## Category 2: Codebase Quality (6 points max)

### 2A. Architecture & Structure (0-2 points)

| Score | Criteria |
|-------|----------|
| **2** | Clean separation of concerns, proper workspace + submodule structure, logical file organization |
| **1.5** | Good structure with minor issues |
| **1** | Decent structure but some messiness |
| **0.5** | Poor organization |
| **0** | Chaotic, no clear organization |

**Check:**
- Workspace properly set up with macos/desktop and api submodules
- Clear separation between Swift frontend and Python backend
- Logical folder structure within each

---

### 2B. Code Quality (0-2 points)

| Score | Criteria |
|-------|----------|
| **2** | Excellent: proper error handling, type safety, follows conventions, no obvious bugs |
| **1.5** | Good with minor issues |
| **1** | Functional but rough edges |
| **0.5** | Poor practices |
| **0** | Buggy, poor practices, doesn't follow conventions |

**Check:**
- Swift: proper optionals handling, SwiftUI best practices
- Python: type hints, proper exception handling, follows PEP8

---

### 2C. Functionality (0-1 point)

| Score | Criteria |
|-------|----------|
| **1** | App works end-to-end (frontend talks to backend, core features work) |
| **0.5** | Partially working |
| **0** | Doesn't run or major broken features |

---

### 2D. Documentation (0-1 point)

| Score | Criteria |
|-------|----------|
| **1** | Good README with setup instructions, code comments where needed |
| **0.5** | Minimal documentation |
| **0** | No documentation or unusable README |

---

## Bonus: Git Workflow (+0 to +2)

Award bonus points for professional Git practices:

| Bonus | Criteria |
|-------|----------|
| **+1** | Used feature branches (not all commits to main) |
| **+0.5** | Created PRs with descriptions |
| **+0.5** | Used gh CLI for git operations (evidence in commit patterns) |

**Maximum bonus: +2 points**

---

## Category 3: Creativity & Presentation (8 points max)

*Scored by everyone (human judges)*

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

| Participant | 1A | 1B | 1C | Rules | 2A | 2B | 2C | 2D | Quality | Bonus | AI Total | Creativity | FINAL |
|-------------|----|----|----|----|----|----|----|----|---------|-------|----------|------------|-------|
| agentarium | | | | | | | | | | | | | |
| anttuii | | | | | | | | | | | | | |
| crucible-collective | | | | | | | | | | | | | |
| csaba-ombori-hack | | | | | | | | | | | | | |
| friction-log | | | | | | | | | | | | | |
| hackathon-lukas | | | | | | | | | | | | | |
| kafeel | | | | | | | | | | | | | |
| make-homer-proud-timer | | | | | | | | | | | | | |
| mini-mate-1 | | | | | | | | | | | | | |
| neatdog | | | | | | | | | | | | | |
| oculog | | | | | | | | | | | | | |
| phystone | | | | | | | | | | | | | |
| pulsync | | | | | | | | | | | | | |
| spotdrop | | | | | | | | | | | | | |
| team-awesome | | | | | | | | | | | | | |
| team-bellard | | | | | | | | | | | | | |
| teamfred | | | | | | | | | | | | | |
| your-3d-print-decision-buddy | | | | | | | | | | | | | |

---

## Tiebreakers (if needed)

- Creative use of Claude Code features not in template
- Evidence of iterative learning in claude.md
- Sophisticated error recovery patterns documented
- MCP server setup or advanced integrations
