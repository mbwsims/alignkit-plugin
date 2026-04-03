# alignkit plugin for Claude Code

Instruction intelligence for Claude Code. Lint your rules, check whether your codebase follows them, and get actionable recommendations to improve your CLAUDE.md.

**Zero dependencies. Works immediately after install.**

## Install

```bash
claude plugin add github.com/mbwsims/alignkit-plugin
```

## What you get

- **`/lint`** — Deep instruction quality analysis: vague rules, conflicts, effectiveness ratings, coverage gaps, consolidation suggestions, placement advice (should this rule be a hook? a scoped rule?)
- **`/check`** — Conformance report: does your codebase actually follow your rules? Checks each rule against the code with specific evidence (file paths, line numbers)
- **Auto-lint hook** — Surfaces quality issues automatically when you edit CLAUDE.md or `.claude/rules/`
- **instruction-advisor agent** — Full autonomous review combining lint + check in one report

All features work out of the box with no setup beyond installing the plugin.

## Optional: session-based adherence tracking

For tracking rule adherence *across sessions over time*, install the [alignkit](https://github.com/mbwsims/alignkit) npm package:

```bash
npm install -g alignkit
```

This upgrades `/check` from a point-in-time conformance check to persistent adherence tracking:

| | Without alignkit | With alignkit |
|---|---|---|
| `/lint` | Full analysis via Claude's reasoning | Enhanced with precise token counts and 10 static analyzers |
| `/check` | Conformance check (does the code match rules *right now*?) | Adherence tracking (did Claude follow rules *across sessions*?) |
| Trend data | No | Yes — "adherence trending down over 12 sessions" |
| History | No | Persistent — builds automatically across sessions |

Most users won't need the npm package. The plugin is fully functional without it.

## How it works

The plugin provides skills, a hook, and an agent that guide Claude through structured instruction analysis. When the alignkit npm package is installed, an MCP server gives Claude access to additional tools for precise static analysis and session history parsing. Without it, Claude applies the same methodology using its native reasoning.

## Components

| Component | Type | Purpose |
|-----------|------|---------|
| `/lint` | Skill | Instruction quality analysis with effectiveness ratings |
| `/check` | Skill | Rule conformance/adherence checking with evidence |
| Auto-lint | Hook | Surfaces issues on instruction file edits |
| instruction-advisor | Agent | Comprehensive lint + check review |

## License

MIT
