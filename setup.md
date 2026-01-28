# Ombori Hackathon Bootstrap

Execute this setup completely. Ask me for a project name first, then proceed autonomously.

## REQUIREMENTS
- You must be a member of the `ombori-hackathon` GitHub organization
- macOS (this bootstrap is optimized for Mac)
- Internet connection

## RULES
1. Use exact versions specified below - DO NOT research or guess
2. Execute commands sequentially in the order given
3. Create ALL files with the EXACT content shown
4. Ask for project name FIRST, then proceed without stopping
5. NEVER ask the user to run commands manually - execute everything yourself
6. Install missing dependencies automatically without asking for confirmation
7. BEFORE deleting any folder, always `cd` to workspace root first (prevents shell from breaking)

---

## STEP 1: Get Project Name

Ask me for a project name (lowercase, hyphens ok, no spaces). Example: `team-awesome`

This creates repos:
- `{name}-ws` (workspace)
- `{name}-macos` (Swift client)
- `{name}-api` (Python backend)

---

## STEP 2: Install & Verify Prerequisites

Check each tool and install if missing. Run these sequentially:

### 2.1 Install Homebrew (if missing)
```bash
if ! command -v brew &> /dev/null; then
    echo "Installing Homebrew..."
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
fi
```

### 2.2 Install Git (if missing)
```bash
if ! command -v git &> /dev/null; then
    echo "Installing Git..."
    brew install git
fi
git --version
```

### 2.3 Install GitHub CLI (if missing)
```bash
if ! command -v gh &> /dev/null; then
    echo "Installing GitHub CLI..."
    brew install gh
fi
gh --version
```

### 2.4 Authenticate GitHub CLI
```bash
# Check if authenticated
if ! gh auth status &> /dev/null; then
    echo "Please authenticate with GitHub..."
    gh auth login
fi

# Configure git to use gh credentials (prevents 403 errors on org repos)
gh auth setup-git

# Verify access to ombori-hackathon org
gh api orgs/ombori-hackathon --silent && echo "✅ Access to ombori-hackathon org confirmed" || echo "❌ No access to ombori-hackathon org - contact organizer"
```

### 2.5 Install Docker Desktop (if missing) and ensure it's running
```bash
if ! command -v docker &> /dev/null; then
    echo "Installing Docker..."
    brew install --cask docker
fi

# Check if Docker daemon is running, if not start Docker Desktop
if ! docker info &> /dev/null; then
    echo "Starting Docker Desktop..."
    open -a Docker
    echo "Waiting for Docker to start..."
    while ! docker info &> /dev/null; do
        sleep 2
    done
    echo "✅ Docker is running"
fi

docker --version
docker compose version
```

### 2.6 Install Python & uv (if missing)
```bash
# Install Python 3.12+ if needed
if ! command -v python3 &> /dev/null || [[ $(python3 -c "import sys; print(sys.version_info >= (3, 12))") != "True" ]]; then
    echo "Installing Python 3.12..."
    brew install python@3.12
fi
python3 --version

# Install uv
if ! command -v uv &> /dev/null; then
    echo "Installing uv..."
    curl -LsSf https://astral.sh/uv/install.sh | sh
    # Add to PATH for current session
    export PATH="$HOME/.local/bin:$PATH"
fi
uv --version
```

### 2.7 Verify Swift (comes with Xcode/Command Line Tools)
```bash
if ! command -v swift &> /dev/null; then
    echo "Installing Xcode Command Line Tools (includes Swift)..."
    xcode-select --install
    echo "⚠️  Follow the popup to install, then continue"
fi
swift --version
```

### 2.8 Final Check - All Prerequisites
```bash
echo "=== Prerequisites Check ==="
echo "Git: $(git --version)"
echo "GitHub CLI: $(gh --version | head -1)"
echo "Docker: $(docker --version)"
echo "Python: $(python3 --version)"
echo "uv: $(uv --version)"
echo "Swift: $(swift --version 2>&1 | head -1)"
echo "=========================="
```

---

## STEP 3: Create Workspace Structure

The current folder becomes your workspace. Create the project structure:

```bash
mkdir -p apps/macos-client
mkdir -p services/api
```

---

## STEP 4: Create SwiftUI App

### 4.1 Initialize Swift Package

```bash
cd apps/macos-client
swift package init --name {Name}Client --type executable
```

Note: `{Name}` is the project name in PascalCase (e.g., `team-awesome` → `TeamAwesomeClient`)

### 4.2 Replace Package.swift

```swift
// swift-tools-version: 6.0
import PackageDescription

let package = Package(
    name: "{Name}Client",
    platforms: [.macOS(.v14)],
    targets: [
        .executableTarget(
            name: "{Name}Client",
            path: "Sources"
        ),
    ]
)
```

### 4.3 Create SwiftUI App Structure

Delete the generated main.swift and create these files:

```bash
rm Sources/main.swift
```

#### Sources/{Name}App.swift

```swift
import SwiftUI

@main
struct {Name}App: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
        .windowStyle(.hiddenTitleBar)
        .defaultSize(width: 800, height: 600)
    }
}
```

#### Sources/ContentView.swift

```swift
import SwiftUI

struct ContentView: View {
    @State private var items: [Item] = []
    @State private var isLoading = false
    @State private var apiStatus = "Checking..."
    @State private var errorMessage: String?

    private let baseURL = "http://localhost:8000"

    var body: some View {
        VStack(spacing: 0) {
            // Header
            HStack {
                Text("{name}")
                    .font(.title.bold())
                Spacer()
                Circle()
                    .fill(apiStatus == "healthy" ? .green : .red)
                    .frame(width: 12, height: 12)
                Text(apiStatus)
                    .foregroundStyle(.secondary)
            }
            .padding()
            .background(.bar)

            Divider()

            // Content
            if let error = errorMessage {
                VStack(spacing: 12) {
                    Image(systemName: "exclamationmark.triangle")
                        .font(.largeTitle)
                        .foregroundStyle(.orange)
                    Text(error)
                        .foregroundStyle(.secondary)
                    Text("Start API: cd services/api && uv run fastapi dev")
                        .font(.caption)
                        .foregroundStyle(.tertiary)
                }
                .frame(maxWidth: .infinity, maxHeight: .infinity)
            } else if isLoading {
                ProgressView("Loading items...")
                    .frame(maxWidth: .infinity, maxHeight: .infinity)
            } else {
                ItemsTable(items: items)
            }
        }
        .task {
            await loadData()
        }
    }

    private func loadData() async {
        isLoading = true
        errorMessage = nil

        // Check health
        do {
            let url = URL(string: "\(baseURL)/health")!
            let (data, _) = try await URLSession.shared.data(from: url)
            let health = try JSONDecoder().decode(HealthResponse.self, from: data)
            apiStatus = health.status
        } catch {
            apiStatus = "offline"
            errorMessage = "API not running"
            isLoading = false
            return
        }

        // Fetch items
        do {
            let url = URL(string: "\(baseURL)/items")!
            let (data, _) = try await URLSession.shared.data(from: url)
            items = try JSONDecoder().decode([Item].self, from: data)
        } catch {
            errorMessage = "Failed to load items"
        }

        isLoading = false
    }
}

#Preview {
    ContentView()
}
```

#### Sources/ItemsTable.swift

```swift
import SwiftUI

struct ItemsTable: View {
    let items: [Item]

    var body: some View {
        Table(items) {
            TableColumn("ID") { item in
                Text("\(item.id)")
                    .monospacedDigit()
            }
            .width(50)

            TableColumn("Name", value: \.name)
                .width(min: 100, ideal: 150)

            TableColumn("Description", value: \.description)

            TableColumn("Price") { item in
                Text(item.price, format: .currency(code: "USD"))
                    .monospacedDigit()
            }
            .width(80)
        }
    }
}
```

#### Sources/Models.swift

```swift
import Foundation

struct Item: Codable, Identifiable {
    let id: Int
    let name: String
    let description: String
    let price: Double
}

struct HealthResponse: Codable {
    let status: String
}
```

### 4.4 Create .gitignore for Swift

```
.DS_Store
.build/
.swiftpm/
*.xcodeproj
xcuserdata/
```

### 4.5 Create CLAUDE.md for Swift submodule

```markdown
# {Name}Client - SwiftUI App

macOS desktop app that communicates with the FastAPI backend.

## Commands
- Build: `swift build`
- Run: `swift run {Name}Client` (opens GUI window)
- Test: `swift test`

## Architecture
- SwiftUI app with native macOS window
- Entry point: Sources/{Name}App.swift
- Main view: Sources/ContentView.swift
- Data models: Sources/Models.swift
- Uses async/await with URLSession
- Targets macOS 14+

## API Integration
- Backend runs at http://localhost:8000
- Health check: GET /health
- Sample data: GET /items (returns list of items)

## Adding Features
1. Create new SwiftUI views in Sources/
2. Add new async functions for API calls in views or a dedicated APIClient
2. Use `URLSession.shared.data(from:)` for GET requests
3. Use `URLSession.shared.data(for:)` for POST/PUT with URLRequest
```

### 4.6 Test Swift builds

```bash
swift build
```

Go back to workspace root:
```bash
cd ../..
```

---

## STEP 5: Create FastAPI Backend

### 5.1 Initialize with uv

```bash
cd services/api
uv init --app
```

### 5.2 Replace pyproject.toml

```toml
[project]
name = "hackathon-api"
version = "0.1.0"
description = "Hackathon FastAPI Backend"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "fastapi[standard]>=0.115.0",
    "sqlalchemy>=2.0.0",
    "alembic>=1.13.0",
    "psycopg2-binary>=2.9.0",
    "pydantic-settings>=2.0.0",
]

[dependency-groups]
dev = [
    "pytest>=8.0.0",
    "pytest-asyncio>=0.23.0",
    "httpx>=0.27.0",
]
```

### 5.3 Create app directory structure

```bash
mkdir -p app/routers app/models app/schemas
touch app/__init__.py app/routers/__init__.py app/models/__init__.py app/schemas/__init__.py
```

### 5.4 Create app/config.py

```python
from pydantic_settings import BaseSettings


class Settings(BaseSettings):
    database_url: str = "postgresql://postgres:postgres@localhost:5432/hackathon"
    debug: bool = True

    class Config:
        env_file = ".env"


settings = Settings()
```

### 5.5 Create app/db.py

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase

from app.config import settings

engine = create_engine(settings.database_url)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)


class Base(DeclarativeBase):
    pass


def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

### 5.6 Create app/main.py

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel


app = FastAPI(
    title="Hackathon API",
    description="Backend API for Ombori Hackathon",
    version="0.1.0",
)

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)


# --- Sample Data Model ---
class Item(BaseModel):
    id: int
    name: str
    description: str
    price: float


# --- Sample Data (replace with database later) ---
SAMPLE_ITEMS = [
    Item(id=1, name="Widget", description="A useful widget for your desk", price=9.99),
    Item(id=2, name="Gadget", description="A fancy gadget with buttons", price=19.99),
    Item(id=3, name="Gizmo", description="An amazing gizmo that does things", price=29.99),
]


@app.get("/")
async def root():
    return {"message": "Hackathon API is running!", "docs": "/docs"}


@app.get("/health")
async def health():
    return {"status": "healthy"}


@app.get("/items", response_model=list[Item])
async def get_items():
    """Get all items - sample data to demonstrate the API pattern"""
    return SAMPLE_ITEMS


@app.get("/items/{item_id}")
async def get_item(item_id: int):
    """Get a specific item by ID"""
    for item in SAMPLE_ITEMS:
        if item.id == item_id:
            return item
    return {"error": "Item not found"}
```

### 5.7 Create .gitignore for Python

```
__pycache__/
*.py[cod]
.venv/
.env
*.egg-info/
dist/
build/
.pytest_cache/
.ruff_cache/
uv.lock
```

### 5.8 Create .env.example

```
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/hackathon
DEBUG=true
```

### 5.9 Create CLAUDE.md for Python submodule

```markdown
# Hackathon API - FastAPI Backend

Python FastAPI backend with PostgreSQL database.

## Commands
- Run dev server: `uv run fastapi dev`
- Run tests: `uv run pytest`
- Sync dependencies: `uv sync`
- Add dependency: `uv add <package>`

## Project Structure
```
app/
├── main.py          # FastAPI app entry point
├── config.py        # Pydantic settings
├── db.py            # SQLAlchemy database setup
├── models/          # SQLAlchemy ORM models
├── schemas/         # Pydantic request/response schemas
└── routers/         # API route handlers
```

## Database
- PostgreSQL via Docker Compose
- SQLAlchemy 2.0 ORM
- Connection: postgresql://postgres:postgres@localhost:5432/hackathon

## API Docs
- Swagger UI: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

## Adding Features
1. Create model in app/models/
2. Create schemas in app/schemas/
3. Create router in app/routers/
4. Register router in app/main.py
```

### 5.10 Install dependencies and test

```bash
uv sync
uv run fastapi dev --host 0.0.0.0 --port 8000 &
sleep 3
curl http://localhost:8000/health
# Kill the test server
pkill -f "fastapi dev"
```

Go back to workspace root:
```bash
cd ../..
```

---

## STEP 6: Create GitHub Repos

```bash
gh repo create ombori-hackathon/{name}-ws --public --description "{name} - Terminal Velocity workspace"
gh repo create ombori-hackathon/{name}-macos --public --description "{name} - Swift macOS client"
gh repo create ombori-hackathon/{name}-api --public --description "{name} - FastAPI Python backend"
```

---

## STEP 7: Push Swift Project and Convert to Submodule

### 7.1 Initialize and push Swift repo

```bash
cd apps/macos-client
git init
git add .
git commit -m "Initial SwiftUI app setup"
git branch -M main
git remote add origin https://github.com/ombori-hackathon/{name}-macos.git
git push -u origin main
cd ../..
```

### 7.2 Remove folder and add as submodule

**IMPORTANT: Must be in workspace root before deleting!**

```bash
cd /path/to/workspace  # Use actual workspace path!
rm -rf apps/macos-client
git submodule add https://github.com/ombori-hackathon/{name}-macos.git apps/macos-client
```

---

## STEP 8: Push Python Project and Convert to Submodule

### 8.1 Initialize and push Python repo

```bash
cd services/api
git init
git add .
git commit -m "Initial FastAPI backend setup"
git branch -M main
git remote add origin https://github.com/ombori-hackathon/{name}-api.git
git push -u origin main
cd ../..
```

### 8.2 Remove folder and add as submodule

**IMPORTANT: Must be in workspace root before deleting!**

```bash
cd /path/to/workspace  # Use actual workspace path!
rm -rf services/api
git submodule add https://github.com/ombori-hackathon/{name}-api.git services/api
```

---

## STEP 9: Create Docker Compose

Create `docker-compose.yml`:

```yaml
services:
  postgres:
    image: postgres:17
    container_name: hackathon-db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: hackathon
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```

Create `.env`:

```
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/hackathon
```

---

## STEP 10: Create Claude Code Agents

Create `.claude/agents/` directory and add these files:

### .claude/agents/architect.md

```yaml
---
name: architect
description: System design and cross-repo planning. Use for API contracts, database schemas, architectural decisions.
tools: Read, Grep, Glob, WebSearch, WebFetch
model: opus
---

# Architect Agent

Senior software architect for hackathon project.

## Your Codebase
- Swift CLI client: apps/macos-client/
- FastAPI backend: services/api/
- PostgreSQL database via Docker
- Specs: specs/

## Responsibilities
1. Design API contracts (OpenAPI spec)
2. Plan database schemas (SQLAlchemy models)
3. Ensure client-server compatibility
4. Document architectural decisions
5. **Update CLAUDE.md and agents** with patterns and learnings

## Output Format
Always output GitHub-compatible markdown specs to `specs/` folder with:
- Clear requirements
- API endpoint definitions
- Data models
- Implementation steps

## Continuous Improvement
After completing features, suggest updates to:
- `CLAUDE.md` - New patterns, conventions discovered
- Agent files - Better instructions based on what worked
- Submodule CLAUDE.md files - Tech-specific learnings
```

### .claude/agents/swift-coder.md

```yaml
---
name: swift-coder
description: Swift client development. Use for implementing features in apps/macos-client/.
model: sonnet
---

# Swift Coder Agent

Swift developer for the macOS SwiftUI app.

## Your Codebase
- Location: apps/macos-client/
- Entry point: Sources/{Name}App.swift
- Main view: Sources/ContentView.swift
- Models: Sources/Models.swift
- Build: swift build
- Run: swift run {Name}Client

## Patterns
- Use async/await for all network calls
- URLSession for HTTP requests
- Codable for JSON parsing
- Print clear status messages

## When Adding Features
1. Add new async functions for API endpoints
2. Update main() to use new features
3. Test with swift run
```

### .claude/agents/python-coder.md

```yaml
---
name: python-coder
description: FastAPI backend development. Use for implementing features in services/api/.
model: sonnet
---

# Python Coder Agent

FastAPI developer for the backend API.

## Your Codebase
- Location: services/api/
- Entry point: app/main.py
- Run: uv run fastapi dev

## Patterns
- Pydantic schemas for request/response
- SQLAlchemy models for database
- Dependency injection with Depends()
- Async endpoints where beneficial

## When Adding Features
1. Create model in app/models/
2. Create schemas in app/schemas/
3. Create router in app/routers/
4. Register router in main.py
5. Test at /docs
```

### .claude/agents/reviewer.md

```yaml
---
name: reviewer
description: Code review across all repos. Use before committing significant changes.
tools: Read, Grep, Glob
model: sonnet
---

# Code Reviewer Agent

Reviews code for quality, security, and consistency.

## Review Checklist
- [ ] No hardcoded secrets
- [ ] Error handling present
- [ ] Types/schemas defined
- [ ] API contracts match between client/server
- [ ] Database queries are safe (no SQL injection)

## Output Format
Provide structured feedback:
1. **Issues** - Must fix before merge
2. **Suggestions** - Recommended improvements
3. **Approval** - Ready to commit or not
```

### .claude/agents/debugger.md

```yaml
---
name: debugger
description: Debug issues across repos. Use when something isn't working.
model: sonnet
---

# Debugger Agent

Investigates and fixes issues.

## Debugging Steps
1. Reproduce the issue
2. Check logs (docker compose logs, uvicorn output, swift build errors)
3. Trace the code path
4. Identify root cause
5. Propose fix

## Common Issues
- API not running: `uv run fastapi dev`
- Database not running: `docker compose up -d`
- Swift build fails: Check Package.swift dependencies
- Connection refused: Check ports (8000 for API, 5432 for DB)
```

### .claude/agents/tester.md

```yaml
---
name: tester
description: Test-driven development. Use to write and run tests.
model: sonnet
---

# Tester Agent

Writes and runs tests for both repos.

## Python Tests
- Location: services/api/tests/
- Run: `uv run pytest`
- Use httpx for API testing
- Use pytest-asyncio for async tests

## Swift Tests
- Location: apps/macos-client/Tests/
- Run: `swift test`

## TDD Workflow
1. Write failing test first
2. Implement feature
3. Run tests until passing
4. Refactor if needed
```

---

## STEP 11: Create Skills

Create `.claude/skills/` directory for workflow commands:

```bash
mkdir -p .claude/skills/feature
```

### .claude/skills/feature/SKILL.md

```yaml
---
name: feature
description: Build a new feature end-to-end. Asks clarifying questions, creates a spec/PRD, then implements using TDD (tests first). Use this to start any new feature.
user-invocable: true
allowed-tools: Read, Write, Edit, Bash, Grep, Glob
---

# /feature - New Feature Workflow

Build features using test-driven development with a proper spec.

## Usage

```
/feature <brief description of what you want to build>
```

## Workflow

### Step 1: Understand the Feature

Ask the user these questions (wait for answers before proceeding):

1. **What should this feature do?** (one sentence)
2. **Who/what triggers it?** (user action, API call, scheduled, etc.)
3. **What's the expected output/result?**
4. **Any edge cases to handle?**

### Step 2: Identify Affected Repos

Based on the feature, determine scope:
- **API only** → `services/api/`
- **Swift client only** → `apps/macos-client/`
- **Full stack** → Both repos

### Step 3: Enter Plan Mode

**IMPORTANT: Enter plan mode now using the EnterPlanMode tool.**

In plan mode, create the spec file `specs/YYYY-MM-DD-feature-name.md` with this structure:

```markdown
# Feature: [Name]

## Summary
[One sentence from Step 1]

## Trigger
[From Step 1 question 2]

## Expected Result
[From Step 1 question 3]

## Edge Cases
[From Step 1 question 4]

## Technical Design

### API Changes (if applicable)
- Endpoint: `METHOD /path`
- Request: `{ field: type }`
- Response: `{ field: type }`

### Swift Client Changes (if applicable)
- New struct/function: `name`
- Display: [how it shows to user]

### Database Changes (if applicable)
- New table/columns: [describe]

## Implementation Plan
1. [ ] Write API tests (Red)
2. [ ] Implement API endpoint (Green)
3. [ ] Write Swift tests (Red)
4. [ ] Implement Swift code (Green)
5. [ ] Integration test
6. [ ] Commit and push
```

After writing the spec, use ExitPlanMode to present it for user approval.

**Do not proceed to implementation until the user approves the plan.**

### Step 4: TDD Red Phase - Write Failing Tests

**For API (if applicable):**
1. Create `services/api/tests/test_<feature>.py`
2. Write test for the new endpoint
3. Run `uv run pytest` - confirm it FAILS (Red)

**For Swift (if applicable):**
1. Add test in `apps/macos-client/Tests/`
2. Write test for new functionality
3. Run `swift test` - confirm it FAILS (Red)

### Step 5: TDD Green Phase - Implement

Implement minimum code to make tests pass:

**API:**
1. Add endpoint to `services/api/app/main.py` or create router
2. Run `uv run pytest` - should PASS (Green)

**Swift:**
1. Add code to `apps/macos-client/Sources/`
2. Run `swift test` - should PASS (Green)

### Step 6: Refactor (Optional)

Clean up code while keeping tests green.

### Step 7: Create PR

Ask user: "All tests pass. Ready to create a PR?"

If yes, use `gh` CLI to create PRs (never use GitHub web interface):

```bash
# Create feature branch, commit, and open PR for each repo
cd apps/macos-client
git checkout -b feature/<feature-name>
git add .
git commit -m "feat: <description>"
git push -u origin feature/<feature-name>
gh pr create --title "feat: <description>" --body "Implements <feature>"

cd ../../services/api
git checkout -b feature/<feature-name>
git add .
git commit -m "feat: <description>"
git push -u origin feature/<feature-name>
gh pr create --title "feat: <description>" --body "Implements <feature>"
```

Update spec with PR links and completion status.
```

---

## STEP 12: Create Specs Directory

Create a `specs/` folder for centralized feature specifications:

```bash
mkdir -p specs
```

Create `specs/README.md`:

```markdown
# Feature Specifications

All feature specs live here. Created via plan mode before implementation.

## Naming Convention
`YYYY-MM-DD-feature-name.md`

## Template
See `specs/_template.md`
```

Create `specs/_template.md`:

```markdown
# Feature: [Name]

## Summary
Brief description of the feature.

## API Changes
- `POST /endpoint` - Description

## Database Changes
- New table: `table_name`
- New columns: `column_name`

## Swift Client Changes
- New function: `fetchSomething()`

## Implementation Steps
1. [ ] Step 1
2. [ ] Step 2

## Tests
- [ ] API test: description
- [ ] Client test: description
```

---

## STEP 13: Create Workspace CLAUDE.md

Create `CLAUDE.md` in workspace root:

```markdown
# Hackathon Workspace

Multi-repo workspace for Ombori hackathon.

## Key Rules

1. **Plan-mode-first**: ALL features start with spec creation in `specs/` folder
2. **TDD where applicable**: Write tests before implementation
3. **Specs in workspace**: All specs centralized in `specs/` folder
4. **Evolve the config**: Update CLAUDE.md and agents with learnings as you build

## Continuous Improvement

As you build, update these files with learnings:
- `CLAUDE.md` - Add new patterns, gotchas, project-specific conventions
- `.claude/agents/*.md` - Refine agent instructions based on what works
- `apps/macos-client/CLAUDE.md` - Swift-specific learnings
- `services/api/CLAUDE.md` - Python/FastAPI-specific learnings

Examples of things to capture:
- "Always use X pattern for Y"
- "Don't forget to run Z after changing W"
- "API endpoint naming follows this convention..."
- "Database migrations require this step..."

## Quick Start

```bash
# Start database
docker compose up -d

# Run API (in new terminal)
cd services/api && uv run fastapi dev

# Run Swift client (in new terminal)
cd apps/macos-client && swift run {Name}Client
```

## Structure
- `apps/macos-client/` - SwiftUI desktop app (submodule)
- `services/api/` - FastAPI Python backend (submodule)
- `specs/` - Feature specifications (plan-mode output)
- `docker-compose.yml` - PostgreSQL database

## Skills (Commands)
Available in `.claude/skills/`:
- `/feature` - **Start here!** Asks questions → creates spec → TDD implementation

## Agents
Available in `.claude/agents/`:
- `/architect` - System design, API contracts → outputs to `specs/`
- `/swift-coder` - Swift client development
- `/python-coder` - FastAPI backend development
- `/reviewer` - Code review across all repos
- `/debugger` - Issue investigation
- `/tester` - Test-driven development

## Development Workflow

### New Features (MANDATORY)
1. **Plan mode first** - Create spec in `specs/YYYY-MM-DD-feature-name.md`
2. **Write tests** - TDD: tests before implementation
3. **Implement** - Use coder agents (can run in parallel)
4. **Review** - Use reviewer agent
5. **Commit** - Submodules first, then workspace

### Git Workflow (use `gh` CLI, not GitHub web)
Always use feature branches and `gh pr create`:
```bash
# In each submodule
git checkout -b feature/<name>
git add . && git commit -m "feat: ..."
git push -u origin feature/<name>
gh pr create --title "feat: ..." --body "Description"

# After PRs merged, update workspace
cd ../..
git add . && git commit -m "Update submodules"
git push
```

## API Reference
- Swagger: http://localhost:8000/docs
- Health: http://localhost:8000/health
- Items: http://localhost:8000/items (sample data)
```

---

## STEP 14: Initialize and Push Workspace

```bash
git init
git add .
git commit -m "Initial workspace setup with submodules"
git branch -M main
git remote add origin https://github.com/ombori-hackathon/{name}-ws.git
git push -u origin main
```

---

## STEP 15: Verify Everything Works

Run these verification steps:

```bash
# 1. Check submodules
git submodule status

# 2. Start database
docker compose up -d

# 3. Wait for postgres
sleep 5

# 4. Check database is healthy
docker compose ps

# 5. Start API
cd services/api
uv sync
uv run fastapi dev &
sleep 3

# 6. Test API
curl http://localhost:8000/health

# 7. Test Swift client
cd ../apps/macos-client
swift build
swift run {Name}Client

# 8. Check agents exist
ls -la ../../.claude/agents/

# 9. Kill API server
pkill -f "fastapi dev"
```

---

## DONE! 🎉

Your hackathon workspace is ready:
- ✅ 3 GitHub repos created and linked
- ✅ SwiftUI app builds and runs
- ✅ FastAPI backend runs with health endpoint
- ✅ PostgreSQL database via Docker
- ✅ Claude Code agents configured

**Next: Start building your hackathon project!**

Use `/feature` to build your first feature!
