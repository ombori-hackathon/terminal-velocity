# Judge Participant - Full Analysis

Orchestrate a complete analysis of a hackathon participant by spawning specialized analysis agents in parallel, then synthesizing their findings into a final scorecard.

## Input
Participant name: $ARGUMENTS

## CRITICAL: Hackathon Rules Compliance

Before scoring, verify the submission follows hackathon rules:
- **Python backend** (FastAPI) - REQUIRED
- **SwiftUI macOS client OR Electron desktop app** - REQUIRED
- Must be a **desktop application**, NOT a web app
- Having additional components (web app, etc.) ON TOP OF a working SwiftUI/Electron client is OK

If a submission has no SwiftUI/Electron desktop client, it is non-compliant and should receive reduced scores.

## Workflow

1. Verify rules compliance (Python backend + SwiftUI/Electron desktop client)
2. Spawn 5 specialized agents in parallel for the participant repo
3. Collect all agent reports
4. Synthesize into final participant scorecard
5. Write results to `/Users/rossmalpass/Code/terminal-velocity-temp/judging/[participant].md`

## Agents to Spawn

For repo at `/Users/rossmalpass/Code/terminal-velocity-temp/participants/[name]-ws/`:

### Agent 1: Claude Code Analyst
Focus: 1A Compliance + 1B Sophistication (5 pts total)
- Analyze claude.md quality and evolution
- Check .claude/commands/ for custom skills
- Look for planning/spec files
- Assess commit co-authoring patterns

### Agent 2: Swift/macOS Analyst
Focus: 2A Architecture + 2B Code Quality (Swift portion)
- Analyze apps/macos-client structure
- Read key Swift files for quality
- Check SwiftUI patterns, state management

### Agent 3: Python/API Analyst
Focus: 2A Architecture + 2B Code Quality (Python portion)
- Analyze services/api structure
- Read key Python files for quality
- Check FastAPI patterns, type hints

### Agent 4: GitHub Practices Analyst
Focus: 1A Compliance + 2D Documentation (contributes to multiple)
- Fetch PR list via gh pr list
- Analyze commit messages and branch patterns
- Evaluate README quality

### Agent 5: DevOps/Guardrails Analyst
Focus: 1C Guardrails + 2C Functionality (2 pts total)
- Check pre-commit hooks, linting configs
- Assess Docker setup
- Evaluate if app appears runnable

## Final Scorecard Format

```markdown
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
[Detailed reasoning from Claude Code and GitHub agents]

### 1B. Sophistication (X/3)
- Context Evolution & Distribution: X/1
- Sub-agent Usage: X/1
- Planning Files: X/1
[Detailed reasoning]

### 1C. Guardrails (X/1)
[Detailed reasoning from DevOps agent]

### 2A. Architecture (X/2)
[Combined reasoning from Swift and Python agents]

### 2B. Code Quality (X/2)
[Combined reasoning from Swift and Python agents]

### 2C. Functionality (X/1)
[Reasoning from DevOps agent]

### 2D. Documentation (X/1)
[Reasoning from GitHub agent]

## Highlights
[Top 3-5 notable positives across all analyses]

## Concerns
[Top 3-5 issues or areas lacking]

## Creativity Notes (for human judges)
[Observations about creativity, innovation, or unique approaches]
```
