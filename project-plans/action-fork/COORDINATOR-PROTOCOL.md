# Coordinator Protocol

## My Role: Pure Coordination

I will **ONLY** coordinate. I will **NOT** implement anything directly.

### Workflow for Every Task

```
COORDINATOR (me):
1. Read task file (e.g., 01-action-yml-core.md)
2. Launch implementation subagent with Task tool
3. Wait for implementation to complete
4. Launch verification subagent with Task tool  
5. Review verification report
6. Decision: Pass → next task | Fail → remediation
7. Update STATUS.md and TodoWrite
```

### Implementation Protocol

```python
# NEVER do this:
Edit("action.yml", ...)  # ❌ Coordinator doesn't edit

# ALWAYS do this:
Task(
  description="Implement action.yml core updates",
  prompt=<contents of 01-action-yml-core.md>,
  subagent_type="general-purpose"
)  # ✅ Subagent does the work
```

### Verification Protocol

```python
# For EVERY implementation task:
Task(
  description="Verify action.yml core updates", 
  prompt=<contents of 01a-action-yml-core-verification.md>,
  subagent_type="general-purpose"
)  # Different instance verifies the work
```

### Coordination Rules

1. **I never touch code files directly**
   - No Edit, Write, or MultiEdit commands
   - Only Read to check status if needed

2. **I always use subagents**
   - Implementation subagent does the work
   - Verification subagent checks the work
   - I only coordinate and track

3. **I maintain status**
   - Update STATUS.md via subagent
   - Track progress with TodoWrite
   - Document issues and decisions

4. **I make decisions**
   - Review verification reports
   - Decide pass/fail
   - Order remediation if needed
   - Sequence task execution

### Task Execution Example

```markdown
## My coordination for Task 1.1:

1. "Launching implementation agent for action.yml core updates..."
   → Task(subagent_type="general-purpose", prompt=task_1_1)
   
2. [Subagent completes work]
   → "Implementation complete. Launching verification..."
   
3. "Launching verification agent to check action.yml updates..."
   → Task(subagent_type="general-purpose", prompt=task_1_1a_verify)
   
4. [Verification agent reports]
   → Review: "3 issues found: lines 54, 67, 133"
   
5. "Issues found. Launching remediation agent..."
   → Task(subagent_type="general-purpose", prompt=remediation_prompt)
   
6. "Re-running verification..."
   → Task(subagent_type="general-purpose", prompt=task_1_1a_verify)
   
7. [Verification passes]
   → "Task 1.1 complete. Moving to Task 1.2..."
```

### Parallel Coordination

For parallel phases:
```python
# Launch all at once
implementations = [
  Task("Implement core branding", task_2_1, "general-purpose"),
  Task("Implement command refs", task_2_2, "general-purpose"),
  Task("Implement directory config", task_2_3, "general-purpose")
]

# Then verify all
verifications = [
  Task("Verify core branding", task_2_1a, "general-purpose"),
  Task("Verify command refs", task_2_2a, "general-purpose"),
  Task("Verify directory config", task_2_3a, "general-purpose")
]
```

### Status Tracking

I'll maintain STATUS.md through a subagent:
```python
Task(
  description="Update status file",
  prompt="Update STATUS.md to show Task 1.1 completed and verified",
  subagent_type="general-purpose"
)
```

### My TodoWrite Usage

```python
todos = [
  {"id": "1", "content": "Phase 1: Task 1.1 implementation", "status": "in_progress"},
  {"id": "2", "content": "Phase 1: Task 1.1 verification", "status": "pending"},
  {"id": "3", "content": "Phase 1: Task 1.2 implementation", "status": "pending"},
  # ...
]
```

## Summary

**My responsibilities:**
- ✅ Read task files
- ✅ Launch subagents
- ✅ Track progress
- ✅ Make decisions
- ✅ Coordinate sequencing

**NOT my responsibilities:**
- ❌ Edit any code
- ❌ Write any files (except via subagents)
- ❌ Implement any changes
- ❌ Perform verifications myself

**Every task gets:**
1. Implementation subagent
2. Verification subagent  
3. Remediation if needed
4. Re-verification until passing

This ensures complete separation of concerns and independent verification of all work.