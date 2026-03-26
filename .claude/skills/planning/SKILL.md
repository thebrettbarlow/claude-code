---
name: planning
description: Generate implementation plans using parallel Claude sessions. Use when planning complex features, comparing approaches, or when you want multiple perspectives before implementing.
---

# Planning

Generate robust implementation plans by running multiple parallel thinking sessions, each exploring independently, then synthesizing the results.

## Quick Start

```
/planning Build a user authentication system
/planning quick: Add a dark mode toggle
/planning deep: Redesign the database schema
```

## Task

$ARGUMENTS

---

## How It Works

### Independent Sessions

Multiple planning sessions run the same prompt. Natural variance emerges from:
- Different exploration paths through the codebase
- Different pattern recognition and interpretation
- Different handling of ambiguous requirements

This ensemble approach surfaces blind spots any single session might miss.

### Depth Levels

| Depth | Sessions | When to Use |
|-------|----------|-------------|
| Quick | 1 | Clear approach, small scope |
| Standard | 3 | Default for most features |
| Thorough | 5 | Architecture, security-critical |

---

## Phase 1: Understand the Task

### 1.1 Parse Arguments

Check for depth prefix in `$ARGUMENTS`:
- `quick:<task>` - 1 session
- `deep:<task>` - 5 sessions
- No prefix - 3 sessions (default)

### 1.2 Explore Task Context (Read-Only)

Before planning, explore the codebase to understand the task:

- `Glob` - Find relevant files
- `Grep` - Search for patterns
- `Read` - Examine file contents

**Do NOT modify any files during this phase.**

### Exploration Checklist

- [ ] What existing code relates to this task?
- [ ] What patterns are already established?
- [ ] What files will likely be affected?
- [ ] Are there existing tests or documentation?

---

## Phase 2: Run Parallel Planning

### 2.1 Spawn Planning Sessions

Use `Task` tool to spawn planning agents in parallel:

```typescript
// Spawn N planning sessions with the same prompt
for (let i = 1; i <= numSessions; i++) {
  Task({
    description: `Planning session ${i}`,
    prompt: `Create an implementation plan for: ${taskDescription}

    Explore the codebase, identify patterns, and produce a plan with:
    - Summary of approach
    - Implementation phases with files to modify
    - Success criteria per phase
    - Risks and mitigations
    - Discarded approaches (with rationale)`,
    subagent_type: "Plan",
    run_in_background: true
  })
}
```

### 2.2 Wait for Completion

Monitor all sessions until complete, then collect their plans.

---

## Phase 3: Interactive Synthesis

### 3.1 Read All Plans

Read each plan and note:
- Common patterns (convergence)
- Unique insights from each
- Conflicting approaches (divergence)

### 3.2 Classify Conflicts

| Type | Criteria | Action |
|------|----------|--------|
| **Major** | Architecture, file structure, technology | Ask user via `AskUserQuestion` |
| **Moderate** | Implementation pattern, scope | Ask if >2 phases affected |
| **Minor** | Style, naming, docs | Auto-resolve (convergence wins) |

### 3.3 Resolve Conflicts

**For major conflicts**, use `AskUserQuestion`:

```typescript
AskUserQuestion({
  questions: [{
    question: "Plans disagree on {conflict}. Which approach?",
    header: "{Short label}",
    options: [
      { label: "Option A", description: "{Plan 1 approach}" },
      { label: "Option B", description: "{Plan 2 approach}" }
    ],
    multiSelect: false
  }]
})
```

**Auto-resolve minor conflicts**: convergence wins, then conservative wins.

### 3.4 Write Synthesized Plan

Create the final plan file:

```markdown
# Plan: {Feature Name}

**Task**: {original task description}
**Status**: Draft

## Summary
{1-2 sentence overview of chosen approach}

## Research Findings
{Key insights from exploration phase}

## Implementation Phases

### Phase 1: {Name}
**Files**: {list files to modify}
**Changes**: {what will change}
**Success Criteria**: {how we know it's done}

### Phase 2: {Name}
...

## Risks & Mitigations
| Risk | Mitigation |
|------|------------|
| ... | ... |

## User Decisions
| Decision | Options | Choice | Rationale |
|----------|---------|--------|-----------|
| ... | ... | ... | ... |

## Discarded Approaches
| Approach | Why Discarded |
|----------|---------------|
| ... | ... |

## Out of Scope
{Things we're explicitly NOT doing}
```

### 3.5 Refine via Interview

After writing the plan, invoke the `interview` skill to refine:
- Are the phase boundaries correct?
- Any missing tasks or edge cases?
- Confidence in the approach?

---

## Phase 4: Review & Approve

Present the plan and offer options:

```typescript
AskUserQuestion({
  questions: [{
    question: "Plan complete. What would you like to do next?",
    header: "Next Step",
    options: [
      { label: "Approve & implement", description: "Begin execution" },
      { label: "Revise plan", description: "I have feedback to incorporate" },
      { label: "Done for now", description: "Save as draft" }
    ],
    multiSelect: false
  }]
})
```

- **Approve**: Update status to "Approved", hand off to `/implementation`
- **Revise**: Interview for feedback, update plan, re-present
- **Done**: Keep as Draft for later

---

## Anti-Patterns

| Don't | Do |
|-------|-----|
| Skip exploration | Read existing code first |
| Single-perspective plan | Multiple sessions surface blind spots |
| Plan without user input | Interview to resolve ambiguity |
| Plan forever | Time-box; "good enough" is fine |
| Ignore conflicts between plans | Classify and resolve systematically |

## See Also

- `research` - Explore a topic before planning
- `interview` - Gather requirements before planning
- `implementation` - Execute the plan
- `rpi` - Full Research-Plan-Implement workflow
