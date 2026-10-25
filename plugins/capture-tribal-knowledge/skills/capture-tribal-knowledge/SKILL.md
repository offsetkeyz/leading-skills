---
name: capture-tribal-knowledge
description: Interview a subject-matter expert to extract tacit/tribal knowledge (how they really fix broken builds, run and tweak scripts, triage recurring issues, decide when to escalate) and turn it into a reusable Claude skill another person or agent can follow. Use this whenever someone wants to document, hand off, offboard, transfer, or "write down" know-how that lives in a person's head — knowledge transfer, runbook from memory, handoff docs, "turn what Sam knows into a skill", bus-factor reduction, team dissolution or re-org handoffs — even if they don't say the word "skill". Works for software engineering (build/CI/deploy/script knowledge) and for any other domain.
---

# Capture Tribal Knowledge

Tribal knowledge is the stuff experts can't tell you because they no longer notice they know it. Ask a senior dev "how do you fix the build?" and you get "depends." Ask "walk me through the last time it broke" and you get three pages of gold. This skill is an interview-and-synthesis workflow that gets the gold out and files it as a Claude skill someone else can run.

Two deliverables, always:
1. **A generated skill** (`<topic>/SKILL.md` + `references/`) written for the person or agent *taking over* the work.
2. **A short coverage report** telling the expert what was captured, what is still vague, and what to verify.

## Why interviewing, not asking for a doc

Experts asked to "write it down" produce the textbook version: the happy path, in order, with the tells of experience stripped out. The tacit layer — what they check first, what they ignore, what the error message *actually* means, the flag they always add — only surfaces under three conditions: concrete episodes, contrast, and being made to correct a wrong reconstruction. The whole protocol is built to create those conditions. Keep that in mind when you improvise.

## Workflow

### 0. Scope (2–3 questions, no more)

Settle these before the interview, in one message:
- **Topic**: one process, not a role. "Fixing failed `parse_and_detect` integration builds" not "what Divya does." If they give a role, split it into candidate topics and ask which one first.
- **Audience**: who runs this next — a human engineer, a Claude agent, or both? This changes how much you spell out tooling.
- **Mode**: live interview (back-and-forth here) or async (you produce a questionnaire the expert fills out in their own time; see `references/interview-playbook.md` → "Async mode").

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

### 2. Synthesize into a skill

Read `references/skill-template.md` and build the output skill from it. The important design choices:

- **Organize by symptom, not by step.** The person taking over will arrive with a failing build and an error message, not with a desire to read a procedure. Lead with a symptom → likely cause → fix table, then the detailed procedures.
- **Put judgment in the open.** Every "it depends" from the interview becomes an explicit branch: *If X, do A. If Y, do B. If unsure, see "When to stop."*
- **Scripts get a card each**: what it does, exact invocation, flags that matter and *why*, known failure modes, what to modify for which situation. Include literal commands and real paths/names from the interview; never substitute placeholders when the real value was given.
- **Make verification explicit.** For every fix: how do you know it worked? Experts verify unconsciously; newcomers skip it.
- **Boundaries section is mandatory.** What never to do, what needs a human, what looks fixable but isn't.
- **Tag provenance.** Mark each procedure `[verified]` (artifact seen / demonstrated), `[expert-stated]`, or `[inferred]` (you reconstructed it and the expert didn't contradict). The newcomer deserves to know which parts are solid.
- **Write for the audience chosen in step 0.** For an agent audience, include exact tool invocations and guard conditions. For a human, include the *why* more generously.

Keep the generated SKILL.md under ~300 lines; push long script cards, log samples, and edge-case catalogs into `references/`.

### 3. Coverage report

After writing the skill, give the expert a short report (in conversation, not a file unless asked):
- What's covered and verified
- What's `[expert-stated]` only — worth a second look
- **Open questions**: things that came up but weren't resolved, or failure modes mentioned once and never explained
- Suggested next session topic, if the scope was split in step 0

Then offer one round of revision: "Read it as if you were out sick and your replacement had only this. What would they get wrong?" That single question reliably surfaces another 10–20% of knowledge.

### 4. Package

If a file-delivery tool is available, package the generated skill (zip or `.skill`) and send it. Otherwise, write the folder and tell the user where it is.

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
- `references/skill-template.md` — the output structure for the generated skill, with an annotated example. Read before synthesizing.
- `references/example-session.md` — a condensed worked example (failed integration build) showing how a tell becomes a question becomes a documented branch. Read if unsure how deep to go.
