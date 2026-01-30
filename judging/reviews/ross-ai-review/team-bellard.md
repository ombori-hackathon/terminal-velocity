# team-bellard - Hackathon Judging Report

**Reviewer:** Ross (AI-Assisted)
**Date:** 2026-01-30

## Project Overview

**Application Name:** Serv

**Description:** A native macOS menu bar application that lets users run any folder as a local web server with human-friendly `.local` domain names and HTTPS support. Drag-and-drop folders to instantly serve them as HTTP/HTTPS servers accessible via URLs like `http://my-project.local:8742`.

---

## Summary Scores

| Category | Score | Max | Notes |
|----------|-------|-----|-------|
| 1A. Compliance | 2 | 2 | 10/10 development commits co-authored (100%) |
| 1B. Sophistication | 3 | 3 | Agents present, SPEC.md evolved, planning files exist |
| 1C. Guardrails | 0 | 1 | No pre-commit, linting, or tests configured |
| **Rules Subtotal** | **5** | **6** | |
| 2A. Architecture | 2 | 2 | Excellent workspace structure with submodules |
| 2B. Code Quality | 2 | 2 | Clean Swift/Python code, proper patterns |
| 2C. Functionality | 1 | 1 | App appears complete and functional |
| 2D. Documentation | 1 | 1 | Excellent README and SPEC documentation |
| **Quality Subtotal** | **6** | **6** | |
| **TOTAL (Leads)** | **11** | **12** | |

---

## Detailed Analysis

### 1A. Compliance (2/2)

#### Workspace Repository
| Metric | Count |
|--------|-------|
| Total Commits | 1 |
| Merge Commits | 0 |
| Initial/Template Commits | 1 |
| Development Commits | 0 |
| Co-Authored Commits | 0 |
| **Co-Author Rate** | N/A (no dev commits) |

#### macOS Client Repository (Primary Development)
| Metric | Count |
|--------|-------|
| Total Commits | 17 |
| Merge Commits | 6 |
| Initial/Template Commits | 1 (ba6f942) |
| **Development Commits** | **10** |
| Co-Authored Commits | 10 |
| **Co-Author Rate** | **100%** |

**Detailed Commit Breakdown (macos-client):**

| Hash | Subject | Type | Co-Authored |
|------|---------|------|-------------|
| 83d90e4 | Merge pull request #6 | Merge | EXCLUDED |
| 8a590cb | release: Add Serv 1.0.0 DMG installer | Dev | Yes |
| f294798 | ui: Add tagline to main window header | Dev | Yes |
| ab4fbf9 | docs: Update SPEC.md with implementation status | Dev | Yes |
| a8d69b5 | Merge pull request #5 | Merge | EXCLUDED |
| 5354f01 | feat: Add project name and port editing | Dev | Yes |
| b854327 | Merge pull request #4 | Merge | EXCLUDED |
| 11caa1e | docs: Update README with all current features | Dev | Yes |
| 363d73d | feat: Add persistence and graceful shutdown | Dev | Yes |
| 7aff459 | Merge pull request #3 | Merge | EXCLUDED |
| db83e7b | feat: Add HTTPS support with local CA certificates | Dev | Yes |
| bc3971c | Merge pull request #2 | Merge | EXCLUDED |
| 3881c8d | docs: Add HTTPS support to planned features | Dev | Yes |
| 1ba89c4 | refactor: Rename app from Xrve to Serv | Dev | Yes |
| 2201f8e | Merge pull request #1 | Merge | EXCLUDED |
| 99f7e21 | feat: Transform app into Xrve - Local Web Server | Dev | Yes |
| ba6f942 | Initial SwiftUI app setup | Initial | EXCLUDED |

#### API Repository
| Metric | Count |
|--------|-------|
| Total Commits | 1 |
| Merge Commits | 0 |
| Initial/Template Commits | 1 |
| Development Commits | 0 |
| Co-Authored Commits | 0 |
| **Co-Author Rate** | N/A (no dev commits) |

**Combined Analysis:**
- Total Development Commits: 10
- Co-Authored Development Commits: 10
- **Overall Co-Author Rate: 100%**

**Git Workflow:**
- Used feature branches properly (e.g., `feature/xrve-local-server`)
- Created PRs via `gh pr create` - 6 PRs visible in merge commits
- Meaningful, well-structured commit messages with detailed bodies

### 1B. Sophistication (3/3)

**Context Evolution & Distribution: 1/1**

**CLAUDE.md Evolution:**
- Workspace CLAUDE.md: 1 commit (initial setup)
- Frontend CLAUDE.md: 1 commit (initial setup)
- Backend CLAUDE.md: 1 commit (initial setup)

While CLAUDE.md files did not evolve, **SPEC.md evolved across 4 commits** with substantial project-specific content:
1. `99f7e21` - Initial Xrve specification
2. `1ba89c4` - Rename to Serv
3. `3881c8d` - Add HTTPS support planning
4. `ab4fbf9` - Add detailed implementation status

The SPEC.md (404 lines) contains comprehensive project-specific learnings:
- Decisions table (app name, port strategy, HTTP library choice)
- Implementation status tracking
- Technical stack documentation
- Data model definitions
- Phase-based development with checkboxes

**Awarding point** because SPEC.md has substantial project-specific content that evolved during development.

**Sub-agents / Multi-agent Usage: 1/1**

Excellent sub-agent configuration in `.claude/agents/`:
- `architect.md` - System design with Opus model
- `swift-coder.md` - Swift client development with Sonnet
- `python-coder.md` - FastAPI backend with Sonnet
- `reviewer.md` - Code review agent (limited tools)
- `debugger.md` - Issue investigation
- `tester.md` - TDD agent

Each agent has clear scope, tool restrictions, and model assignments. The `/feature` skill in `.claude/skills/feature/SKILL.md` is particularly well-designed with a structured TDD workflow.

**Planning Files / Spec Files: 1/1**

Strong evidence of plan-mode usage:
- `SPEC.md` in macOS client evolved across **4 commits** with detailed feature specifications
- Comprehensive feature tracking with Implementation Status tables
- Clear phase-based development roadmap (10 phases)
- Technical design decisions documented
- UI mockups in ASCII art

The `specs/` folder in workspace has template structure but actual specs were kept in the submodule with the code.

### 1C. Guardrails (0/1)

No guardrails configured:
- No `.pre-commit-config.yaml` found
- No linting configs (swiftlint, ruff, pylint)
- No test files found in either `Tests/` or `tests/` directories
- No CI/CD workflows

The tester agent and TDD workflow were defined but not used in practice.

### 2A. Architecture (2/2)

**Excellent workspace structure:**
- Clean workspace with two submodules (apps/macos-client, services/api)
- Proper `.gitmodules` configuration
- Docker Compose for PostgreSQL database
- Centralized specs folder with templates

**Swift Client Architecture:**
```
Sources/
├── ServApp.swift         - App entry with MenuBarExtra
├── ContentView.swift     - Main UI with drop zone
├── AppState.swift        - @MainActor state management
├── Models.swift          - Clean data models with enums
├── StaticServer.swift    - Vapor HTTP/HTTPS server
├── CertificateManager.swift - CA and cert generation
├── BonjourService.swift  - mDNS via dns-sd
└── PortManager.swift     - Port allocation singleton
```

Clear separation of concerns with single-responsibility files.

**Python API Structure:**
```
app/
├── main.py      - FastAPI entry with lifespan
├── config.py    - Pydantic settings
├── db.py        - SQLAlchemy setup
├── models/      - ORM models
└── schemas/     - Pydantic schemas
```

Clean FastAPI project structure (though minimally used).

### 2B. Code Quality (2/2)

**Swift Code Quality:**
- Proper use of `@MainActor`, `@StateObject`, `@ObservedObject`
- Clean SwiftUI with proper view composition
- Excellent error handling with custom error enums (`ServerError`, `CertificateError`, `BonjourError`)
- Proper async/await usage throughout
- Clean use of `Process` for shell commands (dns-sd, openssl)
- Singleton patterns appropriately used (PortManager, CertificateManager)
- Memory management with `deinit` cleanup in BonjourService

**Python Code Quality:**
- Type hints used (response_model annotations)
- Proper dependency injection with `Depends(get_db)`
- Clean Pydantic schemas with `from_attributes = True`
- Proper FastAPI lifespan context manager
- Modern SQLAlchemy 2.0 patterns

### 2C. Functionality (1/1)

The app appears fully functional with:
- Menu bar app with server rack icon
- Drag-and-drop folder serving
- Static file server with directory listing
- mDNS registration for `.local` domains
- HTTPS with auto-generated certificates
- Project persistence across restarts
- Port management and editing
- Node.js project detection

A DMG installer was even created (`downloads/Serv-1.0.0.dmg`).

### 2D. Documentation (1/1)

**Excellent documentation:**

**README.md (217 lines):**
- Clear feature descriptions with badges
- Installation instructions
- Usage guide with HTTPS setup
- Architecture explanation
- Supported file types table

**SPEC.md (403 lines):**
- Comprehensive feature specifications
- Implementation status tracking
- UI mockups in ASCII
- Technical stack documentation
- Phase-based development roadmap
- Data model definitions

Both CLAUDE.md files contain clear setup instructions.

## Where Points Were Lost

| Category | Points Lost | Reason |
|----------|-------------|--------|
| 1C. Guardrails | -1 | No pre-commit hooks, linting, or test files despite TDD being part of the workflow design |

**Total Points Lost: 1/12**

## Highlights

1. **Complete, polished product** - Serv is a fully functional menu bar app with HTTPS support, mDNS, persistence, and even a DMG installer
2. **Excellent sub-agent design** - Six specialized agents with clear responsibilities and tool restrictions
3. **Strong planning documentation** - SPEC.md evolved across 4 commits with detailed implementation tracking
4. **High-quality Swift code** - Proper use of modern Swift patterns, clean error handling, good architecture
5. **Excellent PR workflow** - 6 PRs created via gh CLI with feature branches

## Concerns

1. **No tests implemented** - The tester agent and TDD workflow were designed but never used
2. **Backend essentially unused** - The Python API has only initial setup; the team pivoted to a pure Swift app
3. **No guardrails configured** - Despite having agents for testing and review, no automated quality checks

## Creativity Notes (for human judges)

**Innovative product concept:** Serv is a genuinely useful developer tool - a menu bar app that instantly serves any folder with human-friendly `.local` domains. The HTTPS implementation with auto-generated certificates and Keychain trust is particularly impressive.

**Technical sophistication:** The certificate management system using OpenSSL and AppleScript for privilege escalation shows advanced macOS development skills. The mDNS implementation via dns-sd is clever.

**Pivot decision:** The team wisely pivoted from a client-server architecture to a standalone macOS app when they realized the backend wasn't needed. This shows good product judgment.

**Menu bar integration:** The Docker Desktop-style menu bar interface with quick access to projects is well-designed UX.
