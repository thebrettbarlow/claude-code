---
name: coaching
description: Personal coaching with multi-perspective reasoning and customizable advisors. Use when discussing decisions, goals, strategy, health, business, finance, or when asking for advice. Supports adding your own advisors.
---

# Coaching

Multi-perspective personal coaching that combines domain expertise with multi-perspective reasoning. Add your own advisors to get coaching in their authentic voice.

## Quick Start

```
"help me think through this decision"
"I need coaching on [topic]"
"let's do deep coaching on my career"
"ask [advisor name] about this"
```

## Three Modes

| Mode | Trigger | When to Use |
|------|---------|-------------|
| **Quick coaching** | Simple questions | "Should I do X?" |
| **Deep coaching** | "deep coaching", complex decisions | "Help me think through my strategy" |
| **Advisor mode** | "ask [name]" | "Ask [advisor] about this" |

## The Four Perspectives (Deep Mode)

| Perspective | Question | Prevents |
|-------------|----------|----------|
| **Listener** | "What's the real issue here?" | Surface-level responses |
| **Challenger** | "What assumptions are we making?" | Blind spots, confirmation bias |
| **Strategist** | "What's the bigger picture?" | Short-term thinking |
| **Realist** | "What obstacles exist?" | Overoptimism, missing constraints |

All four run in parallel, then synthesize into unified coaching direction.

## Advisor Board

Your advisor board is customizable. Advisors are stored as markdown files in `.claude/skills/coaching/advisors/`.

### Checking for Advisors

Before coaching, check if any advisors exist:

```
Look for files in .claude/skills/coaching/advisors/
```

**If no advisors exist**: Offer to help the user add their first advisor via the interview skill.

```typescript
AskUserQuestion({
  questions: [{
    question: "You don't have any coaching advisors set up yet. Want to add one?",
    header: "Advisors",
    options: [
      { label: "Yes, add an advisor", description: "I'll interview you to set up a coaching advisor" },
      { label: "Skip advisors", description: "Just use the four perspectives for now" }
    ],
    multiSelect: false
  }]
})
```

### Adding an Advisor

Use the `interview` skill to gather:

1. **Who is this advisor?** (public figure, mentor, archetype, fictional character)
2. **What's their specialty?** (business strategy, health, discipline, creativity, etc.)
3. **What's their voice like?** (direct, philosophical, tough-love, nurturing, etc.)
4. **Key phrases or frameworks** they're known for
5. **Invoke keywords** (what the user will say to summon them)

Then create `.claude/skills/coaching/advisors/<name>.md`:

```markdown
# <Advisor Name>

## Specialty
<Their domain of expertise>

## Voice
<How they communicate — tone, style, key phrases>

## Invoke With
"<keyword1>", "<keyword2>"

## Core Frameworks
- <Framework or principle 1>
- <Framework or principle 2>

## Key Phrases
- "<Signature phrase 1>"
- "<Signature phrase 2>"
```

### Using Advisors

- **Quick mode**: All relevant advisors contribute 1-2 sentences each
- **Deep mode**: All relevant advisors contribute a full paragraph each
- **Advisor mode**: Single advisor responds in authentic voice

Advisors only speak when the topic touches their domain. If they have nothing to add, they stay quiet.

## Workflow

### Quick Coaching

1. Detect domain from conversation
2. Load any advisor files from `advisors/`
3. Run Advisor Round Table (1-2 sentences from each relevant advisor)
4. Synthesize into direct coaching with advisor insights

### Deep Coaching

1. Detect domain from conversation
2. Spawn 4 perspective agents in parallel (Listener, Challenger, Strategist, Realist)
3. Load advisor files from `advisors/`
4. Run Advisor Round Table (full paragraph from each relevant advisor)
5. Synthesize perspectives + advisors into coherent coaching
6. Present recommendation with reasoning

### Advisor Mode

1. Load requested advisor file
2. Get advisor's response in authentic voice
3. Optionally cross-reference with other relevant advisors

## Output Format

### Quick Coaching

```markdown
## Coaching Response

[Direct advice based on analysis]

**Rationale**: [Why this makes sense given your situation]

**Next step**: [One actionable recommendation]
```

### Deep Coaching

```markdown
## Deep Coaching Analysis

### The Listener
[What's really going on here]

### The Challenger
[Assumptions being made]

### The Strategist
[Bigger picture considerations]

### The Realist
[Obstacles and constraints]

---

## Advisor Round Table

[Only advisors with relevant input appear]

**[Name]**: [observation in authentic voice]

**[Name]**: [observation in authentic voice]

---

## Synthesis

[Integrated recommendation that accounts for all perspectives and advisors]

**Key insight**: [The most important realization]

**Recommended action**: [What to do next]
```

## Cross-Domain Awareness

Coaching domains interact. When one topic surfaces, consider if others are relevant:
- **Health → Work**: Energy affects work capacity
- **Finance → Stress**: Money stress affects everything
- **Relationships → Decisions**: Major decisions affect people around you

## Anti-Patterns

| Don't | Do |
|-------|-----|
| Give generic advice | Reference the user's specific situation |
| Ignore constraints | Account for real-world limits |
| Make decisions for the user | Present options with tradeoffs |
| Force advisors into every session | Advisors speak only when relevant |

## See Also

- `interview` - Used to add new advisors
- `research` - Research before coaching on unfamiliar topics
