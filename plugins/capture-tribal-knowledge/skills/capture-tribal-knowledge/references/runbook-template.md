# Runbook Template (Confluence-ready markdown)

The runbook is for a human taking over the work. They find it by searching the wiki for an error string or job name, usually mid-incident. Design principle is the same as the skill: front-load lookup, push narrative to the back. The difference is the container: one wiki page, read by people, pasted into Confluence.

## Markdown that survives the paste

Confluence's editor converts most standard markdown on paste, but not all of it. Anything outside this subset shows up as literal characters or gets mangled, and the person publishing will have to fix it by hand — which they won't.

**Use:**
- `#`, `##`, `###` headings (ATX style). Confluence builds its table of contents from these.
- Bullet and numbered lists, nested at most one level.
- Fenced code blocks with a language (```` ```bash ````). They become code macros.
- Inline code, **bold**, *italic*, `[links](https://…)`.
- Simple pipe tables: one line per row, no line breaks inside a cell, no `|` inside a cell (write "or" instead, or put the pipe inside inline code only if unavoidable).
- Blockquotes starting with a bold label for callouts: `> **Warning:** …`, `> **Note:** …`. The publisher can convert these to Warning/Info panels in one click; even unconverted they read correctly.

**Avoid:**
- YAML frontmatter, HTML tags, `<details>`, footnotes, task-list checkboxes, definition lists.
- Links to headings on the same page (`#procedure-a`). Confluence generates different anchors; the links break. Refer to sections by name instead: "see **Procedure A** below".
- Multi-paragraph content inside table cells. Move it to a procedure and reference it from the table.
- A top-level `#` title. The Confluence page title is a separate field; the body should start at the page-properties table. Give the title to the user separately.

## Page structure

```markdown
| | |
|---|---|
| **Owner** | <team or role that owns this now — not a person; names rot> |
| **Captured from** | <expert name/role>, <date captured> |
| **Last verified** | <date> |
| **Review by** | <date, ~6 months out, or "after next incident"> |
| **Applies to** | <repo / service / pipeline / system names, verbatim> |

<One paragraph: what this process is, when you'd need this page, and what's out of scope. Name the error strings or job names someone would have searched to land here — this paragraph is what Confluence search snippets show.>

**How much to trust each part:** (verified) = seen in a real artifact or demonstrated · (expert-stated) = told to us, not demonstrated · (inferred) = our reconstruction, not contradicted. Trust in that order.

## Before you start

- <Preconditions the expert checks without thinking: access, VPN, env vars, branch state, "is anyone else deploying".>
- <What to grab before touching anything: job URL, log link, commit SHA. This is also what you'll need if you escalate.>

## Symptom → cause → fix

| You see | Most likely | Do | Confidence |
|---|---|---|---|
| `<verbatim error fragment>` | <cause> | Procedure A | (verified) |
| <symptom> | <cause> | Procedure B | (expert-stated) |
| <symptom> | <cause — not ours / not fixable here> | **Stop.** See Escalation | (verified) |

Order rows most-common first. Include at least one "stop" row.

## Procedures

### Procedure A — <name> (verified)

**Use when:** <condition from the symptom table>

1. <Literal command, real path and flags. Explain any non-obvious flag in the step itself.>
   ```bash
   ./scripts/ci/clear-pip-cache.sh --runner <runner id from ci-meta.json>
   ```
2. <…>

**Verify:** <Specific observable result.>

**If it didn't work:** <Next branch, or "go to Escalation".>

> **Warning:** <Any step in this procedure that can do damage, with the consequence. Put it right above the step it applies to, not at the end.>

### Procedure B — <name> (expert-stated)

…

## Decision rules

- **If** <condition> **then** <action>. *Why:* <the reason, ideally from the episode that taught it>.
- **If unsure whether** <X>: <cheap check that tells you>.

## Never do this

- **Never** <action>, even if <tempting reason>. <Consequence, ideally from a real incident: "This happened once; every team's next build took 20 extra minutes.">

## Escalation

- **Stop and escalate when:** <condition that looks fixable but isn't>.
- **Needs approval:** <action> — <who approves, which ticket type>.
- **Escalate to:** <channel or rotation, e.g. `#platform-oncall`>. **Include:** <the items from "Before you start">.

## Open questions

<What the interview didn't resolve. A reader who knows the page is incomplete will ask; one who thinks it's complete will guess.>

- <open question>
- <failure mode mentioned once, cause unknown>

## Appendix: scripts and tools

### `<path/to/script.sh>`
- **Does:** <one line — what it actually does, not what the README says>
- **Run:** `<exact invocation>`
- **Always:** `--flag` because <why>. **Never:** `--other` because <why>.
- **Fails:** <loudly / silently>. **If killed halfway:** <state left behind>.
- **Edit the script (not flags) when:** <situation> — change <line/section>.

## Appendix: past incidents

Short versions of the episodes from the interview. Humans generalize from stories; these are why the rules above exist.

- **<date or "early 2026"> — <one-line title>.** <What was seen, what was tried, what fixed it, what it taught.>
```

## Writing rules

Same as the skill template (verbatim error strings, real names, "why" on every rule, numeric thresholds, verification as a step), plus:

- **Provenance tags use parentheses**, `(verified)` not `[verified]`. Bracketed text sits next to markdown link syntax and some wiki converters treat it oddly; parentheses read as plain prose everywhere.
- **Owner is a team or rotation, not a person.** The whole reason this page exists is that a person is leaving or overloaded.
- **Keep the past-incidents appendix.** In a skill, episodes are optional context; in a runbook they are how humans remember the rules. One to three lines each.
- **No agent-only content.** Skip "ask the human before…" guard phrasing — the reader *is* the human. Keep the warnings, written as warnings.

## Deliver with

Tell the user, alongside the file:
- **Suggested page title:** `<Process> — Runbook` (e.g. "parse_and_detect integration build failures — Runbook"). Searchable nouns first.
- **Suggested parent page:** the team's runbooks/on-call space if mentioned; otherwise ask.
- **Suggested labels:** `runbook`, `<system name>`, `<team>`, `handoff`.
- **After pasting:** convert `> **Warning:**` blockquotes to Warning panels, and add a Table of Contents macro at the top if the page has more than ~4 procedures.
