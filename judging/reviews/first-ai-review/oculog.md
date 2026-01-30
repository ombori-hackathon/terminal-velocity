# oculog - Hackathon Judging Report

## Summary Scores

| Category | Score | Max | Notes |
|----------|-------|-----|-------|
| 1A. Compliance | 2 | 2 | 75% co-authorship, customized CLAUDE.md |
| 1B. Sophistication | 2.5 | 3 | Context: 0.5, Sub-agents: 1, Planning: 1 |
| 1C. Guardrails | 0 | 1 | No pre-commit, no linting configs |
| **Rules Subtotal** | **4.5** | **6** | |
| 2A. Architecture | 2 | 2 | Clean separation in Swift and Python |
| 2B. Code Quality | 2 | 2 | Modern patterns, type safety |
| 2C. Functionality | 1 | 1 | Full auth, CRUD, weather integration |
| 2D. Documentation | 0.5 | 1 | Good CLAUDE.md but empty API README |
| **Quality Subtotal** | **5.5** | **6** | |
| **TOTAL (Leads)** | **10** | **12** | |

## Project Overview

**Oculog** is an eye condition tracking application for monitoring dry eye disease and MGD (Meibomian Gland Dysfunction). Features a SwiftUI macOS client and FastAPI backend with comprehensive symptom logging, lifestyle tracking, and weather integration.

## Detailed Analysis

### 1A. Compliance (2/2)

**Co-authorship Stats:**
- 75% co-authored commits across all repos
- Correct format: `Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>`

**CLAUDE.md Evaluation:**
- Customized with project-specific content
- Contains key rules, quick start, project structure
- Submodule-specific CLAUDE.md files

### 1B. Sophistication (2.5/3)

**Context Evolution: 0.5/1**
- CLAUDE.md structure for evolution exists
- Limited evidence of actual evolution
- Contains "Continuous Improvement" section

**Sub-agent Usage: 1/1**
- 6 custom agents with proper frontmatter
- Clear role separation and model specifications
- Architect uses Opus, coders use Sonnet

**Planning Files: 1/1**
- specs/ directory with template
- Actual spec file: `2026-01-29-splash-screen-eye-animation.md` (121 lines)
- /feature skill with TDD workflow

### 1C. Guardrails (0/1)

- No `.pre-commit-config.yaml`
- No `.swiftlint.yml`
- No ruff/black configuration in pyproject.toml

### 2A. Architecture (2/2)

**Swift:**
- Clean separation: State, Views, Services, Models
- Proper SwiftUI App lifecycle
- Custom animation views

**Python:**
- Clean layered architecture: routers, schemas, models, services
- Proper exception hierarchy
- JWT auth with access/refresh tokens

### 2B. Code Quality (2/2)

**Swift:**
- `@MainActor` for UI-bound classes
- Async/await throughout
- Custom `LocalizedError` conformance
- Combine framework integration

**Python:**
- Full type hints (`str | None`)
- Pydantic validation with Field constraints
- SQLAlchemy 2.0 with Mapped types

### 2C. Functionality (1/1)

- Working full-stack app with authentication
- Condition log CRUD with symptoms, lifestyle, treatments
- Weather integration
- Animated splash screen with eye animation
- Pagination and sorting support

### 2D. Documentation (0.5/1)

- Good CLAUDE.md files with commands
- API README is empty
- Specs template well-designed

## Highlights

1. **Comprehensive health tracking**: Symptoms, lifestyle, treatments all logged
2. **Weather correlation**: Integrates external data for analysis
3. **Custom animations**: Eye animation view shows SwiftUI skill
4. **Detailed spec example**: Splash screen spec shows planning process

## Concerns

- No guardrails (pre-commit, linting, tests)
- Empty API README
- No feature branches/PRs despite documented workflow

## Creativity Notes

The eye condition tracking concept is medically relevant and practical. Tracking correlations between lifestyle factors (sleep, screen time, caffeine) and eye symptoms could provide real insights for sufferers. The eye animation on the splash screen is a nice themed touch.
