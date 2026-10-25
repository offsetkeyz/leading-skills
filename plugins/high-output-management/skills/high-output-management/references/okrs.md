# OKRs

## Origin (get the attribution right)

Grove built a management-by-objectives system at Intel in the 1970s. Its internal name was iMBO ("Intel Management by Objectives"), and its two questions were *Where do I want to go?* (objective) and *How will I pace myself to see if I'm getting there?* (key results). John Doerr learned it at Intel, brought it to Google in 1999, and popularized the term "OKRs". *Measure What Matters* (2018) is his account.

**Grove's** original points:

- **Few objectives.** Grove warned that the system works only with a small number of objectives, because trying to focus on everything means focusing on nothing.
- **Key results are milestones, verifiable at period end.** When the period closes, there should be no argument about whether a key result was hit. "Improve reliability" fails this test. "Error budget burn < 50% for 3 consecutive months" passes.
- **Set them high.** Stretch objectives drive higher output even when they're not fully met.
- **Not a legal document.** OKRs are a planning and focus tool, not the sole basis of a performance review. A person can miss an OKR for good reasons, such as priorities shifting mid-quarter, and still perform well.

**Later practice, from Doerr and Google** (label it as such when used):

- Scoring from 0.0 to 1.0, where roughly 0.7 counts as success.
- The split between committed OKRs (expected at 1.0) and aspirational OKRs (expected around 0.7).
- Making OKRs public across the org.
- Explicitly decoupling OKRs from compensation.

## Writing good OKRs

- **Objective:** qualitative, directional, and time-bound (usually a quarter). Answers "what are we trying to achieve?"
- **Key results:** quantitative, with a baseline and a target. Three to five per objective.
- **Outputs, not activities.** "Ship the retry framework" is an initiative. "Reduce failed ingestion events from 2.1% to 0.5%" is a key result.
- **Pair key results that can be gamed** (see `indicators.md`). A throughput KR needs a quality KR next to it.
- **Cascade by alignment, not by copying.** A team's OKRs should visibly support the level above, while still being the team's own.

### Bad → good (engineering)

| Bad | Why | Better |
|---|---|---|
| Improve the platform | Not verifiable | Reduce p95 API latency from 820 ms to 400 ms |
| Ship features | Activity, no target | 3 of the top-5 customer-requested integrations GA by Mar 31 |
| Fix bugs | No baseline | Reduce open Sev-2 bugs from 46 to 15 |
| Be more reliable | Not measurable | 99.9% monthly availability for the ingestion API, 3 consecutive months |

## Worked example: platform / integrations engineering team

**Context:** 9 engineers own about 40 third-party data integrations feeding a detection platform. Pain points: parser breakages after vendor API changes, a growing backlog of new-integration requests, and on-call fatigue.

### Objective 1: Integrations break less and recover faster

| KR | Baseline | Target |
|---|---|---|
| 1.1 Integration-caused incidents per month | `[8]` | ≤ 3 |
| 1.2 Median time to restore a broken integration | `[19 h]` | < 4 h |
| 1.3 Integrations covered by vendor contract tests | `[25%]` | 80% |
| 1.4 (pair) Change failure rate on integration deploys | `[12%]` | ≤ 12%, i.e. doesn't get worse |

**Initiatives** (not KRs): build a contract-test harness, add schema-drift alerting, write a runbook per top-10 integration.

### Objective 2: New integrations reach customers faster

| KR | Baseline | Target |
|---|---|---|
| 2.1 Lead time from request accepted to GA | `[11 wks]` | 6 wks |
| 2.2 New integrations shipped GA | `[2/qtr]` | 5 |
| 2.3 (pair) New integrations with an incident in the first 30 days | `[50%]` | ≤ 20% |

### Objective 3: On-call is sustainable

| KR | Baseline | Target |
|---|---|---|
| 3.1 Pages per on-call week | `[34]` | ≤ 12 |
| 3.2 Pages that were actionable | `[40%]` | ≥ 80% |
| 3.3 Engineers able to take primary on-call | `[5/9]` | 8/9 |

Notice the pairs: 1.4 keeps 1.1 from being hit by freezing deploys, and 2.3 keeps 2.2 from being hit by shipping junk.

## Cadence

- **Quarter start:** draft bottom-up and top-down, reconcile them, and commit. Keep it to a small set.
- **Weekly:** check KR progress against indicators in the staff meeting, and adjust *activities* rather than OKRs.
- **Mid-quarter:** change an OKR only if the environment changed materially. Record why.
- **Quarter end:** score it (Doerr scale if the team uses one), run a short retro on what was learned, and feed that into the next set.

## Common failure modes

- **Too many OKRs:** focus collapses.
- **Key results that are really a task list:** they don't measure outcomes.
- **Treating OKRs as the performance review:** this produces sandbagging.
- **Setting OKRs and never reviewing them:** they become wallpaper.
