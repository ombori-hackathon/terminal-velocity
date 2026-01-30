# team-bellard - Hackathon Judging Report

**Reviewer:** ross (AI-assisted) | **Date:** 2026-01-30 | **Score:** 8/12 (+2 bonus)

---

## Quick Summary

| Category | Score | Notes |
|----------|-------|-------|
| 1A. Compliance | 1/2 | ~59% co-authored (10/17), backend has only 1 commit with no co-authoring |
| 1B. Sophistication | 2/3 | Good agents and spec files, but context didn't evolve significantly |
| 1C. Guardrails | 0/1 | No pre-commit hooks, no linting, no tests implemented |
| **Rules** | **3/6** | |
| 2A. Architecture | 2/2 | Clean separation, proper submodule structure, logical organization |
| 2B. Code Quality | 2/2 | Excellent Swift code with proper patterns, async/await, error handling |
| 2C. Functionality | 1/1 | Full-featured macOS app with working server, mDNS, HTTPS |
| 2D. Documentation | 1/1 | Comprehensive README and detailed SPEC.md |
| **Quality** | **6/6** | |
| **BASE TOTAL** | **9/12** | |
| **Bonus** | **+2** | Feature branches and PRs in frontend |
| **FINAL** | **11/14** | |

---

## Critical Issues (if any)

> **Backend repo is essentially empty** - Only 1 commit with initial setup, no co-authoring, no development work done. The team pivoted to a pure Swift app that doesn't need a backend, but the backend repo was left untouched.

---

## Points Lost

| Category | Lost | Reason |
|----------|------|--------|
| [1A] | -1 | Backend has 0% co-authoring (1 commit, no Claude). Frontend at 59% (10/17) |
| [1B] | -1 | CLAUDE.md files are lean but no evidence of iterative learning updates |
| [1C] | -1 | No guardrails configured - no pre-commit hooks, no swiftlint, no tests |

**Total Lost: 3 points**

---

## Detailed Analysis

### 1A. Compliance (1/2)

**Co-authoring by Repo:**

| Repo | Dev Commits | Co-authored | Rate |
|------|-------------|-------------|------|
| Workspace | 1 | 0 | 0% |
| Frontend | 17 | 10 | 59% |
| Backend | 1 | 0 | 0% |
| **Total** | **19** | **10** | **53%** |

The frontend (macos-client) has the bulk of the development work with 59% co-authoring rate, which is below the 90%+ target. The workspace and backend repos each have only 1 initial commit with no co-authoring. This suggests the team may have done some commits without using Claude Code, or used it in ways that didn't include the co-author tag.

**Git Workflow (Bonus):**

| Practice | Status | Bonus |
|----------|--------|-------|
| Feature branches | Yes | +1 |
| PRs with descriptions | Yes (6 PRs merged) | +0.5 |
| gh CLI usage | Yes | +0.5 |
| **Bonus Total** | | **+2** |

The frontend repo shows excellent git workflow with 6 merged PRs from `feature/xrve-local-server` branch, demonstrating proper use of feature branches and the `gh pr create` command.

### 1B. Sophistication (2/3)

- **Context Evolution (0/1):** The CLAUDE.md files are reasonably lean and well-structured, but there's no evidence of iterative updates based on learnings. The workspace CLAUDE.md mentions "Evolve the config" but the files themselves appear to be initial setup with no git history showing evolution.

- **Sub-agents (1/1):** Six agents defined with clear roles:
  - `/architect` - System design with Opus model
  - `/swift-coder` - Swift development
  - `/python-coder` - Python development
  - `/reviewer` - Code review
  - `/debugger` - Issue investigation
  - `/tester` - Test-driven development

- **Planning Files (1/1):** Excellent SPEC.md (404 lines) in the frontend repo with:
  - Detailed implementation status tracking
  - Feature specifications with completed/pending status
  - Architecture documentation
  - Data models
  - 10 implementation phases tracked

### 1C. Guardrails (0/1)

- [ ] Pre-commit hooks - Not configured
- [ ] Linting config - No swiftlint or ruff/black
- [ ] Tests - Test locations defined in agents but no actual test files exist
- [ ] CI/CD - None

**Guardrails configured: 0/4 (need 2+ for full point)**

The pyproject.toml has pytest in dev dependencies, and the agents mention test commands, but no actual tests were written.

### 2A. Architecture (2/2)

The project demonstrates excellent architecture:

```
team-bellard-ws/
├── .claude/
│   ├── agents/           # 6 agent definitions
│   └── skills/feature/   # Feature workflow skill
├── apps/macos-client/    # Swift submodule - ACTIVE
│   ├── Sources/
│   │   ├── ServApp.swift         # App entry, menu bar
│   │   ├── ContentView.swift     # Main window UI
│   │   ├── AppState.swift        # State management
│   │   ├── StaticServer.swift    # Vapor HTTP/HTTPS
│   │   ├── CertificateManager.swift  # Local CA
│   │   ├── BonjourService.swift  # mDNS
│   │   └── PortManager.swift     # Port allocation
│   └── SPEC.md                   # Detailed specs
├── services/api/         # Python submodule - UNUSED
└── specs/                # Spec templates
```

The team correctly identified they didn't need a backend and built a fully native macOS app with clean separation of concerns.

### 2B. Code Quality (2/2)

**Swift:**
- Proper use of `@MainActor` for thread safety
- Clean state management with `ObservableObject` and `@Published`
- Async/await throughout for non-blocking operations
- Proper error handling with custom error types
- Clear separation: StaticServer, CertificateManager, BonjourService
- Vapor framework for HTTP/HTTPS with TLS support
- Well-structured SwiftUI views with proper composition

**Python:**
- While minimal, the backend follows FastAPI best practices
- Pydantic schemas, SQLAlchemy models, proper dependency injection
- However, this code is essentially boilerplate and unused

### 2C. Functionality (1/1)

The app is a complete, functional macOS utility:
- Drag & drop folder serving
- HTTP and HTTPS support with local CA certificates
- mDNS registration for `.local` domains
- Multi-project support with persistence
- Menu bar integration
- Project editing (name, port)
- Graceful shutdown
- Node.js project detection

A DMG installer (Serv-1.0.0.dmg) is included for distribution.

### 2D. Documentation (1/1)

Excellent documentation:
- **README.md** (217 lines): Complete with badges, features, installation, usage, architecture
- **SPEC.md** (404 lines): Detailed specs with implementation status, technical stack, phases
- Code comments where needed
- Clear setup instructions

---

## Highlights

1. **Sophisticated Native App** - Built a full-featured macOS menu bar app with HTTPS support, local CA generation, and mDNS registration - significant technical achievement
2. **Excellent PR Workflow** - 6 merged PRs from feature branch demonstrating proper git workflow with meaningful commit messages
3. **Comprehensive Specs** - The SPEC.md is exceptionally detailed with implementation tracking, architecture diagrams, and clear feature breakdown

## Concerns

1. **Low Co-authoring Rate** - 53% overall is below the 90% target, suggesting some work may have been done outside of Claude Code
2. **No Tests or Guardrails** - Despite having a tester agent and pytest in dependencies, no actual tests were written
3. **Abandoned Backend** - The API submodule is essentially unused with only initial setup, no co-authoring

---

## Creativity Notes

This is a genuinely useful utility app - "Serv" allows developers to instantly serve any folder over HTTP/HTTPS with pretty `.local` domains. The HTTPS implementation with automatic CA certificate generation and system trust is particularly impressive for a hackathon project. The app feels polished and production-ready with persistence, graceful shutdown, and a proper DMG installer.

The team made a smart pivot decision to focus on a pure Swift app rather than forcing a frontend/backend split when it wasn't needed. This shows good architectural judgment.

---

*Report generated using template v1.0*
