---
name: claude-code
description: "Creates and configures Claude Code extensions: skills, commands, hooks, CLAUDE.md, settings, and MCP servers. Use when creating a skill, writing SKILL.md, designing /commands, configuring hooks, optimizing CLAUDE.md, setting permissions in settings.json, or adding MCP servers."
---

# Claude Code Extensions

Build skills, commands, hooks, and configuration for Claude Code.

## Quick Start

**Skills (Claude auto-detects)**
```
"Create a skill for managing database migrations"
"Write a SKILL.md for the payment-processing skill"
```

**Commands (User types /name)**
```
"Create a /deploy command"
"Write a slash command for releases"
```

**Hooks (Lifecycle events)**
```
"Create a hook that auto-formats code after edits"
"Add a PreToolUse hook to block dangerous commands"
```

**CLAUDE.md (Project instructions)**
```
"Create a CLAUDE.md for this project"
"Optimize my CLAUDE.md - it's too long"
```

**Settings (Permissions)**
```
"Add npm test to the allow list"
"Block destructive git commands"
```

---

## Extension Types

| Type | Invoked By | Purpose |
|------|------------|---------|
| **Skill** | Claude auto-detects | Load context for a domain |
| **Command** | User types /name | Expand into executable prompt |
| **Hook** | Lifecycle events | Deterministic actions |
| **MCP** | Tool calls | External integrations |
| **CLAUDE.md** | Session start | Project instructions |
| **Settings** | Always loaded | Permission boundaries |

## Decision Matrix

| You want to... | Use |
|----------------|-----|
| Auto-load context when topic detected | Skill |
| Create user-triggered workflow | Command |
| Enforce behavior EVERY time | Hook |
| Integrate external tools/services | MCP |
| Set project-wide instructions | CLAUDE.md |
| Allow/deny specific tools | Settings |

---

## Skills

Auto-loaded when Claude detects relevant context. Best for domain expertise.

```yaml
---
name: my-skill
description: Specific capabilities. Use when X, Y, or Z.
---
```

Key elements:
- **Frontmatter**: `name` + `description` (triggers auto-discovery)
- **Directory**: `.claude/skills/<name>/SKILL.md`
- **Progressive disclosure**: Link to reference files, don't embed everything

### Writing Good Descriptions

The description is critical — it determines when Claude loads the skill.

**Good**: `"Apex development patterns for Salesforce. Use when writing Apex classes, triggers, tests, or SOQL queries."`
**Bad**: `"Salesforce stuff"`

Include:
- What it does (2-3 words)
- When to use it (specific triggers)
- File types or keywords that should activate it

### Skill Structure

```
.claude/skills/my-skill/
├── SKILL.md           # Main skill file (auto-loaded)
├── reference/         # Detailed docs (loaded on demand)
│   ├── patterns.md
│   └── examples.md
└── scripts/           # Executable logic (optional)
    └── helper.ts
```

**Progressive disclosure**: Keep SKILL.md concise. Put detailed reference material in `reference/` and link to it. Claude loads reference files only when needed, saving tokens.

---

## Commands

User-triggered via `/name`. Expand into executable prompts.

```yaml
---
description: Brief description shown in /help
---
```

Key elements:
- **Location**: `.claude/commands/<name>.md`
- **Dynamic content**: `$ARGUMENTS` captures user input after the command name
- **Namespacing**: `commands/git/status.md` becomes `/git:status`

### Example Command

```markdown
---
description: Summarize changes since last commit
---

Run `git diff` and provide a concise summary of:
1. Files changed
2. Nature of changes (feature, fix, refactor)
3. Suggested commit message

$ARGUMENTS
```

---

## Hooks

Deterministic actions at lifecycle events. Cannot be reasoned around.

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{ "type": "command", "command": "npx prettier --write \"$CLAUDE_FILE_PATH\"" }]
    }]
  }
}
```

Key elements:
- **Events**: PreToolUse, PostToolUse, SessionStart, Stop
- **Exit codes**: 0 = continue, 2 = block the action
- **Environment**: `CLAUDE_TOOL_NAME`, `CLAUDE_TOOL_INPUT`, `CLAUDE_FILE_PATH`

### Common Hook Patterns

| Pattern | Event | Use Case |
|---------|-------|----------|
| Auto-format on save | PostToolUse (Write/Edit) | Keep code clean |
| Block dangerous commands | PreToolUse (Bash) | Prevent `rm -rf`, force push |
| Session greeting | SessionStart | Load project context |

---

## CLAUDE.md

Project instructions loaded every session. Token-critical.

Key elements:
- **Locations**: Global (`~/.claude/`), Project (root or `.claude/`), Nested (subdirs)
- **Imports**: `@path/to/file.md` to include external content
- **Size**: Keep under 300 lines (ideally < 60)

### CLAUDE.md Template

```markdown
# Project Name

## About
Brief project description.

## Stack
Key technologies.

## Development
Essential commands (build, test, lint).

## Conventions
Code style, patterns, naming.
```

**Anti-pattern**: Dumping everything into CLAUDE.md. Start minimal, add only what Claude gets wrong without instruction.

---

## Settings

Permission configuration in `.claude/settings.json`.

```json
{
  "permissions": {
    "allow": ["Bash(git status:*)", "Bash(npm test:*)"],
    "deny": ["Bash(rm -rf:*)", "Bash(git push --force:*)"]
  }
}
```

Key elements:
- **Tiers**: Allow (safe, auto-approved), Default (Claude decides), Deny (blocked)
- **Patterns**: `Tool(prefix:*)` matches command with any suffix
- **Precedence**: Deny > Allow > Default

---

## MCP Servers

Extend Claude with external tools via Model Context Protocol.

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-memory"]
    }
  }
}
```

Key elements:
- **Built-in vs MCP**: Use built-in tools first; MCP for external services
- **Configuration**: Define in `.claude/mcp.json`
- **Tool naming**: `mcp__<server>__<tool>` pattern

---

## Context Engineering Principles

**Four Buckets**: Write (store outside context), Select (pull on-demand), Compress (reduce tokens), Isolate (sub-agents)

**Progressive Disclosure**: Metadata always loaded (~100 tokens), body on-trigger (~500), references on-demand (~1-3k each)

**Token Budget**: Keep base context under 10k; reserve headroom for dynamic content

---

## Anti-Patterns Summary

| Category | Anti-Pattern | Fix |
|----------|--------------|-----|
| Skills | Vague description | Add specific verbs + triggers |
| Skills | Bloated SKILL.md | Split into referenced files |
| Commands | Missing $ARGUMENTS | Accept user input |
| Hooks | Using for preferences | Use instructions instead |
| MCP | Adding when built-in exists | Use Glob, Read, WebSearch first |
| CLAUDE.md | Comprehensive dump | Start minimal, add on need |
| Settings | Broad patterns | Use specific subcommands |

## See Also

- `interview` - Gather requirements before building extensions
- `research` - Research best practices for extension design
