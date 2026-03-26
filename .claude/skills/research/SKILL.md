---
name: research
description: Research a topic and document findings. Use when exploring a new domain, investigating technologies, gathering information before decisions, comparing approaches, or when user says "research", "investigate", "explore", "what are my options", or "learn about".
---

# Research

Deep exploration of a topic with consistent documentation structure. Research without the pressure to plan or implement.

## Quick Start

```
Research Claude Agent SDK best practices
Research "state management patterns in React"
```

## Depth Levels

| Depth | Parallel Tasks | Documentation |
|-------|---------------|---------------|
| Quick | 2-3 | Summary only |
| Standard | 3-5 | Summary + key findings |
| Thorough | 5+ | Full structure with sources |

Prefix your request to set depth: `quick:`, `deep:`, or no prefix for standard.

## Task

$ARGUMENTS

---

## Phase 1: Define Scope

### 1.1 Parse Arguments

Check for depth prefix in `$ARGUMENTS`:
- `quick:<topic>` - Quick scan (2-3 tasks, summary-level)
- `deep:<topic>` - Thorough (5+ tasks, verification required)
- No prefix - Standard (3-5 tasks)

### 1.2 Define Research Questions

Before exploring, establish specific questions:

1. What problem does this solve?
2. What are the main approaches/patterns?
3. What are the trade-offs?
4. What are the best practices?
5. What are the common pitfalls?

---

## Phase 2: Research Execution

### 2.1 Choose Research Tools

| Research Need | Tool | When to Use |
|---------------|------|-------------|
| **Web research** | `WebSearch` then `WebFetch` | Industry practices, external docs, blog posts |
| **Codebase exploration** | `Task` (Explore agent) | Finding patterns, understanding existing code |
| **Library docs** | WebSearch/WebFetch | API questions, version-specific docs |

### 2.2 Parallel Execution

Spawn 3-5 research tasks simultaneously. Mix tools based on topic:

**Technology Research Example:**
- WebSearch: "Best practices for X in 2026"
- WebSearch: "Common pitfalls with X"
- Explore agent: "How does our codebase currently handle Y?"

**Domain/Problem Research Example:**
- WebSearch: "Industry approaches to X"
- WebSearch: "Case studies for Y"
- Explore agent: "How does our system currently handle this?"

### 2.3 Research Constraints

- Scope each research task to specific focus (avoid overlap)
- If a search finds nothing useful, note the negative finding
- Don't spawn more tasks to fill gaps - document unknowns

### 2.4 Synthesize Findings

After tasks complete:
1. Merge overlapping discoveries
2. Resolve contradictions (flag if unresolvable)
3. Identify gaps requiring follow-up

---

## Phase 2.5: Verify & Assess

Before documenting, verify findings and assess confidence.

### Confidence Assessment

| Confidence | Criteria |
|------------|----------|
| **High** | 2+ independent sources agree; direct quotes available |
| **Medium** | 1 source with direct evidence; or 2+ sources with partial alignment |
| **Low** | Single source; inference required; or sources conflict |

### Source Quality Assessment

| Factor | Rating |
|--------|--------|
| **Authority** | Official docs > expert blog > general web |
| **Recency** | Published/updated within 2 years |
| **Specificity** | Directly addresses our question |

---

## Phase 3: Document Findings

### 3.1 Create Research File

Create a `research/<topic-slug>.md` file:

```markdown
# <Title>

## Executive Summary

<2-3 paragraphs synthesizing key findings>

## Key Findings

| Finding | Confidence | Details |
|---------|------------|---------|
| Finding 1 summary | High | Details... |
| Finding 2 summary | Medium | Details... |

## Evidence

For each finding, include:
- Source reference
- Direct quote or evidence
- Confidence reasoning

## Recommendations

- <Actionable recommendation 1>
- <Actionable recommendation 2>

## Sources

| Source | Type | Quality |
|--------|------|---------|
| [Official Docs](url) | Doc | High |
| [Blog Post](url) | Web | Medium |

## Next Steps

- [ ] <If user wants to proceed, what's first?>
```

---

## Phase 4: Present Summary

Output the executive summary to the user with:
- Key findings
- Open questions (if any)
- Suggested next steps

Then offer:

```typescript
AskUserQuestion({
  questions: [{
    question: "Research complete. What would you like to do next?",
    header: "Next Step",
    options: [
      { label: "Continue to Planning", description: "Create implementation plan based on findings" },
      { label: "Done for now", description: "Save research and exit" }
    ],
    multiSelect: false
  }]
})
```

---

## Anti-Patterns

| Don't | Do |
|-------|-----|
| Jump straight to conclusions | Explore multiple angles first |
| Research forever | Time-box; "good enough" is fine |
| Skip documentation | Write findings as you go |
| Skip verification | Assess confidence for each finding |
| Ignore negative findings | Document what you didn't find too |

## See Also

- `planning` - Create implementation plans after research
- `interview` - Requirements gathering before research
- `rpi` - Full Research-Plan-Implement workflow
