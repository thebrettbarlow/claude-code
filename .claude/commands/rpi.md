---
description: Research-Plan-Implement workflow for non-trivial tasks
---

# RPI: Research, Plan, Implement

The default workflow for non-trivial tasks. Adapts the **40/20/40 principle** (invest heavily in understanding before acting):
- 40% Research: Understand before acting
- 20% Planning: Get explicit approval
- 40% Implementation: Execute with validation

## Task

$ARGUMENTS

## When RPI is Required

| Task Type | Required | Why |
|-----------|----------|-----|
| New feature | **Yes** | Need to understand existing patterns |
| Bug fix (cause unknown) | **Yes** | Research phase finds root cause |
| Refactoring | **Yes** | Need to plan scope |
| Bug fix (cause known) | No | If simple, just fix it |
| Single-file change | No | Overkill |
| Documentation | No | Just do it |

If this task doesn't require RPI, say so and proceed directly.

---

## Phase 1: Research (40%)

**Goal**: Understand before acting. Never propose changes to code you haven't read.

### 1.1 Parallel Exploration

Spawn 3-5 Explore agents simultaneously to research different angles:

```
Agent 1: Find existing implementations of similar features
Agent 2: Find related tests and test patterns
Agent 3: Look for configuration, settings, or constants
Agent 4: Check documentation and comments
Agent 5: Find similar patterns elsewhere in codebase
```

**Agent constraints**:
- Scope each agent to specific search focus (avoid overlap)
- If agent finds nothing useful, note the negative finding and move on
- Don't spawn more agents to fill gaps — document unknowns instead

### 1.2 Synthesize Agent Findings

After agents complete, consolidate into single understanding:
- Merge overlapping discoveries
- Resolve contradictions (flag for user if unresolvable)
- Identify gaps requiring follow-up

### 1.3 Research Checklist

- [ ] Understand existing patterns and conventions
- [ ] Identify all files that will be affected
- [ ] Find related tests
- [ ] Understand dependencies and constraints
- [ ] Document any technical debt or gotchas discovered

### 1.4 Research is Complete When

- Core patterns and conventions are understood
- Existing approaches to similar problems documented
- Technical constraints identified
- Enough context to make informed planning decisions

**Anti-pattern**: Analysis paralysis. Time-box research. "Good enough" over perfect.

---

## Phase 1.5: Requirements Interview

**After research, refine the approach with the user.** Research grounds interview questions in actual code findings rather than abstract speculation.

Use the `interview` skill. Key guidance:

- **Default: interview.** Only skip when the task is unambiguous AND single-scope.
- **Multi-item tasks (2+ distinct changes)**: Always interview — confirm scope, UX, priorities.
- **Ground questions in research findings**: "I see X in file Y, should we Z?" — not abstract speculation.
- Inject interview results into the plan context.

---

## Phase 2: Plan (20%)

**Goal**: Get explicit user approval before implementing.

### 2.1 Enter Plan Mode

Use Claude Code's Plan Mode:
- Use `EnterPlanMode` tool to activate read-only planning
- Exit plan mode only after user approves

### 2.2 Plan Structure

```markdown
# Plan: {Feature Name}

**Task**: {original task description}
**Status**: Draft | Approved | Completed

## Summary
{1-2 sentence overview}

## Research Findings
{Key insights from research phase}

## Approach
{Chosen approach and why}

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

## Out of Scope
{Things we're explicitly NOT doing}
```

### 2.3 Get Approval

Present the plan. **Do not proceed until user explicitly approves.**

### 2.4 If Plan Rejected

- **Plan rejected**: Return to research with rejection reasons as new constraints
- **Minor changes**: Update plan, re-present
- **Mid-implementation scope change**: Pause, update plan, get re-approval

---

## Phase 3: Implement (40%)

**Goal**: Execute the approved plan with validation.

### 3.1 Choose Execution Strategy

| Plan Shape | Strategy |
|------------|----------|
| 1-2 phases | **Direct**: Implement sequentially |
| 3+ independent tasks | **Delegate**: Parallel subagents + review |
| Mix | **Hybrid**: Sequential for dependencies, parallel for independent |

### 3.2 Execution

1. **One phase at a time** — Complete each phase before starting next
2. **Validate after each** — Run tests, verify success criteria
3. **Stay within scope** — Only what's in the plan
4. **Review subagent work** — Read every file changed before marking complete

### 3.3 After All Phases Complete

- Run full verification from plan's success criteria
- Summarize what was built
- Offer to commit

---

## Warning Signs (Stop and Reassess)

Stop immediately if you notice:

- Can't explain what you just wrote
- Tests failing but proceeding anyway
- Making changes outside the plan
- Code doesn't follow existing patterns
- Touching way more files than expected
- "Just one more small change" syndrome

**Action**: Stop. Return to planning. Reassess.

---

## Quick Reference

| Phase | Time | Key Activity | Output |
|-------|------|--------------|--------|
| Research | 40% | Parallel exploration | Understanding |
| Interview | -- | Refine approach with user | Design decisions |
| Plan | 20% | Detailed plan + approval | Approved plan |
| Implement | 40% | Direct or delegated execution | Working code |

---

Now proceeding with RPI workflow for: **$ARGUMENTS**
