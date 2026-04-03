# alignkit plugin for Claude Code

Instruction intelligence for Claude Code. Lint your rules, track whether Claude actually follows them, and get actionable recommendations to improve your CLAUDE.md.

## What it does

- **`/check`** — See which rules Claude follows across sessions. Evaluates unresolved rules with deep analysis (no API key needed).
- **`/lint`** — Static analysis + deep effectiveness ratings, coverage gaps, and consolidation suggestions.
- **Auto-lint hook** — Surfaces quality issues when you edit CLAUDE.md or `.claude/rules/`.
- **instruction-advisor agent** — Full autonomous review combining lint + adherence in one report.

## Install

1. Install the alignkit npm package:

```bash
npm install -g alignkit
```

2. Install this plugin in Claude Code:

```bash
claude plugin add github.com/mbwsims/alignkit-plugin
```

## Requirements

- Node.js >= 18
- [alignkit](https://github.com/mbwsims/alignkit) npm package (provides the MCP server)

## How it works

The plugin ships an MCP server configuration that starts `alignkit-mcp`. This gives Claude access to three tools:

- `alignkit_lint` — Parses instruction files, runs 10 static analyzers, returns rules with diagnostics and project context
- `alignkit_check` — Reads Claude Code session history, verifies rules against actions, returns adherence data
- `alignkit_status` — Quick adherence pulse with trend

Skills and hooks orchestrate when to call these tools and how to present the results. The deep analysis that previously required an Anthropic API key is now performed by Claude directly.

## Components

| Component | Type | Purpose |
|-----------|------|---------|
| `/check` | Skill | Adherence report with deep unresolved rule evaluation |
| `/lint` | Skill | Static analysis + effectiveness, coverage gaps, consolidation |
| Auto-lint | Hook | Surfaces issues when instruction files are edited |
| instruction-advisor | Agent | Comprehensive lint + adherence review |

## License

MIT
