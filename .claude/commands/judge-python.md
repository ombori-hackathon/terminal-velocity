# Python/API Analysis Agent

You are analyzing a hackathon participant's Python/API code for scoring **2A Architecture (contributes to 2 pts)** and **2B Code Quality (contributes to 2 pts)**.

## Input
Participant repo path: $ARGUMENTS

## Best Practices Reference

### Python API Best Practices:

**Project Structure**
- Clear separation: routes/, models/, services/, schemas/
- Configuration management (config.py, .env)
- Proper package structure with __init__.py files

**FastAPI Patterns**
- Pydantic v2 models for request/response schemas
  - `ConfigDict(from_attributes=True)` for ORM compatibility
  - `Field(...)` with validation (`ge=`, `le=`, `min_length=`)
- Dependency injection with `Depends()` for services and DB sessions
- Proper router organization with `APIRouter`
- Background tasks for async operations
- Proper HTTP status codes
- Custom exception classes with HTTPException

**SQLAlchemy Patterns (2.0+)**
- `DeclarativeBase` or `MappedAsDataclass`
- `Mapped[T]` type hints for columns
- Proper relationship definitions with back_populates
- Index definitions for query optimization

**Code Quality Indicators**
- Type hints on functions and variables
  - Modern syntax: `str | None` instead of `Optional[str]`
  - Generic types: `list[str]` instead of `List[str]`
- Docstrings for complex functions
- Proper exception handling with typed errors
- Async/await for I/O operations
- Following PEP8 conventions
- Logging with `logging.getLogger(__name__)`

**Anti-Patterns to Flag**
- No type hints
- Bare except clauses
- Business logic in route handlers
- Missing validation
- Synchronous blocking calls in async context

## Analysis Tasks

### 1. Check Project Structure
```bash
ls -la "$REPO_PATH/services/api/" 2>/dev/null
find "$REPO_PATH/services/api" -type d 2>/dev/null | head -20
```

### 2. Find Key Python Files
```bash
find "$REPO_PATH/services/api" -name "*.py" 2>/dev/null | head -20
```

### 3. Read and Evaluate Main Files
Read main.py, key route files, and model/schema files.
Assess:
- FastAPI usage patterns
- Code organization
- API design

### 4. Check Type Hints
```bash
# Look for type annotations
grep -rn "def.*->.*:" "$REPO_PATH/services/api" --include="*.py" | head -15
grep -rn ": str\|: int\|: bool\|: list\|: dict\|: Optional" "$REPO_PATH/services/api" --include="*.py" | head -15
```

### 5. Check Error Handling
```bash
# Exception handling
grep -rn "try:\|except\|raise\|HTTPException" "$REPO_PATH/services/api" --include="*.py" | head -20
```

### 6. Check for Pydantic Models
```bash
grep -rn "BaseModel\|Field\|validator" "$REPO_PATH/services/api" --include="*.py" | head -15
```

### 7. Check Async Patterns
```bash
grep -rn "async def\|await " "$REPO_PATH/services/api" --include="*.py" | head -15
```

### 8. Check for Testing
```bash
find "$REPO_PATH/services/api" -name "test_*.py" -o -name "*_test.py" 2>/dev/null
ls "$REPO_PATH/services/api/tests/" 2>/dev/null
```

## Scoring Contribution

### Architecture Contribution (toward 2A)
- Clean folder structure with separation of concerns
- Proper package organization
- Logical separation of routes, models, services

### Code Quality Contribution (toward 2B)
| Aspect | Good | Poor |
|--------|------|------|
| Type hints | Full coverage | None or minimal |
| Validation | Pydantic models | No validation |
| Error handling | HTTPException, proper codes | Bare excepts, 500s |
| Async | async/await where appropriate | Blocking calls |
| Structure | Thin routes, service layer | Fat route handlers |

## Output Format

```markdown
## Python/API Analysis: [Participant Name]

### Architecture Assessment

**Project Structure:**
- Folder organization: [Description]
- Package structure: [Proper __init__.py, etc.]
- Key directories found: [List]

**API Design:**
- Framework: [FastAPI/Flask/other]
- Router organization: [Assessment]
- Endpoint structure: [Assessment]

### Code Quality Assessment

**Type Hints:**
- Coverage: [Good/Partial/None]
- Examples: [Sample patterns found]

**Validation:**
- Pydantic usage: [Yes/No]
- Request validation: [Assessment]
- Response schemas: [Assessment]

**Error Handling:**
- HTTPException usage: [Assessment]
- Exception patterns: [Assessment]
- Issues found: [List if any]

**Async Patterns:**
- async/await usage: [Good/Partial/None]
- Blocking concerns: [Assessment]

**Testing:**
- Test files found: [Yes/No]
- Coverage impression: [Assessment]

### Code Samples Reviewed
[List files read and key observations]

### Architecture Score Contribution: [Strong/Moderate/Weak]
### Code Quality Score Contribution: [Strong/Moderate/Weak]

### Highlights
[Notable positive patterns]

### Concerns
[Anti-patterns or issues found]
```

Perform this analysis now for the specified repository.
