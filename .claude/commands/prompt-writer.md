---
description: Transform messy thoughts into clear, effective prompts
---

# Prompt Writer

Transform unstructured text into a well-organized prompt.

## Input

$ARGUMENTS

## Your Task

Analyze the above input and produce a clean, effective prompt.

### Step 1: Understand Intent

Identify from the user's text:
- **Goal**: What are they trying to accomplish?
- **Context**: What background is relevant?
- **Constraints**: Any limitations or requirements?
- **Scope**: How large is this change? Single file? Multi-file? Multi-session?
- **Verification**: How will Claude verify the work is complete and correct?

### Step 2: Clarity Check (Interview if Needed)

**If any of these are unclear, invoke the `interview` skill:**

| Signal | Action |
|--------|--------|
| Goal is vague ("improve X", "fix the thing") | Interview: clarify objective |
| Scope is ambiguous ("add auth", "refactor") | Interview: define boundaries |
| Multiple valid interpretations | Interview: resolve ambiguity |
| Verification can't be inferred | Interview: define success criteria |
| Design decisions implicit | Interview: surface choices |

**How to interview:**
- Use `AskUserQuestion` tool with 1-4 targeted questions
- Offer smart options (mark one as "Recommended" when appropriate)
- Users can always select "Other" for custom input
- Iterate until requirements are clear

**After interview, inject results as context:**
```markdown
## Interview Results
**Goal**: [Clarified objective]
**Scope**: [What's in/out]
**Design Decisions**: [Choices made]
**Verification**: [Success criteria]
```

Then continue to Step 3 with refined understanding.

### Step 3: Identify Verification Steps

**Critical**: Clear verification steps are essential.

Consider:
- How will Claude know the task is complete?
- What tests should pass?
- What commands verify success?
- What should Claude check before declaring done?

**If verification is unclear from the input, ASK the user**:
> "How should Claude verify this work is complete? For example: run tests, check specific behavior, validate output format, etc."

### Step 4: Output

Output the refined prompt in this format:

```
## Goal
[Clear statement of objective]

## Context
[Relevant background, constraints, environment]

## Requirements
- [Requirement 1]
- [Requirement 2]

## Verification
- [How to verify it's done]
```

### Guidelines

- Preserve the user's intent; don't add scope
- Be concise but complete
- Include constraints the user implied but didn't state explicitly
- **Always include verification steps** - ask if unclear
