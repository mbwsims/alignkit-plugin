# alignkit plugin for Claude Code

Discover your project's unwritten conventions, lint your instruction rules, and check whether your codebase actually follows them.

## Install

```bash
claude plugin add github.com/mbwsims/alignkit-plugin
```

## What you get

- **`/discover`** — Reverse-engineer conventions from your codebase. Reads your code, finds patterns, and generates paste-ready rules with evidence.
- **`/lint`** — Deep instruction quality analysis: vague rules, conflicts, effectiveness ratings, coverage gaps, consolidation suggestions, placement advice.
- **`/check`** — Conformance report: does your codebase actually follow your rules? Checks each rule against the code with specific evidence.
- **Auto-lint hook** — Surfaces quality issues automatically when you edit CLAUDE.md or `.claude/rules/`.
- **instruction-advisor agent** — Full autonomous review combining all of the above.

### Example: /discover

```
Discovered Conventions — 10 conventions found across 20+ files

1. Auth guard at top of every API route                                    high
   Suggested rule: "Every API route must start with const user =
   await getApiUser(); if (!user) return unauthorized()."
   Evidence: All 7 routes follow this exact pattern.

2. Ownership check returns 404, not 403                                    high
   Suggested rule: "Return 404 (not 403) for unauthorized resource
   access to avoid revealing resource existence."
   Evidence: 6 ownership checks across 5 routes — all return 404.

3. Components never import from lib/db/ or lib/agents/                     high
   Suggested rule: "Client components must not import from @/lib/db/
   or @/lib/agents/. Data flows through API routes or server props."
   Evidence: 0 Prisma or agent imports in any of 30 component files.

Add rules: [1] [2] [3] [all] — or specify which to add
```

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
| `/check` | Conformance check (does code match rules *now*?) | Adherence tracking (did Claude follow rules *across sessions*?) |
| Trend data | No | Yes — tracks adherence over time |

The plugin works out of the box. Install alignkit when you're ready to track adherence across sessions.

## Components

| Component | Type | Purpose |
|-----------|------|---------|
| `/discover` | Skill | Reverse-engineer conventions from codebase into rules |
| `/lint` | Skill | Instruction quality analysis with effectiveness ratings |
| `/check` | Skill | Rule conformance/adherence checking with evidence |
| Auto-lint | Hook | Surfaces issues on instruction file edits |
| instruction-advisor | Agent | Comprehensive discover + lint + check review |

## Links

- [alignkit npm package](https://github.com/mbwsims/alignkit) — CLI + MCP server for CI integration and session-based adherence tracking

## License

MIT
