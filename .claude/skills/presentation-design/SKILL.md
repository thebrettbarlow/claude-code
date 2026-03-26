---
name: presentation-design
description: Design memorable, professional presentations. Use when creating slides, reviewing deck structure, designing visual hierarchy for projection, or avoiding generic "AI slop" patterns. Platform-agnostic principles.
---

# Presentation Design

Design presentations that audiences remember and speakers deliver with confidence.

## Quick Start

```
"Help me structure this presentation"
"Review my slide deck for design issues"
"I need to present [topic] in 15 minutes"
"Make these slides more memorable"
```

## When to Use

| Trigger | Action |
|---------|--------|
| "create presentation" | Full narrative + visual design |
| "review slides" | Design critique using checklist |
| "structure my talk" | Narrative arc mapping |
| "make it memorable" | Anti-pattern cleanup |

## Design Philosophy

**Core principle: Presentations are not documents projected on a wall.**

| Medium | Purpose | Constraint |
|--------|---------|------------|
| Document | Self-contained reading | Reader controls pace |
| Presentation | Speaker-augmented visual aid | Speaker controls pace, projected viewing, time-bound |

**The presenter IS the content.** Slides exist to amplify, not replace.

## Narrative Architecture

### Story Arc (Required)

Every presentation needs tension and resolution:

```
Hook → Problem → Journey → Insight → Close
(Why care)  (Stakes)   (Evidence)  (So what)  (Now what)
```

**Hook**: First 30 seconds determine attention. Start with a surprising statistic, a provocative question, a brief story that creates curiosity, or a counterintuitive claim.

**Problem**: Establish stakes before solutions. Audience must feel the tension.

**Journey**: Evidence, examples, progression. Each slide advances the story.

**Insight**: The "aha" moment. What should they remember in a week?

**Close**: Clear call to action or memorable takeaway. Never end with Q&A slide.

### One Idea Per Slide

**The fundamental rule.** Every slide answers one question:

| Slide Type | The One Question |
|------------|------------------|
| Title | "What will you learn?" |
| Problem | "Why does this matter?" |
| Data | "What does the evidence show?" |
| Process | "How does this work?" |
| Conclusion | "What should you do now?" |

If a slide answers multiple questions, split it.

## Visual Hierarchy

### Focal Point Hierarchy

Every slide needs one obvious focal point. Achieve with:

| Technique | When to Use |
|-----------|-------------|
| Scale (biggest element) | Most important concept |
| Contrast (brightest/boldest) | Key data point |
| Isolation (whitespace around) | Single takeaway |
| Position (center/top-left) | Primary message |

**Test**: Squint at the slide. What's the first thing you see? That better be the main idea.

## Typography for Projection

### Minimum Sizes (Projection-Safe)

| Element | Minimum | Recommended |
|---------|---------|-------------|
| Headline | 44pt | 60-72pt |
| Body text | 24pt | 28-32pt |
| Labels/captions | 18pt | 20-24pt |
| Footnotes | 14pt (avoid) | Eliminate |

**Rule**: If you can't read it from 15 feet away, it's too small.

### Text Reduction

Transform sentences to fragments:

```
Bad:  "We implemented a new customer onboarding process that reduced time-to-value by 40%"
Good: "New onboarding -> 40% faster time-to-value"
```

**Maximum**: 6 words per bullet, 6 bullets per slide (and that's pushing it).

## Color and Contrast

### Projection Environment Reality

Projectors wash out colors. Design for the worst case:
- Bright room with ambient light
- Cheap projector with low contrast
- Sun glare on screen

### Color Palette

| Purpose | Approach |
|---------|----------|
| Primary | One dominant color for emphasis |
| Accent | One contrasting color sparingly |
| Neutrals | Black, white, one gray |
| Data viz | Max 5 distinct colors, accessible palette |

**Test**: Convert to grayscale. If information is lost, fix the contrast.

## Animation Philosophy

### Purpose-Driven Motion

**The only valid reasons for animation:**

| Reason | Example |
|--------|---------|
| Reveal sequence | Build complex diagram step-by-step |
| Direct attention | Highlight current speaker point |
| Show relationship | Transition that connects concepts |
| Pace delivery | Control information reveal timing |

### Timing

- Appear/fade: 200-400ms (fast enough to feel instant)
- Motion: 300-500ms (perceptible but not distracting)
- Never: Bounces, spins, swooshes, 3D flips

## Anti-Patterns

### "AI Slop" Indicators

| Pattern | Why It Fails | Fix |
|---------|--------------|-----|
| Generic stock photos | Looks template-driven | Custom imagery or none |
| Bullet-point paragraphs | Reading != presenting | One idea, few words |
| Gradient rainbow colors | Aesthetically chaotic | 2-3 color palette max |
| Every slide animated | Exhausting, distracting | Reserve for purpose |
| Title slide with agenda | Nobody cares | Start with hook |
| "Questions?" slide | Lazy ending | End with impact |

## Review Checklist

### Narrative
- [ ] Hook in first 30 seconds
- [ ] Clear problem/stakes established
- [ ] Single insight per slide
- [ ] Memorable close (not "Questions?")

### Visual Hierarchy
- [ ] One focal point per slide
- [ ] 30%+ white space
- [ ] Passes squint test

### Typography
- [ ] Headlines 44pt+, body 24pt+
- [ ] Max 6 words per bullet
- [ ] 2 fonts maximum

### Color/Contrast
- [ ] 7:1+ contrast ratio
- [ ] Works in grayscale
- [ ] 3-color palette max

### Animation
- [ ] Purpose-driven only
- [ ] Speaker-controlled (click to advance)
- [ ] 200-500ms timing

## See Also

- `interview` - Gather requirements before designing
- `frontend-design` - For web-based presentations
