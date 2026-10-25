# Example Session (condensed)

Topic: "Fixing failed integration builds in `parse_and_detect`." Audience: human engineers on the receiving team + a Claude agent for first-pass triage. Live mode. ~30 exchanges, shown here at ~12 with commentary on *why* each question was asked.

Fictional details; the pattern is what matters.

---

**Claude:** Walk me through fixing a failed integration build, start to finish, as if I'd never seen it. Five to eight steps.

**Expert:** Look at the CI log, figure out which stage died, usually it's deps or the smoke stage, fix it, rerun.

> *Four steps, two "usually"s, one "figure out." The map is thin, which is normal. Don't push on the map; get the names.*

**Claude:** What's the CI system, and which stages are there?

**Expert:** GitLab. Stages are `lint`, `unit`, `build-wheel`, `integration-smoke`, `publish`. Nobody cares about lint failures, they're obvious.

> *"Nobody cares" — a tell. Noted, not chased yet. Moving to episodes early because the map is already at its ceiling.*

**Claude:** Tell me about the last time a build failed. The actual last one, not a typical one. What did you see first?

**Expert:** Last Thursday. Mike pinged me, `build-wheel` was red. I opened the log, saw `could not resolve dependency parse-core>=2.4`, knew it was the cache, cleared it, reran, done.

> *"Knew it was the cache" — how? That's the cue we need.*

**Claude:** What told you it was the cache and not that `parse-core` 2.4 didn't exist?

**Expert:** Because it's an internal package. If a public package doesn't resolve, that's a real problem. Internal ones almost always mean the runner's cache is stale because someone published a new version an hour ago.

> *Decision rule captured: internal vs public package → different cause. "Almost always" → there's a branch. Chase it.*

**Claude:** Almost always. When wasn't it the cache?

**Expert:** Once the release pipeline for `parse-core` had failed silently and the version was never actually published. Cleared cache twice before I checked.

> *An episode with a dead end. Becomes: "If second failure after cache clear → check the release pipeline before anything else." Also note: release pipeline "fails silently" — a candidate for Known gaps or a separate topic.*

**Claude:** How do you clear the cache? Paste the command if you can.

**Expert:** `./scripts/ci/clear-pip-cache.sh --runner <id>`. You get the runner id from `ci-meta.json`. Always pass `--runner`.

> *"Always" — why?*

**Claude:** What happens if you don't pass `--runner`?

**Expert:** It clears the shared cache for all runners. Every team's next build takes 20 extra minutes. I did that once. People noticed.

> *This is the single most valuable fact in the session and it would never have appeared in a written runbook. Goes straight into Boundaries as a "Never," with the consequence.*

**Claude:** After clearing — do you rerun from the UI or push a new commit?

**Expert:** UI, retry the one job. A new push re-triggers the whole matrix, that's 40 minutes.

> *Shortcut captured. The expert didn't mention this until asked directly — it was "obvious."*

**Claude:** How do you know the rerun actually fixed it, versus just happening to pass?

**Expert:** Log should say it resolved from `internal-index`. If it says `pypi.org`, the index config is wrong, that's a different fix.

> *Verification step captured, plus a third branch (index config). Enough episodes; moving to judgment probes on the other stage.*

**Claude:** You said `integration-smoke` is the other usual failure. What do new people get wrong there?

**Expert:** They treat it like a build problem. Nine times out of ten it's the downstream mock service being down, which isn't ours. They'll spend two hours reading our code. Just check the mock service health page first.

> *Novice contrast delivered a "stop" row for the symptom table and the first-check cue. "Just check the health page" — where?*

**Claude:** Where's the health page, and who owns the mock service?

**Expert:** `https://mock-svc.internal/health`. Platform team, `#platform-oncall`.

**Claude:** Teach-back. Here's my reconstruction — tell me where I'm wrong:
1. Open GitLab log, find failed stage.
2. If `build-wheel` with dependency-resolve error on internal package → clear runner cache with `--runner`, retry job from UI, verify log shows `internal-index`. Second failure → check `parse-core` release pipeline.
3. If `integration-smoke` fails → check mock service health page before reading any code. Down → post job URL in `#platform-oncall`.
4. Lint failures: ignore?

**Expert:** Mostly right. Lint — don't *ignore*, just they're self-explanatory, the log tells you the line. And you're missing the one where `unit` passes locally but fails in CI. That's `requirements-ci.txt` drifting from `requirements.txt`. People forget there are two.

> *Teach-back worked: the deliberately-weak "ignore?" drew a correction, and the reconstruction prompted a whole missed failure mode. That's the pattern — experts spot gaps in a draft far more readily than they volunteer them.*

**Claude:** Last one. If you were out two weeks and your replacement had only this doc — what's the first thing they'd break?

**Expert:** They'd run the cache clear without `--runner`. Put that in bold.

---

## What went into the output skill from this

- Symptom table: 4 rows (3 fixable, 1 stop) — all `[verified]` or `[expert-stated]`
- Procedures: A (cache), B (requirements drift), C (index config, `[inferred]` — never walked through)
- Boundaries: "Never run `clear-pip-cache.sh` without `--runner`" with the consequence
- Decision rule: internal vs public package
- Shortcut: retry job from UI, not new push, with the 40-minute reason
- Known gaps: `parse-core` release pipeline "fails silently" — how to detect; Procedure C never demonstrated; what "self-explanatory" lint failures look like to someone who's never seen one
- Suggested next session: release pipeline for internal packages

## Why this worked

- Went to episodes by exchange 3; didn't let the map phase absorb the session.
- Every tell got chased within one exchange ("almost always," "always pass," "just check").
- Asked for the command, not a description of the command.
- Teach-back included a deliberate weak spot (lint) and surfaced a missed failure mode.
- The "out sick" question confirmed the highest-risk item rather than discovering a new one — that's the signal coverage is decent.
