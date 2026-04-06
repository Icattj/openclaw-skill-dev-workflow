# ---

> OpenClaw AI Agent Skill

---
name: dev-workflow
description: Professional development workflow combining Writing Plans, Test-Driven Development (TDD), and Subagent-Driven Development. Use when implementing any feature, building multi-step projects, or executing complex development tasks. Enforces plan-first, test-first, review-always methodology.
---

# Development Workflow

Three integrated patterns for professional software development.

## 1. Writing Plans (Plan Before Code)

Before writing ANY implementation code for non-trivial tasks:

1. **Define the goal** — one sentence, what does "done" look like?
2. **List tasks** — break into bite-sized pieces (each < 30 min)
3. **Specify files** — exact file paths that will be created/modified
4. **Order tasks** — dependencies first, independent tasks can parallelize
5. **Add verification** — how to test each task is done correctly

### Plan Template
```markdown
# Plan: [Feature Name]

**Goal:** [One sentence]
**Estimated effort:** [X tasks, ~Y hours]

## Tasks

### 1. [Task name]
- **Files:** `path/to/file.js`
- **What:** [Specific description]
- **Verify:** [How to test it works]

### 2. [Task name]
...
```

**Key rules:**
- No task should be vague — "implement the thing" is not a task
- Include code examples for complex changes
- Flag risks and unknowns upfront

See `references/writing-plans.md` for detailed examples.

## 2. Test-Driven Development (TDD)

For every feature or bugfix, follow RED → GREEN → REFACTOR:

### RED: Write a Failing Test First
```bash
# Write the test that describes desired behavior
# Run it — it MUST fail (if it passes, your test is wrong)
```

### GREEN: Write Minimal Code to Pass
```bash
# Write the SIMPLEST implementation that makes the test pass
# No extra features, no premature optimization
# Run tests — they MUST pass
```

### REFACTOR: Clean Up
```bash
# Now improve the code quality
# Extract functions, rename variables, remove duplication
# Run tests after EVERY refactor step — still green?
```

**Key rules:**
- Never write implementation before a test
- One test at a time — don't batch
- Tests should be fast (< 1 second each)
- Test behavior, not implementation details

See `references/tdd.md` for framework-specific patterns.

## 3. Subagent-Driven Development

For multi-task plans with independent work items:

### Process
1. Parse the plan into independent tasks
2. For each task, spawn a fresh subagent (via `sessions_spawn`)
3. Each subagent gets: task description + relevant file context + acceptance criteria
4. On completion, run two-stage review:
   - **Stage 1: Spec compliance** — does it match the plan?
   - **Stage 2: Code quality** — clean, tested, no regressions?
5. If review fails → send feedback to subagent for revision
6. If review passes → merge and move to next task

### When to Use Subagents vs Sequential
| Scenario | Approach |
|----------|----------|
| Tasks are independent (different files) | Parallel subagents |
| Tasks depend on each other | Sequential, one at a time |
| Task is simple (< 5 min) | Do it yourself, skip subagent overhead |
| Task is complex + isolated | Perfect for subagent |

### Review Checklist
- [ ] Matches the task spec exactly
- [ ] No unrelated changes
- [ ] Tests added and passing
- [ ] No new warnings or errors
- [ ] Code is readable and follows project conventions

See `references/subagent-dev.md` for orchestration patterns.

## Choosing Your Workflow

```
Is this a quick fix (< 10 min)?
  YES → Just do it with TDD (RED→GREEN→REFACTOR)
  NO ↓

Is this a multi-step feature?
  YES → Write a plan first, then:
    Are tasks independent? → Subagent-driven
    Are tasks sequential? → TDD each task in order
  NO ↓

Is this a bugfix?
  YES → Use systematic-debugging skill first, then TDD the fix
```

## Installation

```bash
cp -r dev-workflow/ ~/.openclaw/workspace/skills/dev-workflow/
```

## License

MIT © [Sentra Technology](https://github.com/Icattj)
