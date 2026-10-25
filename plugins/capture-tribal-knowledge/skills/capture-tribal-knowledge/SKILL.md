---
name: capture-tribal-knowledge
description: Interview a subject-matter expert to extract tacit/tribal knowledge (how they really fix broken builds, run and tweak scripts, triage recurring issues, decide when to escalate) and turn it into a Confluence-ready markdown runbook, a reusable Claude skill, or both. Use this whenever someone wants to document, hand off, offboard, transfer, or "write down" know-how that lives in a person's head — knowledge transfer, runbook from memory, wiki/Confluence page for a process, handoff docs, "turn what Sam knows into a skill", bus-factor reduction, team dissolution or re-org handoffs — even if they don't say "skill" or "runbook". Also use it when someone pastes interview notes, Slack threads, or questionnaire answers from an expert and wants them turned into a handoff doc. Works for software engineering (build/CI/deploy/script knowledge) and for any other domain.
---

# Capture Tribal Knowledge

Tribal knowledge is the stuff experts can't tell you because they no longer notice they know it. Ask a senior dev "how do you fix the build?" and you get "depends." Ask "walk me through the last time it broke" and you get three pages of gold. This skill is an interview-and-synthesis workflow that gets the gold out and files it where the next person will actually look.

Deliverables:
1. **The captured knowledge, in the format(s) chosen in step 0** — written for the person or agent *taking over* the work:
   - **Runbook** — one markdown page that pastes cleanly into Confluence (or any wiki). For humans.
   - **Skill** — a `<topic>/SKILL.md` + `references/` folder a Claude agent loads and follows.
   - **Both** — same facts, two renderings.
2. **A short coverage report** telling the expert what was captured, what is still vague, and what to verify.

The interview is identical whichever format you pick. Only step 2 (synthesis) changes, so the format decision never needs to slow the interview down.

## Why interviewing, not asking for a doc

Experts asked to "write it down" produce the textbook version: the happy path, in order, with the tells of experience stripped out. The tacit layer — what they check first, what they ignore, what the error message *actually* means, the flag they always add — only surfaces under three conditions: concrete episodes, contrast, and being made to correct a wrong reconstruction. The whole protocol is built to create those conditions. Keep that in mind when you improvise.

## Workflow

### 0. Scope (2–3 questions, no more)

Settle these before the interview, in one message:
- **Topic**: one process, not a role. "Fixing failed `parse_and_detect` integration builds" not "what Mike does." If they give a role, split it into candidate topics and ask which one first.
- **Audience & format**: who runs this next — a human engineer, a Claude agent, or both? The answer picks the output format unless the user says otherwise:

  | Audience | Default format | Why |
  |---|---|---|
  | Humans (team wiki, on-call, new hire) | **Runbook** | Humans search the wiki, not a skills folder |
  | Claude agent (first-pass triage, automation) | **Skill** | Agents need trigger description, guard conditions, exact tool calls |
  | Both, or unsure | **Both** | Cheap: the hard part is the interview; rendering twice is minutes |

  Offer the default and let them override in one line ("Runbook for Confluence, skill, or both? I'd suggest both since you mentioned an agent doing first-pass triage."). If the user already said "Confluence", "wiki", "runbook", or "skill", don't ask — that's the answer.
- **Mode**: one of three —
  - **Live**: interview the expert here, back-and-forth.
  - **Async**: produce a questionnaire the expert fills out in their own time (`references/interview-playbook.md` → "Async mode").
  - **From material**: the user already has notes, call transcripts, Slack threads, filled-in questionnaires, or an old doc. Skip to step 1b. If they've attached material, this is the mode — don't ask.

If a repo, script, log, or existing doc is available, ask for it now. Reading real artifacts lets you ask *specific* questions ("line 42 greps for `BUILD_TAG` — what sets that?") instead of generic ones, and specificity is what unlocks tacit knowledge.

### 1. Interview

Read `references/interview-playbook.md` before starting. It has the phase structure, question banks, and the "tells" that signal buried knowledge. Summary of the phases:

| Phase | Goal | Core move |
|---|---|---|
| Map | Get the skeleton of the task in the expert's own words | "Walk me through it start to finish, in 5–8 steps." |
| Episodes | Replace generalities with real incidents | "Tell me about the last time it went wrong." (×3, different failure modes) |
| Judgment | Surface decision points, cues, shortcuts | Knowledge-audit probes: what do you notice, what do you check first, what do novices get wrong |
| Teach-back | Force corrections | You reconstruct the procedure; expert fixes it |
| Boundaries | Capture when NOT to proceed | Escalation triggers, dangerous actions, "if you see X, stop" |

Rules of engagement:
- **One question at a time** in live mode. Batching questions gets you shallow answers to all of them.
- **Chase the tells.** "Obviously", "just", "usually", "you have to", "it depends", "everyone knows" — each one is a door. Open it: "It depends on what?"
- **Ask for the artifact, not the description.** "Paste the error" beats "what does the error say." "Paste the command you ran" beats "which script."
- **Prefer episodes to rules.** When the expert states a rule, ask for the incident that taught it to them. The incident contains the cue the rule omits.
- **Track confidence.** Note whether each fact was demonstrated (artifact seen), stated firmly, or hedged. This tagging flows into the output.
- **Stop when returns diminish**, not when a checklist is done. Typical live session: 20–40 exchanges. If the expert is tired or repeating, move to teach-back.

### 1b. From existing material

Notes and questionnaires are an interview that already happened, without the follow-ups. Treat them as raw episodes, not as a finished procedure — they reliably capture 60–70% of what a live session would, and the missing part is exactly the tacit layer (cues, thresholds, what "usually" means).

1. **Read everything first**, then sort facts into the running-notes format at the end of the playbook. Tag provenance strictly: pasted errors/commands/logs → `[verified]`; the expert's own words → `[expert-stated]`; anything hedged ("I think", "usually", "not sure") keeps its hedge; your connective tissue → `[inferred]`. Notes taken *by someone else* about the expert are one step weaker — say so in the intro.
2. **Run the tells over the text.** Every "usually", "just", "it depends", unnamed script, or missing threshold you'd have chased live becomes a follow-up question.
3. **Write the output now** — don't block on the expert. A draft with honest gaps is more useful than a perfect doc next month, and a draft is the best teach-back prompt there is.
4. **Hand back a follow-up list** with the coverage report: the 5–10 highest-value questions, ordered by risk to the newcomer (destructive steps and silent failures first), phrased so the expert can answer in a line each. Offer to fold their answers in as a second pass.

If the expert is reachable and the material is thin (a few bullets, no episodes), say so and recommend a short live or async round before synthesizing.

### 2. Synthesize

Read the template for each chosen format before writing:
- Runbook → `references/runbook-template.md` (Confluence-safe markdown rules, page structure)
- Skill → `references/skill-template.md`

Both formats share the same content principles:

- **Organize by symptom, not by step.** The person taking over will arrive with a failing build and an error message, not with a desire to read a procedure. Lead with a symptom → likely cause → fix table, then the detailed procedures.
- **Put judgment in the open.** Every "it depends" from the interview becomes an explicit branch: *If X, do A. If Y, do B. If unsure, see "When to stop."*
- **Scripts get a card each**: what it does, exact invocation, flags that matter and *why*, known failure modes, what to modify for which situation. Include literal commands and real paths/names from the interview; never substitute placeholders when the real value was given.
- **Make verification explicit.** For every fix: how do you know it worked? Experts verify unconsciously; newcomers skip it.
- **Boundaries section is mandatory.** What never to do, what needs a human, what looks fixable but isn't.
- **Tag provenance.** Mark each procedure `[verified]` (artifact seen / demonstrated), `[expert-stated]`, or `[inferred]` (you reconstructed it and the expert didn't contradict). The newcomer deserves to know which parts are solid.
- **Write for the audience chosen in step 0.** For an agent audience, include exact tool invocations and guard conditions. For a human, include the *why* more generously.

Format-specific differences, in brief (details in the templates):

| | Runbook | Skill |
|---|---|---|
| Front matter | Page-properties table (owner, source expert, captured, review-by) | YAML `name` + pushy `description` naming error strings and job names |
| Length | One page; long script cards and log samples go in appendix sections at the bottom | SKILL.md ≤ ~300 lines; overflow into `references/` |
| Warnings | Bold-labelled blockquotes (`> **Warning:** …`) — they survive paste | Inline in Boundaries |
| Agent guards | Omit "ask the human" rules; keep the *why* and episode stories | Add guard conditions before destructive steps and explicit stop-and-ask rules |
| Provenance | Same tags, in a Confidence column / after headings | Same tags |

**When producing both:** write the runbook first, then the skill from it, so the facts match exactly. In the skill's intro paragraph, name the runbook page title as the human-maintained source of truth, so whoever updates one knows the other exists.

### 3. Coverage report

After writing the output, give the expert a short report (in conversation, not a file unless asked):
- What's covered and verified
- What's `[expert-stated]` only — worth a second look
- **Open questions**: things that came up but weren't resolved, or failure modes mentioned once and never explained
- Suggested next session topic, if the scope was split in step 0

Then offer one round of revision: "Read it as if you were out sick and your replacement had only this. What would they get wrong?" That single question reliably surfaces another 10–20% of knowledge.

### 4. Deliver

- **Runbook:** write `<topic-slug>-runbook.md` and tell the user the suggested Confluence page title, parent page, and labels (from the template). If a Confluence/Atlassian tool is connected and the user asks, create the page directly; otherwise leave publishing to them — a wiki page is visible to the whole team, so it's their call.
- **Skill:** write the `<topic-slug>/` folder. If a file-delivery tool is available, package it (zip or `.skill`) and send it; otherwise tell the user where it is.

## Adapting to the domain

The playbook's question banks are engineering-flavored (builds, scripts, CI, environments) because that's the most common case, but the phases are domain-neutral. For a non-engineering topic, keep the phases and swap the vocabulary: "script" → "template/form/tool", "build break" → "the thing that goes wrong", "logs" → "whatever evidence you look at." Don't force engineering framing onto a support, ops, or clinical process.

## Anti-patterns to avoid

- Producing a generic runbook that could have been written without the interview. If the output doesn't contain at least a few things that surprised *you*, you stopped too early.
- Letting the expert narrate the happy path for twenty minutes. Redirect to failures by exchange three or four.
- Treating a hedge as a fact. "I think it's usually the cache" is `[expert-stated]`, hedged — flag it, don't assert it.
- Asking "anything else?" at the end. It returns nothing. Use teach-back or the "out sick" question instead.
- Letting scope creep into a second process mid-interview. Note it as a future topic and return.

## Files

- `references/interview-playbook.md` — phases, question banks, tells, probes, async-mode questionnaire generator. Read before any interview.
- `references/runbook-template.md` — Confluence-ready runbook structure and the markdown subset that survives paste. Read before writing a runbook.
- `references/skill-template.md` — the output structure for the generated skill, with an annotated example. Read before writing a skill.
- `references/example-session.md` — a condensed worked example (failed integration build) showing how a tell becomes a question becomes a documented branch. Read if unsure how deep to go.
