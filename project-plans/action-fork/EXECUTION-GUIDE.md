# Execution and Coordination Guide

## How to Execute This Plan

### Execution Strategy

I will act as the **coordinator** and use the Task tool to spawn specialized subagents for each task. Here's the workflow:

```
Coordinator (me)
    ├── Launch implementation agent for task
    ├── Wait for completion
    ├── Launch verification agent to check work
    ├── Review verification results
    └── Proceed to next task or remediate issues
```

### Phase-by-Phase Execution

#### Phase 1: Core Action Updates (Critical Path)
```bash
# These must be done in sequence
Task 1.1: action.yml core updates → Verify 1.1a
Task 1.2: Provider inputs → Verify 1.2a  
Task 1.3: CLI installation → Verify 1.3a
Task 1.4: CLI execution → Verify 1.4a
```

#### Phase 2: Branding (Can be parallelized)
```bash
# These can run in parallel
Task 2.1: Core branding ─┐
Task 2.2: Command refs  ─┼→ Verify all
Task 2.3: Directory cfg ─┘
```

#### Phase 3: Documentation (Can be parallelized)
```bash
# These can run in parallel after Phase 2
Task 3.1: README providers ─┐
Task 3.2: Auth docs        ─┼→ Verify all
Task 3.3: Workflow examples─┘
```

#### Phase 4: Compatibility & Testing
```bash
Task 4.1: Gemini compatibility → Verify
Task 5.1: Integration tests → Verify
Task 6.1: Final verification
```

### Coordination Commands

Here's how I'll execute each task:

```python
# Example for Task 1.1
Task(
  description="Update action.yml core",
  prompt=open("01-action-yml-core.md").read(),
  subagent_type="general-purpose"
)

# After completion, verify
Task(
  description="Verify action.yml updates",
  prompt=open("01a-action-yml-core-verification.md").read(),
  subagent_type="general-purpose"
)
```

### Tracking Progress

I'll maintain a status file to track progress:

```markdown
# project-plans/action-fork/STATUS.md

## Phase 1: Core Action Updates
- [x] Task 1.1: action.yml core ✅ Verified
- [x] Task 1.2: Provider inputs ✅ Verified
- [ ] Task 1.3: CLI installation 🔄 In Progress
- [ ] Task 1.4: CLI execution ⏳ Pending

## Issues Found
- Task 1.2: Missing environment variable export (FIXED)
```

### Parallel Execution Strategy

For parallel tasks, I'll launch multiple agents simultaneously:

```python
# Launch parallel tasks
agents = [
  Task("Core branding", prompt_2_1, "general-purpose"),
  Task("Command refs", prompt_2_2, "general-purpose"),
  Task("Directory config", prompt_2_3, "general-purpose")
]

# Wait for all to complete
# Then launch verification agents for each
```

### Error Handling

When verification fails:

1. **Document the issue** in STATUS.md
2. **Create remediation task** with specific fixes needed
3. **Re-run implementation** with clearer instructions
4. **Re-verify** until passing

### Quality Gates

Before proceeding to next phase:
- All tasks in current phase must be verified ✅
- No blocking issues remaining
- Backward compatibility confirmed
- Core functionality tested

### Time Optimization

To optimize execution time:
1. **Parallelize within phases** where possible
2. **Use multiple subagents** for independent tasks
3. **Batch similar operations** (e.g., all command replacements)
4. **Pre-validate** before expensive operations

### Communication Protocol

Each subagent receives:
1. **Complete context** from task file
2. **Specific file paths** to work on
3. **Clear success criteria**
4. **Validation steps** to self-check

Each subagent returns:
1. **Completion status**
2. **Files modified**
3. **Issues encountered**
4. **Validation results**

## Execution Commands for Human

If you want to monitor or assist:

```bash
# Watch the status
cat project-plans/action-fork/STATUS.md

# Check for remaining gemini references
grep -r "@gemini-cli" . --exclude-dir=project-plans

# Test backward compatibility
# (Create a test workflow using old format)

# Verify the action.yml is valid
npm run test  # If tests exist
```

## Ready to Execute?

Once you give the go-ahead, I will:
1. Start with Phase 1, Task 1.1
2. Track all progress in STATUS.md
3. Report any blocking issues immediately
4. Complete all phases systematically
5. Run final verification

The entire process should take approximately:
- Phase 1: 20-30 minutes
- Phase 2: 15-20 minutes (parallel)
- Phase 3: 20-30 minutes (parallel)
- Phase 4: 15-20 minutes
- **Total: ~1-2 hours** with parallelization

## Success Metrics

The plan succeeds when:
- ✅ All tasks verified
- ✅ Zero breaking changes
- ✅ Multi-provider working
- ✅ Documentation complete
- ✅ Ready to publish