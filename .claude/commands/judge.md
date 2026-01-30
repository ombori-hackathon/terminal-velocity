# Judge All Participants

Run a complete hackathon judging session for ALL participants in parallel.

## Usage
```
/judge [your-name]
```

## Input
Judge name: $ARGUMENTS

If no name provided, ask the user for their name before proceeding.

## CRITICAL: Hackathon Rules Compliance

Before scoring each participant, verify the submission follows hackathon rules:
- **Python backend** (FastAPI) - REQUIRED
- **SwiftUI macOS client OR Electron desktop app** - REQUIRED
- Must be a **desktop application**, NOT a web app
- Having additional components (web app, etc.) ON TOP of a working SwiftUI/Electron client is OK

Non-compliant submissions should receive reduced scores.

---

## Workflow

### Step 1: Setup
1. If no judge name provided in arguments, ask the user for their name
2. Create the output folder: `judging/reviews/[name]-ai-review/`

### Step 2: Get Participant List
Read the list of participants from `judging/participants/` directory.
Each participant folder is named `[participant-name]-ws`.

### Step 3: Spawn Parallel Judging Agents
For EACH participant, spawn a general-purpose agent using the Task tool.
**CRITICAL: Spawn ALL agents in a SINGLE message with multiple Task tool calls to run them in parallel.**

Each agent receives the FULL prompt template below and must:
1. Read the scoring matrix: `/Users/rossmalpass/Code/terminal-velocity-temp/judging/scoring-matrix.md`
2. Read the report template: `/Users/rossmalpass/Code/terminal-velocity-temp/judging/reviews/_template.md`
3. Analyze ALL THREE repos (workspace, frontend, backend)
4. Apply STRICT scoring criteria (see below)
5. Write report EXACTLY matching the template format

### Step 4: Generate Summary
After ALL agents complete, read all generated reports and create a `summary.md` file with:
- Rankings sorted by total score (highest to lowest)
- Quick comparison table
- Notable highlights across all submissions

### Step 5: Verify Summary
Spawn a verification agent to cross-check the summary against individual reviews.

---

## STRICT SCORING CRITERIA

### Base Score: 12 points maximum
### Bonus Points: Up to +2 for excellent Git workflow

---

### 1A. Compliance - Co-authoring (0-2 points)

**STRICT Calculation:**
- Exclude: Merge commits, initial template commits, submodule pointer updates
- Include: All other development commits

```
Effective % = Co-authored / (Total - Merges - Initial)
```

**Scoring (STRICT):**
| Score | Criteria |
|-------|----------|
| **2** | 95%+ co-authoring AND no evidence of IDE editing |
| **1.5** | 85-94% co-authoring |
| **1** | 70-84% co-authoring |
| **0.5** | 50-69% co-authoring |
| **0** | <50% OR clear IDE/manual editing evidence |

**Red flags that reduce score:**
- Commits with formatting-only changes (IDE auto-format)
- Multiple rapid small commits (manual editing pattern)
- Commits without meaningful messages

---

### 1B. Sophistication (0-3 points) - STRICT

#### Context Evolution (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | CLAUDE.md evolved with REAL learnings (not just template). Must show: specific gotchas, patterns discovered, or lessons learned during development. |
| **0** | Template-only content OR no meaningful evolution |

**To get this point, CLAUDE.md must contain SPECIFIC learnings like:**
- "We discovered that X pattern works better than Y because..."
- "Gotcha: When doing Z, make sure to..."
- NOT generic statements like "This project uses SwiftUI"

#### Sub-agents (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | Custom agents in `.claude/agents/` with SPECIFIC roles and model assignments |
| **0** | No agents OR only copied template agents without customization |

#### Planning Files (0 or 1 point)
| Score | Criteria |
|-------|----------|
| **1** | Actual spec/planning files with REAL content (not empty templates) |
| **0** | No specs OR empty/template-only spec files |

---

### 1C. Guardrails (0-1 point) - STRICT

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

### 2A. Architecture (0-2 points)

| Score | Criteria |
|-------|----------|
| **2** | Clean separation, proper workspace + submodules, logical organization, follows platform conventions |
| **1.5** | Good structure with minor issues |
| **1** | Basic structure, some messiness |
| **0.5** | Poor organization |
| **0** | Chaotic or missing structure |

---

### 2B. Code Quality (0-2 points)

| Score | Criteria |
|-------|----------|
| **2** | Excellent: proper error handling, type safety, follows conventions, no obvious bugs |
| **1.5** | Good with minor issues |
| **1** | Functional but rough |
| **0.5** | Poor practices |
| **0** | Buggy or broken |

---

### 2C. Functionality (0-1 point)

| Score | Criteria |
|-------|----------|
| **1** | App runs end-to-end, frontend connects to backend |
| **0.5** | Partially working |
| **0** | Doesn't run or major features broken |

---

### 2D. Documentation (0-1 point)

| Score | Criteria |
|-------|----------|
| **1** | Good README with setup instructions, code comments where needed |
| **0.5** | Minimal documentation |
| **0** | No documentation or unusable |

---

## BONUS POINTS: Git Workflow (+0 to +2)

Award bonus points for professional Git practices:

| Bonus | Criteria |
|-------|----------|
| **+1** | Used feature branches (not all commits to main) |
| **+0.5** | Created PRs with descriptions (check with `gh pr list`) |
| **+0.5** | Used gh CLI for git operations (evidence in commit patterns) |

**Maximum bonus: +2 points**
**Final score: Base (0-12) + Bonus (0-2) = 0-14 possible**

---

## Agent Prompt Template

For each participant, use this EXACT prompt:

```
You are judging hackathon participant "[PARTICIPANT]".

## REQUIRED READING (do this first)
1. Read scoring matrix: /Users/rossmalpass/Code/terminal-velocity-temp/judging/scoring-matrix.md
2. Read report template: /Users/rossmalpass/Code/terminal-velocity-temp/judging/reviews/_template.md

## REPO PATH
/Users/rossmalpass/Code/terminal-velocity-temp/judging/participants/[PARTICIPANT]-ws/

## CHECK ALL THREE REPOS
- WORKSPACE: [PARTICIPANT]-ws/ (root)
- FRONTEND: [PARTICIPANT]-ws/apps/macos-client/ OR similar (submodule)
- BACKEND: [PARTICIPANT]-ws/services/api/ OR similar (submodule)

## STRICT SCORING - BE CRITICAL

You must be STRICT. Most submissions should NOT get perfect scores.

### Co-authoring Analysis (run in EACH repo):
```bash
cd [repo] && echo "=== $(pwd) ===" && \
echo "Total:" && git log --oneline | wc -l && \
echo "Co-authored:" && git log --format="%B" | grep -ci "co-authored-by.*claude" && \
echo "Recent commits:" && git log --oneline -10
```

### PR/Branch Analysis:
```bash
cd [repo] && echo "=== Branches ===" && git branch -a && \
echo "=== Check for PRs ===" && git log --oneline | grep -i "merge\|pr\|#"
```

### Deduct points for:
- CLAUDE.md that's just template content (no real learnings)
- Agents copied from template without customization
- Empty spec files or planning docs
- Single guardrail instead of multiple
- Direct pushes to main instead of PRs

## OUTPUT

Write report to: /Users/rossmalpass/Code/terminal-velocity-temp/judging/reviews/[JUDGE]-ai-review/[PARTICIPANT].md

**CRITICAL: Follow the template EXACTLY. Copy the structure from _template.md**

Your report MUST include:
1. Quick Summary table with scores at TOP
2. Critical Issues section
3. Points Lost table (be specific about WHY points were lost)
4. Git Workflow table showing branch/PR usage
5. Bonus points calculation

Date for report: [TODAY'S DATE]
Reviewer: [JUDGE]
```

---

## Template Structure (MANDATORY)

Reports MUST follow this exact structure from `judging/reviews/_template.md`:

```markdown
# [Participant Name] - Hackathon Judging Report

**Reviewer:** [Name] | **Date:** [YYYY-MM-DD] | **Score:** [X/12] (+Y bonus)

---

## Quick Summary

| Category | Score | Notes |
|----------|-------|-------|
| 1A. Compliance | X/2 | [one-liner] |
| 1B. Sophistication | X/3 | [one-liner] |
| 1C. Guardrails | X/1 | [one-liner] |
| **Rules** | **X/6** | |
| 2A. Architecture | X/2 | [one-liner] |
| 2B. Code Quality | X/2 | [one-liner] |
| 2C. Functionality | X/1 | [one-liner] |
| 2D. Documentation | X/1 | [one-liner] |
| **Quality** | **X/6** | |
| **BASE TOTAL** | **X/12** | |
| **Bonus** | **+X** | [Git workflow] |
| **FINAL** | **X/14** | |

---

## Critical Issues (if any)

> [Red flags - Write "None" if no issues]

---

## Points Lost

| Category | Lost | Reason |
|----------|------|--------|
| [1A] | [-X] | [specific reason] |

**Total Lost: X points**

---

## Detailed Analysis

### 1A. Compliance (X/2)

**Co-authoring by Repo:**

| Repo | Dev Commits | Co-authored | Rate |
|------|-------------|-------------|------|
| Workspace | X | X | X% |
| Frontend | X | X | X% |
| Backend | X | X | X% |
| **Total** | **X** | **X** | **X%** |

[Explanation of any issues]

**Git Workflow (Bonus):**

| Practice | Status | Bonus |
|----------|--------|-------|
| Feature branches | ✅/❌ | +1/+0 |
| PRs with descriptions | ✅/❌ | +0.5/+0 |
| gh CLI usage | ✅/❌ | +0.5/+0 |
| **Bonus Total** | | **+X** |

### 1B. Sophistication (X/3)

- **Context Evolution (X/1):** [Specific evidence or why 0]
- **Sub-agents (X/1):** [Specific evidence or why 0]
- **Planning Files (X/1):** [Specific evidence or why 0]

### 1C. Guardrails (X/1)

- [x] / [ ] Pre-commit hooks - [details]
- [x] / [ ] Linting config - [details]
- [x] / [ ] Tests - [details]
- [x] / [ ] CI/CD - [details]

**Guardrails configured: X/4 (need 2+ for full point)**

### 2A. Architecture (X/2)
[Analysis]

### 2B. Code Quality (X/2)
**Swift:** [analysis]
**Python:** [analysis]

### 2C. Functionality (X/1)
[Analysis]

### 2D. Documentation (X/1)
[Analysis]

---

## Highlights
1. **[Topic]** - [Description]
2. **[Topic]** - [Description]
3. **[Topic]** - [Description]

## Concerns
1. **[Topic]** - [Description]
2. **[Topic]** - [Description]
3. **[Topic]** - [Description]

---

## Creativity Notes
[For human judges]

---

*Report generated using template v1.0*
```

---

## Participants List

Expected in `judging/participants/`:
- agentarium-ws
- antisocial-ws (or antisocial-workspace)
- anttuii-ws
- crucible-collective-ws
- csaba-ombori-hack-ws
- friction-log-ws
- hackathon-app-ws
- hackathon-lukas-ws
- kafeel-ws
- make-homer-proud-timer-ws
- mini-mate-1-ws
- neatdog-ws
- oculog-ws
- phystone-ws
- pulsync-ws
- spotdrop-ws (or spotdrop-workspace)
- team-awesome-ws
- team-bellard-ws
- teamfred-ws
- your-3d-print-decision-buddy-ws
