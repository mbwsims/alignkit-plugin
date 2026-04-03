---
name: check-adherence
description: >-
  This skill should be used when the user asks to "check adherence", "check rule adherence",
  "are my rules being followed", "is Claude following my instructions", "how effective is my
  CLAUDE.md", "rule compliance", "instruction effectiveness", "how are my rules performing",
  "which rules are being violated", "audit my rules", "check my CLAUDE.md", mentions "/check",
  or wants to know whether their coding agent instruction rules are actually being followed
  across Claude Code sessions.
allowed-tools:
  - mcp__alignkit__alignkit_check
  - mcp__alignkit__alignkit_status
  - Read
  - Glob
  - Grep
argument-hint: "[file]"
---

# Check Adherence

Analyze whether instruction rules are actually being followed by checking Claude Code session
history. Combines alignkit's automated verification engine with deep evaluation of rules that
can't be auto-verified — providing the full analysis that previously required an API key, at
no cost.

## Workflow

### 1. Gather Data

Call the `alignkit_check` tool. Pass the `file` argument if the user specified one; otherwise
omit it for auto-discovery. Optionally pass `since_days` to narrow the analysis window.

If no sessions or history exist, explain that adherence tracking builds over time as the user
works with Claude Code. Suggest checking back after several sessions.

### 2. Present the Adherence Overview

Format results as a structured report. Start with a summary line, then a status breakdown,
then detailed analysis of problem areas.

**Report format:**

```
## Adherence Report — {file}

{sessionCount} sessions analyzed · {total rules} rules tracked · {overall}% adherence

| Status            | Count |
|-------------------|-------|
| Fully followed    | {n}   |
| Partial adherence | {n}   |
| Never triggered   | {n}   |
| Unresolved        | {n}   |
```

Then list rules needing attention, ordered by severity (lowest adherence first):

```
### Rules Needing Attention

1. **"{rule text}"** — {adherence}% ({followed}/{resolved} sessions)
   {One-line analysis: why it's low and what to do}

2. **"{rule text}"** — never triggered ({sessionCount} sessions)
   {Assessment: still relevant or should be removed?}
```

### 3. Evaluate Unresolved Rules

This is the core value — replacing the paid `--deep` flag. For each unresolved rule, the
`alignkit_check` response includes session action summaries: bash commands run, files written,
files edited. Examine this evidence to determine whether the rule was followed.

For each unresolved rule (evaluate the 8 with the most associated session action data):

1. Read the rule text — understand what behavior it requires
2. Examine session actions — look for evidence of compliance or violation
3. Render a verdict: **Followed**, **Violated**, or **Inconclusive**
4. Cite specific evidence supporting the verdict

Present evaluations in a clean table:

```
### Deep Evaluation — Unresolved Rules

| Rule | Verdict | Evidence |
|------|---------|----------|
| "Always run tests..." | Followed | test commands in 4/5 sessions |
| "Use strict types..." | Violated | no type-check commands found |
| "Follow naming..." | Inconclusive | style rules not observable from actions |
```

If more than 8 unresolved rules exist, note the remainder: "{N} additional unresolved rules —
run `/check` again for next batch."

Consult `references/evaluation-guide.md` for detailed evaluation patterns and common evidence
indicators.

### 4. Recommendations

Close with 2-4 specific, actionable recommendations prioritized by impact. Reference actual
rule text in every recommendation. Avoid generic advice.

Consult `references/evaluation-guide.md` for recommendation patterns by rule outcome
(violated, never-triggered, inconclusive).

### 5. Trend (When Available)

If the data warrants it, call `alignkit_status` and include a trend line:

```
**Trend:** {up|down|stable} over {sessionCount} sessions
```

Only include when 5+ sessions exist and the trend is meaningful.

## Guidelines

- Present data honestly — if adherence is low, say so directly
- Distinguish "rule is being violated" from "rule can't be measured" — different situations
  requiring different responses
- Never fabricate evidence — if actions don't clearly indicate compliance, mark Inconclusive
- Keep the report scannable — follow the structured format consistently
- Structure the report so the summary alone conveys whether instructions are working

## Additional Resources

- **`references/evaluation-guide.md`** — Detailed patterns for evaluating unresolved rules,
  common evidence indicators, and edge case handling
