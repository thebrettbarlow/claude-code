---
name: interview
description: Requirements gathering through iterative questioning. Use when user intent is unclear, scope is ambiguous, design decisions need input, or before generating prompts/plans for complex work. Lean towards interviewing when unsure.
---

# Interview

Iterative requirements gathering via `AskUserQuestion`. Extracts tacit knowledge, clarifies intent, and surfaces non-obvious constraints before work begins.

## When to Invoke

**Auto-trigger** when ANY of these apply:

| Signal | Example |
|--------|---------|
| Vague goal | "improve the search" (improve how?) |
| Missing scope | "add auth" (which auth? where?) |
| Implicit design decisions | "add a button" (style? placement? behavior?) |
| Unknown verification | How will we know it's done? |
| High-stakes work | Multi-session, architectural, or costly to redo |
| User explicitly asks | "interview me", "ask me questions" |

**Lean towards interviewing when uncertain.** A few clarifying questions upfront saves hours of rework.

## How It Works

1. Identify **ambiguity categories** in the request
2. Ask 1-4 questions per round using `AskUserQuestion`
3. Analyze responses, identify follow-ups
4. Repeat until requirements are clear
5. Output structured context for downstream use

## Question Design

### Use AskUserQuestion Tool

The tool shows options as clickable chips but **always allows "Other"** for custom input.

```typescript
AskUserQuestion({
  questions: [
    {
      question: "What should happen when the user clicks submit?",
      header: "Behavior",  // Short label (max 12 chars)
      options: [
        { label: "Save and stay", description: "Keep user on current page" },
        { label: "Save and redirect", description: "Navigate to success page" },
        { label: "Save and close", description: "Close modal/drawer" },
        { label: "Show confirmation", description: "Display success message first" }
      ],
      multiSelect: false
    }
  ]
})
```

### Question Principles

| Principle | Why |
|-----------|-----|
| **Ground in findings** | "I see you have X. Should we Y?" |
| **Offer smart defaults** | Mark recommended option: "Save and redirect (Recommended)" |
| **Ask non-obvious questions** | Skip what's clear, probe what's hidden |
| **One concern per question** | Don't combine scope + design + verification |
| **Max 4 questions per round** | Avoid overwhelming; iterate instead |

### Question Categories

| Category | Purpose | Example |
|----------|---------|---------|
| **Goal** | What are we trying to achieve? | "Is the goal to X or Y?" |
| **Scope** | What's in/out of bounds? | "Should this include Z?" |
| **Design** | How should it work/look? | "Which pattern fits better?" |
| **Constraints** | What limits exist? | "Any compatibility requirements?" |
| **Verification** | How do we know it's done? | "What should pass to ship?" |
| **Priority** | What matters most? | "If we can only do one, which?" |

## Interview Flow

### Round 1: Big Picture

Start with highest-uncertainty questions. Often Goal + Scope.

```
Questions to ask myself before asking user:
- What's the core ambiguity?
- What would change my approach most?
- What assumption am I making?
```

### Round 2+: Drill Down

Based on Round 1 answers, probe deeper into specific areas.

```
If user said "both X and Y" → clarify priority
If user said "Other: ..." → explore that direction
If new ambiguity surfaced → address it
```

### Exit Conditions

Stop interviewing when:

- [ ] Goal is concrete and specific
- [ ] Scope boundaries are defined
- [ ] Critical design decisions are made
- [ ] Verification criteria exist
- [ ] No questions remain that would change approach

**Or** user signals: "that's enough", "let's proceed", "good enough"

## Output Format

After interview completes, inject structured context:

```markdown
## Interview Results

**Goal**: [Concrete, specific objective]

**Scope**:
- Includes: [what's in]
- Excludes: [what's out]

**Design Decisions**:
- [Decision 1]: [Choice made] — [why/context]
- [Decision 2]: [Choice made] — [why/context]

**Constraints**:
- [Constraint 1]
- [Constraint 2]

**Verification**:
- [ ] [Criterion 1]
- [ ] [Criterion 2]

**Open Items** (if any):
- [Thing to figure out during implementation]
```

This context is then used by the calling workflow (prompt-writer, rpi, etc.).

## Example Interview

**User input**: "add dark mode"

### Round 1

```
AskUserQuestion({
  questions: [
    {
      question: "What should trigger the dark mode toggle?",
      header: "Trigger",
      options: [
        { label: "System preference", description: "Follow OS dark/light setting" },
        { label: "Manual toggle", description: "User clicks button to switch" },
        { label: "Both (Recommended)", description: "System default with manual override" }
      ],
      multiSelect: false
    },
    {
      question: "Where should the toggle live?",
      header: "Location",
      options: [
        { label: "Header/navbar", description: "Always visible at top" },
        { label: "Settings page", description: "In user preferences" },
        { label: "Both", description: "Quick access + settings" }
      ],
      multiSelect: false
    }
  ]
})
```

### Round 2

```
AskUserQuestion({
  questions: [
    {
      question: "Should dark mode persist across sessions?",
      header: "Persistence",
      options: [
        { label: "Yes, localStorage (Recommended)", description: "Remembers preference" },
        { label: "No, session only", description: "Resets on page refresh" },
        { label: "Sync to account", description: "Requires backend change" }
      ],
      multiSelect: false
    }
  ]
})
```

## Anti-Patterns

| Don't | Do |
|-------|-----|
| Ask about obvious things | Probe hidden assumptions |
| 10 questions at once | Max 4 per round, iterate |
| Generic options | Context-aware choices |
| Skip when unsure | Default to interviewing |
| Over-interview simple tasks | Match depth to complexity |

## Question Count Guidelines

| Task Complexity | Typical Questions |
|-----------------|-------------------|
| Simple clarification | 1-3 |
| Feature with options | 4-8 |
| Architectural decision | 8-15 |
| Multi-system change | 15-30 |

## See Also

- `prompt-writer` - Uses interview to clarify ambiguous prompts
- `rpi` - Uses interview before planning phase
- `planning` - Uses interview to refine synthesized plans
