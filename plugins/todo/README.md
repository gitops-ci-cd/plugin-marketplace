# Todo

A productivity plugin that helps you organize, prioritize, and stay on top of your work. It uses the [todo-mcp-server](https://github.com/gitops-ci-cd/todo-mcp-server) under the hood for task storage and operations, but the plugin itself is about the *workflow layer* above raw CRUD.

## What it provides

| Entity | Path | What you get |
|--------|------|--------------|
| **Skill** | `skills/productivity/SKILL.md` | Claude auto-activates when you ask "what should I work on?" or "I'm overwhelmed" |
| **Command** | `commands/quick-add.md` | `/todo:quick-add Buy milk` — capture without breaking flow |
| **Command** | `commands/status.md` | `/todo:status` — workload snapshot: next up, done today, going stale |
| **Agent** | `agents/todo-coach.md` | A coach subagent that helps you decide what to focus on |
| **Hook** | `hooks/hooks.json` | Activity log — appends to `.todo-activity.log` on every task change |
| **MCP server** | `.mcp.json` | Wires in `@gitops-ci-cd/todo-mcp-server` for the actual tools |
| **LSP server** | `.lsp.json` | Code intelligence for `.todo` / `.tasks` files (placeholder) |
| **Monitor** | `monitors/monitors.json` | Background health-check for the remote MCP endpoint |
| **Bin** | `bin/todo-summary` | Shell script on `$PATH` — quick CLI summary |
| **Settings** | `settings.json` | Activates the `coach` agent by default when the plugin is enabled |

## The split

```
┌─────────────────────────────────┐
│  This plugin (todo)             │  ← Workflows, coaching, commands
│  • What should I work on next?  │
│  • Break this down for me       │
│  • Plan my day                  │
├─────────────────────────────────┤
│  .mcp.json                      │  ← Wiring
├─────────────────────────────────┤
│  todo-mcp-server                │  ← Raw tools: list, upsert, delete,
│  (npm / Docker / HTTP)          │     import, breakdown, analyze
└─────────────────────────────────┘
```

## MCP server transports

The `.mcp.json` uses npx by default. Alternatives:

| Transport | Value |
|-----------|-------|
| stdio (npx) | `npx @gitops-ci-cd/todo-mcp-server@latest` |
| HTTP | `https://ai.acme.com/mcp` |
| Docker | `ghcr.io/gitops-ci-cd/todo-mcp-server` |

## Prerequisites

- Node.js 18+
