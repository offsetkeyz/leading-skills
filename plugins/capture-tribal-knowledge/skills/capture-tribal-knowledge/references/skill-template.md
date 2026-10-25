# Output Skill Template

The generated skill is for the person or agent *taking over*. Design principle: they arrive with a problem, not with time to read. Front-load lookup; push narrative to the back.

## Folder layout

```
<topic-slug>/
├── SKILL.md                      # ≤300 lines; symptom table, procedures, boundaries
└── references/
    ├── scripts.md                # one card per script/tool (if >2 scripts)
    ├── episodes.md               # the incidents from the interview, lightly cleaned
    └── samples/                  # pasted logs, error text, example configs
```

Skip `references/` files that would be nearly empty. Three thin files are worse than one solid SKILL.md.

## SKILL.md structure

```markdown
---
name: <topic-slug>
description: <What this covers + when to use it. Name the symptoms a user would type: the error strings, job names, script names. Be pushy: "Use this whenever a parse_and_detect integration build fails, CI shows X, or someone asks how to run Y.">
---

# <Topic>

One paragraph: what this process is, who used to own it, why it matters, and the state of this doc (date captured, from whom, provenance legend). If a runbook was produced too, name its Confluence page title here as the human-maintained source of truth.

**Provenance legend:** `[verified]` = seen in an artifact or demonstrated · `[expert-stated]` = told to us, not demonstrated · `[inferred]` = our reconstruction, not contradicted by the expert. Trust in that order.

## Before you start
- Preconditions the expert checks without thinking (access, env vars, branch state, VPN, "is anyone else deploying").
- What to gather before touching anything (job ID, log link, commit SHA) — this doubles as the escalation package.

## Symptom → cause → fix
Lead with this. It is what a newcomer at 4:45pm actually needs.

| You see | Most likely | Do | Confidence |
|---|---|---|---|
| `<verbatim error fragment>` | <cause> | → Procedure A | [verified] |
| job hangs >10 min at step X | <cause> | → Procedure B | [expert-stated] |
| <symptom> | <cause> | **Stop. Escalate.** See Boundaries | [verified] |

Order rows by frequency (most common first). Include at least one "stop" row.

## Procedures
One per fix. Each is: when to use → steps → verify → if it didn't work.

### Procedure A — <name>  [provenance]
**Use when:** <condition from the symptom table>
**Steps:**
1. Literal command, with the real path and flags. Explain any flag that isn't obvious *in the step*, not in a footnote.
2. ...
**Verify:** How you know it worked. Specific: "job status shows SUCCESS and `artifacts/` contains a `.whl`", not "it should work."
**If it didn't:** The next branch. Or "→ Boundaries."

## Decision points
Every "it depends" from the interview, as explicit rules:
- **If** <condition> **then** <action>. *Because* <the reason the expert gave>.
- **If** unsure whether <X> → <cheap diagnostic to find out>.

## Scripts & tools
Short card per script (or link to `references/scripts.md` if many):
- **`path/to/script.sh`** — what it does in one line. Invocation: `...`. Always set `--flag` because <why>. Never use `--other` because <why>. Fails <loudly/silently>; leaves <state> if killed. Edit the script (not flags) when <situation>: change <line/section>.

## Boundaries
Mandatory. Newcomers over-act.
- **Never:** <action>, even if <tempting reason>. <Consequence.>
- **Stop and escalate when:** <condition>. Looks fixable; isn't.
- **Needs approval / ticket / second reviewer:** <action>.
- **Escalate to:** <role or channel, not just a name — names rot>. **Bring:** <the escalation package from "Before you start">.

## Known gaps
What the interview didn't resolve. Be honest; a newcomer who knows the doc is incomplete will ask, one who thinks it's complete will guess.
- <open question>
- <failure mode mentioned once, cause unknown>
```

## Writing rules for the generated skill

- **Verbatim over paraphrase** for anything greppable: error strings, job names, paths, flags, env var names. The newcomer will search for the literal text.
- **Real names, not placeholders.** If the expert said `BUILD_TAG`, write `BUILD_TAG`, not `<your tag variable>`. Only placeholder what truly varies per run, and say what it varies by.
- **"Because" on every rule.** A rule without a reason gets ignored the first time it's inconvenient. The reason is usually in the episode that taught it.
- **Verification is a step, not a hope.** If the expert couldn't articulate how they verify, write `[inferred]` and your best guess, and list it in Known gaps.
- **Thresholds are numbers.** "A while" → "more than ~10 minutes." If the expert gave a range, give the range.
- **Agent audience:** add guard conditions before any destructive step ("only if `git status` is clean"), exact tool calls, and an explicit "ask the human" rule at each boundary. Agents don't feel hesitation; write it in.
- **Human audience:** keep the *why* and the episode references. Humans generalize from stories better than from rules.
- **No motivational framing.** No "don't worry, this is easier than it looks." State it; move on.

## Annotated mini-example

```markdown
## Symptom → cause → fix

| You see | Most likely | Do | Confidence |
|---|---|---|---|
| `ERROR: could not resolve dependency parse-core>=2.4` in CI build log | Internal package index cache stale on the runner | → Procedure A | [verified] |
| Build passes locally, fails in CI with `ModuleNotFoundError: yaml` | `requirements-ci.txt` drifted from `requirements.txt` | → Procedure B | [expert-stated] |
| Build succeeds but `integration-smoke` stage times out | Downstream mock service down; **not a build problem** | **Stop.** Post in #platform-oncall with job URL | [verified] |

### Procedure A — refresh runner package cache  [verified]
**Use when:** dependency-resolve errors for *internal* packages (anything under `aw-*` or `parse-*`). Public packages failing to resolve is a different problem (→ Known gaps).
**Steps:**
1. `./scripts/ci/clear-pip-cache.sh --runner $(jq -r .runner_id ci-meta.json)` — the `--runner` flag matters; without it the script clears the *shared* cache and every other team's next build slows down for ~20 min. That's the "never" in Boundaries.
2. Re-run the failed job from the CI UI (not a new push — a new push re-triggers the full matrix, ~40 min).
**Verify:** Job log shows `Resolved parse-core 2.4.x from internal-index` within the first 2 min. If it resolves from `pypi.org` instead, the index config is wrong → Procedure C.
**If it didn't:** Second failure with the same error after a cache clear has only ever meant the package genuinely wasn't published. Check the `parse-core` release pipeline before doing anything else.
```

Notice: the "why" on the `--runner` flag came from an episode where the expert once cleared the shared cache. The "not a new push" note is a shortcut the expert used without ever mentioning it until asked "is there a faster way." The last line is a hedge converted to a specific claim with its scope ("has only ever meant") preserved.
