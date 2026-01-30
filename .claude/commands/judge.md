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

## Workflow

### Step 1: Setup
1. If no judge name provided in arguments, ask the user for their name
2. Create the output folder: `judging/reviews/[name]-ai-review/`

### Step 2: Get Participant List
Read the list of participants from `judging/participants/` directory.
Each participant folder is named `[participant-name]-ws`.

### Step 3: Spawn Parallel Agents
For EACH participant, spawn a general-purpose agent using the Task tool.
**CRITICAL: Spawn ALL agents in a SINGLE message with multiple Task tool calls to run them in parallel.**

Each agent should:
1. Analyze the participant repo at `judging/participants/[name]-ws/`
2. Run the 5 specialist perspectives (Claude Code, Swift, Python, GitHub, DevOps)
3. Generate a comprehensive report following the scorecard format below
4. Write the report to `judging/reviews/[judge-name]-ai-review/[participant-name].md`

### Step 4: Generate Summary
After ALL agents complete, read all generated reports and create a `summary.md` file with:
- Rankings sorted by total score (highest to lowest)
- Quick comparison table
- Notable highlights across all submissions

### Step 5: Verify Summary
Spawn a verification agent to cross-check the summary against individual reviews.

The verification agent should:
1. Read the summary.md
2. Read EACH individual review report
3. Check for inconsistencies:
   - Do the scores in summary match the individual reports?
   - Are claims like "only submission with X" actually true across all reports?
   - Are rankings correct based on actual scores?
4. If inconsistencies found, update summary.md with corrections
5. Write a brief `verification-notes.md` listing any corrections made

## Agent Prompt Template

For each participant, use this prompt when spawning the agent:

```
Analyze hackathon submission for participant "[PARTICIPANT]" and write a judging report.

REPO PATH: /Users/rossmalpass/Code/terminal-velocity-temp/judging/participants/[PARTICIPANT]-ws/

SCORING REFERENCE: Read /Users/rossmalpass/Code/terminal-velocity-temp/judging/scoring-matrix.md for detailed criteria.

CRITICAL: CHECK ALL THREE REPOS
Most submissions have a workspace repo with two submodules. You MUST check ALL of them:
- WORKSPACE: [PARTICIPANT]-ws/ (root)
- FRONTEND: [PARTICIPANT]-ws/apps/macos-client/ OR similar (submodule)
- BACKEND: [PARTICIPANT]-ws/services/api/ OR similar (submodule)

For EACH repo, check for CLAUDE.md, .claude/commands/, git history, etc.

ANALYSIS TASKS:
1. **Claude Code Analysis** (1A Compliance + 1B Sophistication - 5 pts)

   FOR EACH OF THE 3 REPOS (workspace, frontend, backend):
   - Check if CLAUDE.md exists and read it
   - Check git log for CLAUDE.md changes: `git log --oneline -- CLAUDE.md` or `git log --oneline -- claude.md`
   - Look for .claude/commands/ custom skills
   - Find planning/spec files (*.md files with plans, specs, architecture)
   - Check commit co-authoring: `git log --format="%s%n%b" | grep -c "Co-Authored-By: Claude"` vs total commits

   Evolution means CLAUDE.md was MODIFIED after initial creation - look for multiple commits touching it.

2. **Swift/macOS Analysis** (2A + 2B partial)
   - Analyze apps/macos-client or similar structure
   - Evaluate SwiftUI patterns, state management
   - Check code quality

3. **Python/API Analysis** (2A + 2B partial)
   - Analyze services/api structure
   - Check FastAPI patterns, type hints
   - Evaluate code quality

4. **GitHub Analysis** (1A partial + 2D)
   - Run: gh pr list --repo [appropriate-repo] if applicable
   - Check commit messages and patterns
   - Evaluate README and documentation

5. **DevOps/Guardrails Analysis** (1C + 2C)
   - Check for .pre-commit-config.yaml, linting configs
   - Verify Docker setup
   - Assess if app appears runnable

OUTPUT: Write a comprehensive report to:
/Users/rossmalpass/Code/terminal-velocity-temp/judging/reviews/[JUDGE]-ai-review/[PARTICIPANT].md

Use this format:

# [Participant Name] - Hackathon Judging Report

## Summary Scores

| Category | Score | Max | Notes |
|----------|-------|-----|-------|
| 1A. Compliance | X | 2 | [brief note] |
| 1B. Sophistication | X | 3 | [brief note] |
| 1C. Guardrails | X | 1 | [brief note] |
| **Rules Subtotal** | **X** | **6** | |
| 2A. Architecture | X | 2 | [brief note] |
| 2B. Code Quality | X | 2 | [brief note] |
| 2C. Functionality | X | 1 | [brief note] |
| 2D. Documentation | X | 1 | [brief note] |
| **Quality Subtotal** | **X** | **6** | |
| **TOTAL (Leads)** | **X** | **12** | |

## Detailed Analysis

### 1A. Compliance (X/2)
[Detailed reasoning]

### 1B. Sophistication (X/3)
- Context Evolution & Distribution: X/1
- Sub-agent Usage: X/1
- Planning Files: X/1

**Repos Checked:**
- Workspace: [path] - CLAUDE.md evolution? [yes/no, # commits]
- Frontend: [path] - CLAUDE.md evolution? [yes/no, # commits]
- Backend: [path] - CLAUDE.md evolution? [yes/no, # commits]

[Detailed reasoning with specific evidence from each repo]

### 1C. Guardrails (X/1)
[Detailed reasoning]

### 2A. Architecture (X/2)
[Combined Swift and Python analysis]

### 2B. Code Quality (X/2)
[Combined Swift and Python analysis]

### 2C. Functionality (X/1)
[Assessment]

### 2D. Documentation (X/1)
[Assessment]

## Where Points Were Lost

Summarize exactly where and why this participant lost marks. Be specific about what was missing or insufficient.

| Category | Points Lost | Reason |
|----------|-------------|--------|
| [e.g. 1A] | [e.g. -1] | [Specific reason, e.g. "~30% of commits missing co-author tag"] |
| [e.g. 1B] | [e.g. -2] | [Specific reason, e.g. "No sub-agents used, no planning files found"] |
| ... | ... | ... |

**Total Points Lost: X/12**

## Highlights
[Top 3-5 notable positives]

## Concerns
[Top 3-5 issues or areas lacking]

## Creativity Notes (for human judges)
[Observations about creativity, innovation, or unique approaches - NOT scored]
```

## Participants List (for reference)

These are the expected participants in `judging/participants/`:
- agentarium-ws
- antisocial-ws
- anttuii-ws
- crucible-collective-ws
- csaba-ombori-hack-ws
- hackathon-app-ws
- hackathon-lukas-ws
- kafeel-ws
- make-homer-proud-timer-ws
- mini-mate-1-ws
- neatdog-ws
- oculog-ws
- phystone-ws
- pulsync-ws
- spotdrop-ws
- team-awesome-ws
- team-bellard-ws
- teamfred-ws
- your-3d-print-decision-buddy-ws
