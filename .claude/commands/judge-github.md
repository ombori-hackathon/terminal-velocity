# GitHub Practices Analysis Agent

You are analyzing a hackathon participant's GitHub practices for scoring **1A Compliance (contributes to 2 pts)** and **2D Documentation (1 pt)**.

## Input
Participant repo path: $ARGUMENTS

## Best Practices Reference

### GitHub Workflow Best Practices:

**Pull Requests**
- Feature branches merged via PRs (not direct pushes)
- Descriptive PR titles and descriptions
- One feature per PR (not massive PRs)
- PR descriptions explain the "why" not just "what"

**Branch Strategy**
- Feature branches named descriptively (feature/, fix/, etc.)
- Main branch protected
- Clean branch history

**Commit Messages**
- Imperative mood ("Add feature" not "Added feature")
- Subject line under 72 chars
- Body explains why when needed
- Co-authored commits indicate Claude Code usage

**Documentation**
- README explains what the project is
- Setup instructions are clear and complete
- Architecture overview present
- API documentation if applicable

## Analysis Tasks

### 1. Fetch PR List
```bash
cd "$REPO_PATH" && gh pr list --state all --limit 50 2>/dev/null || echo "gh CLI not available or no PRs"
```

### 2. Analyze PR Quality (if PRs exist)
```bash
cd "$REPO_PATH" && gh pr list --state all --json number,title,body --limit 10 2>/dev/null
```
Assess PR titles and descriptions for quality.

### 3. Check Branch Patterns
```bash
cd "$REPO_PATH" && git branch -a 2>/dev/null | head -30
```
Look for feature branch naming conventions.

### 4. Analyze Commit Messages
```bash
cd "$REPO_PATH" && git log --oneline --all | head -50
cd "$REPO_PATH" && git log --all --format="%s" | head -30
```
Assess quality of commit messages.

### 5. Check Co-authorship Pattern
```bash
cd "$REPO_PATH" && git log --all --format="%b" | grep -c "Co-Authored-By" 2>/dev/null || echo "0"
cd "$REPO_PATH" && git log --all --oneline | wc -l
```

### 6. Read and Evaluate README
```bash
cat "$REPO_PATH/README.md" 2>/dev/null || cat "$REPO_PATH/readme.md" 2>/dev/null
```
Assess:
- Project description clarity
- Setup instructions presence and quality
- Architecture explanation
- Useful vs boilerplate

### 7. Check for Additional Documentation
```bash
find "$REPO_PATH" -maxdepth 2 -name "*.md" -type f 2>/dev/null | head -20
ls "$REPO_PATH/docs/" 2>/dev/null
```

## Scoring Rubric

### Compliance Contribution (toward 1A)
- PRs used for features (not direct pushes)
- Feature branches with clear naming
- Co-authored commits indicate Claude usage
- Commit messages are meaningful

### 2D. Documentation (0-1 point)
| Score | Criteria |
|-------|----------|
| **1** | Good README, setup instructions, code comments where needed |
| **0** | No documentation or unusable README |

**Criteria for 1 point:**
- README clearly explains what project does
- Setup/install instructions present and accurate
- Any non-obvious decisions documented
- README is project-specific (not just template)

## Output Format

```markdown
## GitHub Practices Analysis: [Participant Name]

### Pull Requests Assessment

**PR Count:** X total PRs
**PR Quality:**
- Titles: [Descriptive/Generic/Poor]
- Descriptions: [Detailed/Brief/None]
- Size: [Appropriate/Too large/Mixed]

**Notable PRs:**
[List 2-3 interesting PRs with brief assessment]

### Branch Strategy

**Branches Found:**
[List key branches]

**Naming Convention:**
- Pattern used: [feature/, fix/, etc.]
- Consistency: [Good/Inconsistent/None]

### Commit Messages

**Sample Messages:**
[List 5-10 representative commit messages]

**Quality Assessment:**
- Format: [Imperative/Mixed/Poor]
- Clarity: [Clear/Vague/Poor]
- Co-authorship: X% of commits

### Documentation (2D Score: X/1)

**README Quality:**
- Project description: [Clear/Vague/Missing]
- Setup instructions: [Complete/Partial/Missing]
- Architecture docs: [Present/Missing]
- Template vs Custom: [Customized/Template only]

**Additional Docs:**
[List other markdown files found]

**Justification:**
[Why this documentation score]

### Compliance Contribution: [Strong/Moderate/Weak]

### Highlights
[Notable positive practices]

### Concerns
[Issues with GitHub workflow]
```

Perform this analysis now for the specified repository.
