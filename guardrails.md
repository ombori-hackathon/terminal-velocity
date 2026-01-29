# Guardrails Setup

Add linting, formatting, and pre-commit hooks to catch mistakes before they hit the repo.

## RULES
1. Execute all commands without asking
2. Create ALL files with the EXACT content shown

---

## STEP 1: Add Ruff for Python Linting & Formatting

### services/api/ruff.toml

```toml
[lint]
select = ["E", "F", "I", "W"]
ignore = ["E501"]

[format]
quote-style = "double"
indent-style = "space"
```

### Update services/api/pyproject.toml

Add to the `[dependency-groups]` dev section:
```toml
dev = [
    "pytest>=8.0.0",
    "pytest-asyncio>=0.23.0",
    "httpx>=0.27.0",
    "ruff>=0.4.0",
]
```

### Test ruff works

```bash
cd services/api
uv sync
uv run ruff check app/
uv run ruff format app/ --check
```

---

## STEP 2: Add Pre-commit Hooks for Python

### services/api/.pre-commit-config.yaml

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.4.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format

  - repo: local
    hooks:
      - id: pytest
        name: pytest
        entry: uv run pytest
        language: system
        pass_filenames: false
        always_run: true
```

### Install pre-commit

```bash
cd services/api
uv add --dev pre-commit
uv run pre-commit install
```

### Test pre-commit works

```bash
cd services/api
uv run pre-commit run --all-files
```

---

## STEP 3: Add Swift Linting (Optional)

SwiftLint requires Homebrew installation:

```bash
brew install swiftlint
```

### apps/macos-client/.swiftlint.yml

```yaml
included:
  - Sources

disabled_rules:
  - line_length
  - trailing_whitespace

opt_in_rules:
  - empty_count
  - force_unwrapping
```

### Test swiftlint works

```bash
cd apps/macos-client
swiftlint
```

---

## STEP 4: Add Git Hooks to Workspace

Create a workspace-level pre-commit that runs both:

### .githooks/pre-commit

```bash
#!/bin/bash
set -e

echo "Running Python checks..."
cd services/api
uv run ruff check app/ --fix
uv run ruff format app/
uv run pytest -q

echo "Running Swift checks..."
cd ../../apps/macos-client
swift build

echo "All checks passed!"
```

### Enable the hook

```bash
chmod +x .githooks/pre-commit
git config core.hooksPath .githooks
```

---

## DONE!

Guardrails configured:
- Ruff linting and formatting for Python
- Pre-commit hooks for automatic checks
- SwiftLint for Swift (if installed)
- Workspace-level git hooks

Now every commit will be checked automatically.
