---
name: implementation
description: Execute implementation plans incrementally using Claude Code plan mode. Reviews each item after completion.
---

# Implementation

Execute implementation plans incrementally using Claude Code's native plan mode for each task item.

## Quick Start

| Command | Effect |
|---------|--------|
| `/implementation` | Execute from context or prompt |
| `/implementation [plan description]` | Execute a specific plan |

## Task

$ARGUMENTS

---

## Phase 0: Find Plan

### 0.1 Locate the Plan

Check for a plan to execute:
1. If `$ARGUMENTS` references a plan file, read it
2. If working in a project with a `plan.md` or `plans/` directory, look there
3. If no plan found, ask the user

### 0.2 Parse Plan

From the plan, extract:
- Implementation phases/tasks
- Files to modify per phase
- Success criteria per phase
- Risks and mitigations

---

## Phase 1: Select Next Item

### 1.1 Find Next Work

Scan the plan for the next incomplete item:
- Look for unchecked items (`- [ ]`)
- Follow phase order (don't skip ahead)
- Respect dependencies between phases

### 1.2 Present Work

Show the user what you're about to work on:

```typescript
AskUserQuestion({
  questions: [{
    question: `Ready to work on Phase ${N}: ${phaseName}? (${taskCount} tasks)`,
    header: "Next Phase",
    options: [
      { label: "Yes, start", description: "Begin implementation" },
      { label: "Show details", description: "See task descriptions first" },
      { label: "Skip to next", description: "Skip this phase" }
    ],
    multiSelect: false
  }]
})
```

---

## Phase 2: Plan Implementation

**REQUIRED**: Before making code changes, enter Claude Code's plan mode.

1. **Call `EnterPlanMode` tool**
2. **Create implementation plan** covering:
   - Steps to implement the current task
   - Files to modify
   - Verification commands (lint, tests)
3. **Call `ExitPlanMode` tool** for user approval

Only after the user approves the plan should you begin writing code.

---

## Phase 3: Implement

Execute the approved plan:

### 3.1 Make Changes

1. **Read before editing** — Always understand existing code first
2. **One logical change at a time** — Don't batch unrelated changes
3. **Stay within scope** — Only what's in the plan

### 3.2 Verify

After each change, verify it works:

| Level | When | What |
|-------|------|------|
| **Per-edit** | After each file change | Lint, syntax check |
| **Per-phase** | After completing a phase | Run relevant tests |
| **End-to-end** | After all phases | Full test suite |

### 3.3 Track Progress

Update the plan as you go:
- Mark completed items: `- [x]`
- Note any discoveries or deviations
- Flag blocked items with reason

### 3.4 Prompt Next Action

After completing each phase:

```typescript
AskUserQuestion({
  questions: [{
    question: "Phase complete. What's next?",
    header: "Continue",
    options: [
      { label: "Next phase", description: "Continue to next incomplete phase" },
      { label: "Review work", description: "Review what was just implemented" },
      { label: "Done for now", description: "Pause implementation" }
    ],
    multiSelect: false
  }]
})
```

---

## Phase 4: Completion

All items implemented.

### 4.1 Final Verification

Run all verification steps from the plan:
- Lint and format
- Test suite
- Any manual verification steps

### 4.2 Summary

Provide:
1. **What was built**: Files changed, tests added
2. **Deviations from plan**: Anything that changed during implementation
3. **Next steps**: Commit, additional testing, documentation

---

## Anti-Patterns

| Don't | Do | Why |
|-------|-----|-----|
| Start coding without reading | Read first, understand patterns | Consistency matters |
| Make sweeping changes | Change only what's needed | Minimize blast radius |
| Skip verification | Verify after each edit | Catch problems early |
| Batch unrelated changes | One logical change at a time | Easier review/rollback |
| Guess at requirements | Ask via `AskUserQuestion` | Avoid rework |

## See Also

- `planning` - Create plans before implementing
- `interview` - Clarify requirements
- `rpi` - Full Research-Plan-Implement workflow
