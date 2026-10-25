# Interview Playbook

Techniques here are adapted from Applied Cognitive Task Analysis (Militello & Hutton), the Critical Decision Method (Klein), and Flanagan's Critical Incident Technique — all built for exactly this problem: getting expertise out of people who can't articulate it. Don't cite them to the expert; just use them.

Contents
1. The tells — words that mark buried knowledge
2. Phase 1: Map
3. Phase 2: Episodes
4. Phase 3: Judgment (knowledge audit)
5. Phase 4: Teach-back
6. Phase 5: Boundaries
7. Probes you can use any time
8. Reading artifacts
9. Async mode (questionnaire)
10. Running notes format

---

## 1. The tells

Listen for these. Each one is compressed knowledge; your job is to decompress it.

| Expert says | What's hidden | Ask |
|---|---|---|
| "obviously", "of course" | A rule they've forgotten is a rule | "What would someone see if they didn't know that?" |
| "just" (as in "just rerun it") | A procedure with unstated preconditions | "Just rerun it — with the same args? From where? What if it fails again?" |
| "usually", "normally", "most of the time" | A branch exists | "And the other times?" |
| "it depends" | A decision rule | "On what? Give me the two most common cases." |
| "you have to" / "you can't" | A hard constraint learned the painful way | "What happens if you don't? When did you learn that?" |
| "the right way" / "properly" | A known-bad alternative that newcomers reach for | "What's the wrong way people try?" |
| "a while", "a bit", "pretty quickly" | A threshold | "How long before you'd worry?" |
| "that one script", "the usual fix" | A named artifact they assume you know | "Which one — path or name?" |
| "I can tell" / "you can see" | A perceptual cue | "Tell from what, specifically? What does it look like?" |
| "I'd probably", "I think" | Hedged knowledge | Tag it hedged; ask for the last time they actually did it |
| skipping a step quickly | Automaticity | "Slow down — what happened between X and Y?" |

## 2. Phase 1: Map (5–8 exchanges)

Goal: skeleton of the task in the expert's structure, not yours.

Open with:
> "Walk me through [topic] start to finish, as if I'd never seen it. Aim for 5–8 steps; we'll zoom in after."

Then:
- Reflect the steps back as a numbered list. Ask which 1–2 steps are hardest *for a newcomer* (not for them — they find all of it easy).
- Ask which steps they'd worry about handing off. That's your priority order for Phase 3.
- Ask what tools, scripts, repos, dashboards, and environments are involved. Get exact names and paths now; you'll need them for specific questions later.

Don't dig yet. The map is just to know where to dig.

## 3. Phase 2: Episodes (8–15 exchanges)

Goal: replace generalities with three or more real incidents. Episodes carry the cues, the dead ends, and the recovery steps that rules omit.

Opening question:
> "Tell me about the last time this went wrong. Not a typical case — the actual last one. What did you see first?"

Walk the timeline:
1. **Trigger** — how did they find out? (alert, someone pinged, noticed in passing)
2. **First look** — what did they check first, and why that first?
3. **Hypotheses** — what did they think it was? What ruled things out?
4. **Dead ends** — did they try something that didn't work? (These are the newcomer's most likely mistakes.)
5. **Fix** — exact action. Command, flag, file, line.
6. **Verification** — how did they know it was fixed?
7. **Aftermath** — anything they changed so it wouldn't recur? Anything they *should* have but didn't?

Then:
> "Now one that was different — a different root cause. What's a failure that *looks* like that one but isn't?"

Aim for three episodes covering distinct root causes. If they offer a fourth with a new cause, take it. If the fourth repeats a cause, move on.

Episode probes:
- "What would a newcomer have done at that point?"
- "What was the moment you knew what it was?"
- "If you'd been wrong, what would it have been instead?"
- "What did the error message say, verbatim? Paste it if you can."

## 4. Phase 3: Judgment — knowledge audit (6–12 exchanges)

Goal: surface the decision points and the cues that drive them. Work through the hard steps identified in Phase 1. For each one, pick the probes that fit:

**Cues and patterns**
- "When you look at [the log / the failing job / the output], what do you notice first? What do you ignore?"
- "Is there anything that tells you immediately this is going to be a long one vs a quick fix?"

**Big picture**
- "What's going on upstream/downstream that someone fixing this needs to understand?"
- "What else depends on this that could break if the fix is wrong?"

**Shortcuts and workarounds**
- "Is there a faster way you do this that isn't the documented way? Why does it work?"
- "What do you do that you probably shouldn't, but it's fine because you know X?" (Capture X; that's the guard condition.)

**Anomalies**
- "What's the weirdest version of this you've seen? What tipped you off that it was weird?"

**Self-monitoring**
- "How do you know when you're going down the wrong path? What makes you back up?"
- "How long do you spend before you ask someone? Who?"

**Improvisation**
- "If [usual tool/script] weren't available, what would you do instead?"

**Novice contrast** (the single highest-yield probe family)
- "What do new people get wrong here?"
- "What's a mistake you made early on that you'd never make now?"
- "If I handed this to a smart engineer who'd never touched this repo, where would they get stuck first?"

**Script-specific** (for each script that came up)
- "What does it actually do? Not the README — what does it do."
- "Which flags do you always set? Which do you never touch? Why?"
- "When do you edit the script itself instead of passing flags? What do you change?"
- "What does it do when it fails? Does it fail loudly or silently?"
- "What state does it leave behind if you kill it halfway?"

## 5. Phase 4: Teach-back (3–6 exchanges)

Goal: force corrections. Experts are far better at spotting errors in a reconstruction than at generating complete accounts from scratch.

Reconstruct the procedure yourself — as a numbered procedure with the branches you've heard — and present it:
> "Here's how I'd do it based on what you've told me. Tell me where I'm wrong or where I'd get stuck."

Deliberately include one plausible-but-wrong inference if you're unsure about a branch. The correction tells you more than the confirmation would.

Then ask:
> "Imagine you're out sick for two weeks and your replacement has only this. What's the first thing they'd screw up?"

Repeat teach-back once more with the corrections folded in. If the second pass gets no corrections, the map is good enough.

## 6. Phase 5: Boundaries (3–5 exchanges)

Goal: capture when to stop. Newcomers over-act; this section is what keeps them from making it worse.

- "What should someone *never* do here, even if it seems like it would help?"
- "What looks fixable but is actually a 'stop and get someone' situation?"
- "What's the blast radius if this goes wrong? Prod? Customers? Just the dev?"
- "Is there anything that needs approval, a change ticket, or a second pair of eyes?"
- "Who do they escalate to, and what should they have gathered before they do?" (The escalation package — logs, job ID, what was tried — is a deliverable of its own.)

## 7. Probes you can use any time

- **Laddering**: "Why?" — then "Why that?" — three levels down usually reaches the real constraint.
- **Counterfactual**: "What if [env / branch / time of day / data size] were different — would you do the same thing?"
- **Quantify**: "How often does that happen — once a week? once a quarter?"
- **Prioritize**: "If you could only tell the next person three things, what are they?"
- **Freeze-frame**: "Stop there. At that exact moment, what's on your screen?"

## 8. Reading artifacts

If the user shares a repo, script, log, or doc: read it before or during the interview and generate questions *from* it. Specific beats generic every time:

- "`deploy.sh` line 31 `set -e`s but line 58 pipes through `grep` — if grep finds nothing, does the script die? Is that intended?"
- "This log shows `retry 3/3` then success. Is three retries normal or is that already a smell?"
- "The README says to run `make build`; you said `make build-local`. Which is right and when?"

Artifact-sourced facts you confirm with the expert become `[verified]`. Facts you infer from code the expert hasn't confirmed stay `[inferred]`.

## 9. Async mode (questionnaire)

When the expert can't do a live session, generate a questionnaire they fill in (Slack thread, Notion page, Google Doc — whatever the team uses). Design rules:

- **Lead with episodes, not procedures.** First question is always "Describe the last 3 times this went wrong." Procedures come second.
- **Ask for pasted artifacts** explicitly: "Paste the exact command you ran" / "Paste the error text."
- **Use the novice-contrast probes** — they work in writing.
- **One sheet, 12–18 questions, ~30–45 minutes to answer.** Longer gets abandoned.
- **Include a teach-back draft** if you already have any material: "Here's my understanding; mark anything wrong."
- Always end with: "What did I not ask that I should have?"

Questionnaire skeleton:

```
## [Topic] knowledge capture — ~30 min

### Recent incidents
1. Describe the last 3 times this went wrong. For each: what you saw first, what you checked, what the fix was, how you confirmed it.
2. Paste the exact error/output from at least one of them.

### The procedure
3. List the steps as you actually do them (not the README version).
4. Which step would a newcomer get stuck on? Why?
5. What do you check *before* starting that isn't written anywhere?

### Scripts & tools
6. For each script/tool you use: path, the command you actually type, flags you always/never use, and when you edit the script instead.
7. What do these tools do when they fail? Loud or silent?

### Judgment
8. What tells you this will be quick vs. a long one?
9. What's a shortcut you use that isn't the "official" way, and why it's safe?
10. What's the weirdest case you've seen?
11. What did you get wrong when you were new to this?

### Boundaries
12. What should the next person never do here?
13. What looks fixable but means "stop and escalate"? To whom, with what info?

### Gaps
14. What did I not ask that I should have?
```

After answers come back, run a short live or written teach-back round if at all possible — async answers alone are reliably ~60–70% complete.

## 10. Running notes format

Keep a working scratch structure as you interview, so synthesis is fast:

```
TOPIC:
AUDIENCE:
STEPS (expert's words):
HARD STEPS:
EPISODES:
  E1: trigger / first look / hypotheses / dead ends / fix / verify / aftermath — [cause]
  E2: ...
DECISION POINTS:
  - condition → action  [verified|expert-stated|inferred]
SCRIPTS:
  - name/path / does / invocation / flags / failure mode / when-to-edit
NEVER DO:
ESCALATE WHEN / TO WHOM / BRING:
OPEN QUESTIONS:
SURPRISES (things you didn't expect):
```

If "SURPRISES" is empty by the end, you haven't reached the tacit layer. Go back to Phase 3 with the novice-contrast probes.
