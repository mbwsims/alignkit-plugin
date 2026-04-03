# alignkit plugin for Claude Code

Instruction intelligence for Claude Code. Discover your project's unwritten conventions, lint your rules, and check whether your codebase actually follows them.

**Zero dependencies. Works immediately after install.**

## Install

```bash
claude plugin add github.com/mbwsims/alignkit-plugin
```

## What you get

- **`/discover`** — Reverse-engineer conventions from your codebase. Reads your code, finds patterns (import styles, naming, error handling, API shapes, architecture boundaries), and generates paste-ready rules with evidence. *"All 47 files use @/ imports — zero relative paths found."*
- **`/lint`** — Deep instruction quality analysis: vague rules, conflicts, effectiveness ratings, coverage gaps, consolidation suggestions, placement advice.
- **`/check`** — Conformance report: does your codebase actually follow your rules? Checks each rule against the code with specific evidence.
- **Auto-lint hook** — Surfaces quality issues automatically when you edit CLAUDE.md or `.claude/rules/`.
- **instruction-advisor agent** — Full autonomous review combining all of the above.

All features work out of the box with no setup beyond installing the plugin.

## The loop

```
/discover  →  find unwritten conventions in your code
    ↓
/lint      →  check the quality of your rules
    ↓
/check     →  verify the codebase follows them
    ↓
/discover  →  find more as the project evolves
```

## Optional: session-based adherence tracking

For tracking rule adherence *across sessions over time*, install the [alignkit](https://github.com/mbwsims/alignkit) npm package:

```bash
npm install -g alignkit
```

This upgrades `/check` from a point-in-time conformance check to persistent adherence tracking:

| | Without alignkit | With alignkit |
|---|---|---|
| `/discover` | Full convention discovery | Same |
| `/lint` | Full analysis via Claude's reasoning | Enhanced with precise token counts and 10 static analyzers |
| `/check` | Conformance check (does the code match rules *right now*?) | Adherence tracking (did Claude follow rules *across sessions*?) |
| Trend data | No | Yes — "adherence trending down over 12 sessions" |

Most users won't need the npm package. The plugin is fully functional without it.

## Components

| Component | Type | Purpose |
|-----------|------|---------|
| `/discover` | Skill | Reverse-engineer conventions from codebase into rules |
| `/lint` | Skill | Instruction quality analysis with effectiveness ratings |
| `/check` | Skill | Rule conformance/adherence checking with evidence |
| Auto-lint | Hook | Surfaces issues on instruction file edits |
| instruction-advisor | Agent | Comprehensive discover + lint + check review |

## License

MIT
