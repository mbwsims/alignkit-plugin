# alignkit plugin for Claude Code

Instruction intelligence for Claude Code. Lint your rules, track whether Claude actually follows them, and get actionable recommendations to improve your CLAUDE.md.

## What it does

- **`/lint`** — Deep analysis of instruction quality: effectiveness ratings, coverage gaps, consolidation suggestions, placement advice. Works out of the box.
- **`/check`** — See which rules Claude follows across sessions. Evaluates unresolved rules with deep analysis (no API key needed). Requires alignkit.
- **Auto-lint hook** — Surfaces quality issues when you edit CLAUDE.md or `.claude/rules/`.
- **instruction-advisor agent** — Full autonomous review combining lint + adherence in one report.

## Install

```bash
claude plugin add github.com/mbwsims/alignkit-plugin
```

That's it. `/lint` and the auto-lint hook work immediately.

### Unlock adherence tracking

To use `/check` (the hero feature), install the alignkit npm package:

```bash
npm install -g alignkit
```

This provides an MCP server that reads Claude Code session history and verifies whether your rules are actually being followed. Without it, `/lint` still provides full instruction quality analysis — `/check` is the only feature that requires the package.

## How it works

**Without alignkit installed:** `/lint` reads your instruction files directly and applies the analysis methodology (vague detection, conflict detection, effectiveness ratings, coverage gaps, consolidation) using Claude's native reasoning. Token counts are estimated.

**With alignkit installed:** The plugin's MCP server (`alignkit-mcp`) gives Claude access to three tools:

- `alignkit_lint` — Parses instruction files, runs 10 static analyzers, returns precise diagnostics and project context
- `alignkit_check` — Reads Claude Code session history, verifies rules against actions, returns adherence data with unresolved rules for Claude to evaluate
- `alignkit_status` — Quick adherence pulse with trend

The deep analysis that previously required an Anthropic API key is now performed by Claude directly — at no cost.

## Components

| Component | Type | Requires alignkit | Purpose |
|-----------|------|:-:|---------|
| `/lint` | Skill | No | Instruction quality analysis with deep effectiveness ratings |
| `/check` | Skill | Yes | Adherence tracking across sessions |
| Auto-lint | Hook | No | Surfaces issues on instruction file edits |
| instruction-advisor | Agent | Partial | Full lint + adherence review (lint works without, check needs alignkit) |

## License

MIT
