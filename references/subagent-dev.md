# Subagent-Driven Development — Orchestration Patterns

## Spawning Pattern (OpenClaw)

```javascript
// For each independent task in the plan:
sessions_spawn({
  task: `Implement task: ${taskDescription}
  
Files to modify: ${filePaths}
Acceptance criteria: ${criteria}
  
Context:
${relevantFileContents}`,
  mode: "run",
  runTimeoutSeconds: 180
});
```

## Review Protocol

### Stage 1: Spec Compliance
Ask:
- Does the output match the task description exactly?
- Are all acceptance criteria met?
- Are there any unrelated changes?
- Did it modify only the specified files?

### Stage 2: Code Quality
Ask:
- Is the code readable and well-structured?
- Are there tests? Do they pass?
- Any obvious performance issues?
- Security concerns (injection, hardcoded secrets, etc.)?
- Does it follow project conventions?

## Handling Review Failures

```
Review failed? → Send specific feedback:
"The CSV export is missing header row. 
Expected: name,email,date
Got: just data rows.
Fix: Add headers as first line in toCSV function."

NOT: "It doesn't work, try again."
```

## Parallel vs Sequential

```
Tasks A, B, C are independent (different files)
→ Spawn all 3 in parallel
→ Review each as they complete

Task D depends on A
→ Wait for A to complete + pass review
→ Then spawn D with A's output as context
```

## When NOT to Use Subagents

- Task takes < 5 minutes → just do it
- Task requires deep context of the full codebase → do it in main session
- Task is exploratory (you don't know what the output looks like) → investigate first, then delegate
- You're debugging → use systematic-debugging, not subagents
