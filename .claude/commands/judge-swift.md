# Swift/macOS Analysis Agent

You are analyzing a hackathon participant's Swift/macOS code for scoring **2A Architecture (contributes to 2 pts)** and **2B Code Quality (contributes to 2 pts)**.

## Input
Participant repo path: $ARGUMENTS

## Best Practices Reference

### SwiftUI/macOS Best Practices:

**Project Structure**
- Clear separation: Views/, Models/, ViewModels/, Services/
- Proper use of SwiftUI App lifecycle
- Assets organized appropriately

**SwiftUI Patterns (Modern Swift 5.9+)**
- `@State` for view-local state
- `@StateObject` for owned ObservableObjects (older pattern)
- `@Observable` macro (Swift 5.9+, preferred over @ObservableObject)
- `@Bindable` for @Observable objects in views
- `@ObservedObject` for passed-in ObservableObjects
- `@EnvironmentObject` for app-wide shared state
- `@Binding` for two-way data passing
- `@MainActor` for main-thread-bound classes (IMPORTANT for ViewModels)
- `actor` keyword for thread-safe singletons (e.g., APIClient)

**Code Quality Indicators**
- Proper optional handling (guard let, if let, nil coalescing)
- No force unwraps (!) except where truly safe
- Error handling with do-catch, Result types
- Custom error types with `LocalizedError` conformance
- Async/await for asynchronous operations
- `Task { }` for launching async work from sync contexts
- Clear naming conventions (verbs for functions, nouns for types)
- `Codable` with `CodingKeys` for snake_case API mapping
- Combine framework usage where appropriate (`@Published`, `.sink`)

**Anti-Patterns to Flag**
- Force unwraps everywhere
- Massive views with no extraction
- Business logic in views
- Improper state management
- Missing error handling

## Analysis Tasks

### 1. Check Project Structure
```bash
ls -la "$REPO_PATH/apps/macos-client/" 2>/dev/null
find "$REPO_PATH/apps/macos-client" -type d -name "Sources" -o -name "Views" -o -name "Models" -o -name "ViewModels" -o -name "Services" 2>/dev/null
```

### 2. Find Key Swift Files
```bash
find "$REPO_PATH/apps/macos-client" -name "*.swift" 2>/dev/null | head -20
```

### 3. Read and Evaluate Main Files
Read the main App file, ContentView, and 2-3 key view/model files.
Assess:
- SwiftUI patterns used
- State management approach
- Code organization within files

### 4. Check for Modern Patterns and Anti-Patterns
```bash
# Force unwraps (anti-pattern)
grep -rn "!" "$REPO_PATH/apps/macos-client" --include="*.swift" | grep -v "//\|!=\|!=" | head -20

# Check for state property wrappers (both old and modern)
grep -rn "@State\|@StateObject\|@ObservedObject\|@EnvironmentObject\|@Binding" "$REPO_PATH/apps/macos-client" --include="*.swift" | head -20

# Check for modern Swift 5.9+ patterns (IMPORTANT - these indicate quality)
grep -rn "@Observable\|@Bindable\|@MainActor" "$REPO_PATH/apps/macos-client" --include="*.swift" | head -20

# Check for actor-based thread safety (excellent pattern)
grep -rn "^actor \|actor " "$REPO_PATH/apps/macos-client" --include="*.swift" | head -10
```

### 5. Error Handling Assessment
```bash
# Look for error handling patterns
grep -rn "do {\|catch\|Result<\|throws\|try " "$REPO_PATH/apps/macos-client" --include="*.swift" | head -20
```

### 6. Check for Async Patterns
```bash
grep -rn "async\|await\|Task {" "$REPO_PATH/apps/macos-client" --include="*.swift" | head -15
```

## Scoring Contribution

### Architecture Contribution (toward 2A)
- Clean folder structure with separation of concerns
- Proper SwiftUI app structure
- Logical organization of views, models, services

### Code Quality Contribution (toward 2B)
| Aspect | Good | Poor |
|--------|------|------|
| Optionals | guard let, if let, proper nil handling | Force unwraps everywhere |
| State | Correct property wrappers | Wrong wrapper types, state in wrong places |
| Error handling | do-catch, Result types | No error handling, crashes on failure |
| Async | async/await, Task | Blocking calls, callback hell |
| Naming | Clear, descriptive | Cryptic, inconsistent |

## Output Format

```markdown
## Swift/macOS Analysis: [Participant Name]

### Architecture Assessment

**Project Structure:**
- Folder organization: [Description]
- Separation of concerns: [Good/Moderate/Poor]
- Key directories found: [List]

**SwiftUI App Structure:**
- Entry point: [Assessment]
- View hierarchy: [Assessment]

### Code Quality Assessment

**State Management:**
- Property wrappers used: [List with counts]
- Correctness: [Assessment]
- Issues found: [List if any]

**Optional Handling:**
- Force unwraps found: [Count and assessment]
- Safe unwrapping patterns: [Examples]

**Error Handling:**
- Patterns found: [do-catch, Result, etc.]
- Coverage: [Good/Partial/Missing]

**Async Patterns:**
- async/await usage: [Yes/No]
- Task handling: [Assessment]

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
