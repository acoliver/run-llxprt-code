# Task 2.2: Command Reference Updates

## Objective
Replace all @gemini-cli references with @llxprt throughout the codebase, updating all command examples and triggers.

## Context
The action uses `@gemini-cli` as the command trigger in GitHub comments. All these references need to be updated to `@llxprt` for the new branding.

## Files to Search and Update
Search the entire repository for `@gemini-cli` references:
- `/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/**/*.md`
- `/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/**/*.yml`
- `/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/**/*.yaml`

## Specific Changes Required

### 1. Simple Command References
Replace all instances:
```
@gemini-cli → @llxprt
```

### 2. Command Examples
Update all command examples:
```markdown
# Old:
@gemini-cli /review
@gemini-cli /triage
@gemini-cli explain this code

# New:
@llxprt /review
@llxprt /triage
@llxprt explain this code
```

### 3. Bot Identity References
Update bot username references:
```
gemini-cli[bot] → llxprt[bot]
```

### 4. Workflow Trigger Patterns
In workflow files, update comment matching patterns:
```yaml
# Old:
if: contains(github.event.comment.body, '@gemini-cli')

# New:
if: contains(github.event.comment.body, '@llxprt')
```

### 5. Documentation Examples
Update all documentation showing command usage:
```markdown
# Old:
Comment `@gemini-cli /review` on a pull request

# New:
Comment `@llxprt /review` on a pull request
```

## Implementation Instructions

1. Use grep to find ALL instances of `@gemini-cli` in the repository
2. Use grep to find ALL instances of `gemini-cli[bot]`
3. Replace each instance with the appropriate llxprt equivalent
4. Pay special attention to:
   - README.md examples
   - Workflow files (.github/workflows/*.yml)
   - Example workflows (examples/workflows/**/*.yml)
   - Documentation files (docs/*.md)
5. Ensure context makes sense after replacement

## Special Considerations

### Workflow Files
Some workflows may have multiple references:
- In trigger conditions
- In comment parsing logic
- In bot user setup
- In example comments

### Example Files
The examples/workflows directory likely has many duplicated examples. Update them all consistently.

### Case Sensitivity
- `@gemini-cli` → `@llxprt` (lowercase)
- `gemini-cli[bot]` → `llxprt[bot]` (lowercase)
- Don't change `GEMINI_API_KEY` environment variable names

## Validation

After implementation:
1. No `@gemini-cli` references remain (except in migration docs if any)
2. All command examples use `@llxprt`
3. Bot identity uses `llxprt[bot]`
4. Workflow triggers look for `@llxprt`
5. Documentation examples are consistent

## Search Commands to Use

```bash
# Find all @gemini-cli references
grep -r "@gemini-cli" /Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/

# Find all gemini-cli[bot] references
grep -r "gemini-cli\[bot\]" /Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/

# Find any remaining gemini-cli references
grep -r "gemini-cli" /Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/
```

## Important Notes

- This is likely to be 100+ replacements across many files
- Be systematic - check every file type
- Don't change `gemini-cli` in URLs that reference the original project
- Don't change historical references or attribution
- Focus on command triggers and bot identity

## Expected Outcome

All user-facing command references will:
1. Use `@llxprt` as the trigger
2. Show `llxprt[bot]` as the bot identity
3. Have consistent examples throughout
4. Work correctly in workflow triggers