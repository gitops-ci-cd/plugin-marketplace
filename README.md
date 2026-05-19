# Plugin Marketplace

A demo marketplace that showcases **every plugin entity type** supported by Claude Code, with Copilot compatibility.

## Plugins

| Plugin | Description |
| -------- | ------------- |
| [todo](plugins/todo/) | Productivity & task management — skills, commands, agents, hooks, MCP, LSP, monitors, bin, settings |

## Usage

### Claude Code (primary)

```sh
# Add the marketplace
/plugin marketplace add gitops-ci-cd/plugin-marketplace

# Install the plugin
/plugin install todo@gitops-ci-cd-plugins
```

### VS Code / GitHub Copilot

The `.github/plugin/marketplace.json` provides Copilot-compatible metadata. Copilot reads skills and MCP server configs from the same directories.

## Cross-client Compatibility

There is **no single universal standard** for AI coding-assistant plugins. This repo supports both major formats:

| Path | Client | Purpose |
| ------ | -------- | --------- |
| `.claude-plugin/marketplace.json` | Claude Code | Canonical marketplace manifest |
| `.github/plugin/marketplace.json` | VS Code Copilot | Copilot marketplace manifest |
| `.mcp.json` (in plugin) | Both | MCP server config (shared format) |
| `skills/*/SKILL.md` | Both | Skill definitions with YAML frontmatter (shared format) |
| `commands/*.md` | Claude Code | Flat-file slash commands |
| `agents/*.md` | Claude Code | Custom subagent definitions |
| `hooks/hooks.json` | Claude Code | Lifecycle event handlers |
| `.lsp.json` | Claude Code | LSP server configurations |
| `monitors/monitors.json` | Claude Code | Background process watchers |
| `bin/*` | Claude Code | Executables added to `$PATH` |
| `settings.json` | Claude Code | Default settings when plugin is enabled |

## Structure

```sh
.claude-plugin/
  marketplace.json              ← Claude Code marketplace manifest
.github/plugin/
  marketplace.json              ← Copilot marketplace manifest
plugins/
  todo/                         ← the plugin (higher-level productivity layer)
    .claude-plugin/
      plugin.json               ← plugin identity & metadata
    skills/
      productivity/
        SKILL.md                ← model-invoked: "what should I work on?"
    commands/
      quick-add.md              ← /todo:quick-add <title>
      status.md                 ← /todo:status
    agents/
      coach.md                  ← coach subagent
    hooks/
      hooks.json                ← activity log on task changes
    monitors/
      monitors.json             ← background health check
    bin/
      todo-summary              ← shell script on $PATH
      todo-lsp-server.js        ← Node LSP (placeholder)
    .mcp.json                   ← wires in todo-mcp-server (the implementation)
    .lsp.json                   ← LSP server config
    settings.json               ← activates coach agent by default
    README.md
```

## Entity Type Reference

| # | Entity | File | Invocation | Description |
| --- | -------- | ------ | ------------ | ------------- |
| 1 | **Skill** | `skills/<name>/SKILL.md` | Auto (model chooses) or `/plugin:skill` | Markdown with YAML frontmatter. Claude uses it when the user's task matches the `description`. |
| 2 | **Command** | `commands/<name>.md` | `/plugin:command [args]` | Flat markdown file. User-invoked slash command. `$ARGUMENTS` captures input. |
| 3 | **Agent** | `agents/<name>.md` | `/agents` menu or `settings.json` | Markdown with YAML frontmatter. Defines system prompt, model, and tool allow/deny lists. |
| 4 | **Hook** | `hooks/hooks.json` | Automatic on lifecycle events | JSON. Runs shell commands on events like `PreToolUse`, `PostToolUse`, `Notification`, etc. |
| 5 | **MCP server** | `.mcp.json` | Automatic when plugin is enabled | JSON. Same format as VS Code / Copilot `.mcp.json`. Registers MCP servers for tool access. |
| 6 | **LSP server** | `.lsp.json` | Automatic for matching file extensions | JSON. Provides code intelligence (diagnostics, completions) for specified file types. |
| 7 | **Monitor** | `monitors/monitors.json` | Automatic background process | JSON array. Long-running commands whose stdout lines are delivered to Claude as notifications. |
| 8 | **Bin** | `bin/<name>` | `!todo-summary` in chat or from hooks | Executable files added to `$PATH` while the plugin is active. |
| 9 | **Settings** | `settings.json` | Automatic when plugin is enabled | JSON. Currently supports `agent` (activate a default subagent) and `subagentStatusLine`. |
| 10 | **Plugin manifest** | `.claude-plugin/plugin.json` | — | JSON. Plugin identity: name, description, version, author, license. |
