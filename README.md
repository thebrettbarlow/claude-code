# Claude Code Starter Kit

A curated collection of skills, commands, and configuration for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — Anthropic's CLI tool for working with Claude directly in your terminal.

## Why Claude Code?

If you've been using browser-based AI tools (ChatGPT, Claude.ai, etc.) and getting mixed results, Claude Code offers a fundamentally different experience:

- **Artifact-based**: Claude reads and writes real files on your computer, not ephemeral chat bubbles
- **Customizable**: Skills and commands tailor Claude's behavior to how *you* work
- **Persistent context**: CLAUDE.md files give Claude project-specific knowledge every session
- **Tool access**: Claude can search the web, run commands, and interact with your filesystem directly

Think of it as going from a chatbot to a capable assistant that lives in your project.

## Prerequisites

You'll need two things before getting started:

1. **Node.js 18+** — [Download here](https://nodejs.org/) (choose the LTS version)
2. **A Claude plan** — [Pro ($20/mo)](https://claude.ai/settings/billing) or [Max ($100-200/mo)](https://claude.ai/settings/billing)

To check if Node.js is installed, open your terminal and type:
```bash
node --version
```

If you see a version number (like `v22.x.x`), you're good. If not, install Node.js first.

## Installation

### Step 1: Install Claude Code

Open your terminal and run:
```bash
npm install -g @anthropic-ai/claude-code
```

### Step 2: Clone this starter kit

```bash
git clone https://github.com/brettbarlow/claude-code.git
cd claude-code
```

### Step 3: Start Claude Code

From inside the cloned folder:
```bash
claude
```

Claude will walk you through authentication on first run.

## What's Included

### Skills (auto-loaded by Claude when relevant)

| Skill | What it does |
|-------|-------------|
| **research** | Deep exploration of any topic with structured documentation |
| **interview** | Clarifies your intent through smart questions before acting |
| **coaching** | Multi-perspective personal coaching (add your own advisors) |
| **frontend-design** | Creates distinctive, production-grade web interfaces |
| **presentation-design** | Designs memorable slides and presentations |
| **planning** | Generates implementation plans using parallel thinking |
| **implementation** | Executes plans incrementally with verification |
| **claude-code** | Meta-skill for creating your own skills, commands, and config |

### Commands (you type these)

| Command | What it does |
|---------|-------------|
| `/prompt-writer` | Transforms messy thoughts into clear, effective prompts |
| `/dictation` | Cleans up garbled speech-to-text input |
| `/rpi` | Research-Plan-Implement workflow for complex tasks |

### Configuration

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Project instructions Claude follows every session |
| `.claude/settings.json` | Permission boundaries (what Claude can/can't do) |

## Getting Started

Once Claude is running, try these:

### Ask Claude to research something
```
Research the best note-taking apps for students
```

### Clean up a voice memo
```
/dictation i wanna no about the best way to learn python programming for beginrs
```

### Refine a messy idea
```
/prompt-writer I need help writing an email to my landlord about the broken dishwasher
  it's been 3 weeks and they haven't fixed it I want to be firm but professional
```

### Get coached on a decision
```
Help me think through whether I should take this job offer
```

### Build something
```
/rpi Build me a personal portfolio website
```

## Customizing Your Setup

### Adding to CLAUDE.md

The `CLAUDE.md` file in the root tells Claude how to behave. Edit it to add your own preferences:

```markdown
## My Preferences
- Always explain things simply
- I prefer Python over JavaScript
- When suggesting tools, prefer free/open-source options
```

### Creating Your Own Commands

Add a `.md` file to `.claude/commands/`:

```markdown
---
description: Summarize the current project
---

Look at the files in this project and give me a brief summary of:
1. What this project does
2. Key technologies used
3. Current state (complete, in-progress, etc.)
```

Save as `.claude/commands/summarize.md`, then use it with `/summarize`.

### Adding Coaching Advisors

The coaching skill starts empty — you add advisors that matter to you. Just ask Claude:

```
I want to add a coaching advisor. Let's set one up.
```

Claude will interview you to understand who you want to add and create the advisor file.

## Tips for Beginners

1. **Start simple.** Just type what you want in plain English. Claude figures out the rest.
2. **Use `/prompt-writer` when stuck.** If you can't articulate what you want, this command helps you clarify.
3. **Let skills auto-load.** You don't need to invoke skills manually — Claude detects when they're relevant.
4. **Build on what works.** When Claude does something you like, ask it to create a skill or command so it does it that way every time.
5. **Check the `/help` menu.** Type `/help` inside Claude Code to see all available commands.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `command not found: claude` | Run `npm install -g @anthropic-ai/claude-code` again |
| Authentication issues | Run `claude` and follow the login prompts |
| Claude doesn't use skills | Make sure you're running Claude from inside this folder |
| Permission denied errors | Check `.claude/settings.json` — you may need to allow the command |

## License

MIT
