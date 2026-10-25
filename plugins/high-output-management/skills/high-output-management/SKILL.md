---
name: high-output-management
description: "Apply Andy Grove's High Output Management operating system to a manager's real situation: leverage, meetings and 1:1s, OKRs, indicators and metrics, task-relevant maturity, delegation and monitoring, performance reviews, and motivation. Use this skill whenever a manager or team lead asks how to get more output from their team, feels buried in meetings, is setting up or fixing 1:1s or staff meetings, writing OKRs or choosing team metrics, deciding how closely to manage or delegate to someone, writing or delivering a performance review, dealing with an underperformer or a disengaged report, or transitioning from individual contributor to manager — even if they never mention Grove or the book."
license: MIT
metadata:
  author: ClawFu (Guia, github.com/guia-matthieu/clawfu-skills)
  adapted_by: Colin M.
  version: 2.0.0
  mcp-server: "@clawfu/mcp-skills"
  source: "Andy Grove, High Output Management (1983; rev. 1995)"
  category: leadership
  tags: [management, Andy Grove, OKRs, leverage, meetings, indicators, performance-reviews, delegation]
---

# High Output Management

Andy Grove ran Intel like an engineer runs a production line: define the output, find the limiting step, measure it, and spend management time where it multiplies. This skill applies that operating system to the user's actual team, not as a book summary.

**Grove's core equation:** a manager's output = the output of their organization + the output of neighboring organizations under their influence.

Everything below follows from it. A manager's own work only counts to the extent it raises team output, so the question behind every recommendation is: *does this activity raise the output of the people I manage or influence, and by how much per hour I spend?*

## How to work a request

1. **Get the situation, briefly.** Team size and type of work, the manager's level, what's going wrong or what's being set up. If the user gave enough, don't interrogate; state assumptions and proceed. One or two targeted questions at most when a wrong guess would derail the output (for example, a performance review needs the person's actual output to work from).
2. **Pick the Grove concept(s) that actually fit.** Most requests need one or two, not all. Use the routing table below.
3. **Read the matching reference file** before producing the output. The references hold the detail, templates, and worked examples; this file only holds the spine.
4. **Produce a concrete artifact**, not a lecture: a weekly rhythm, a 1:1 agenda, OKRs with numbers, an indicator set, a review draft, a delegation plan. Fill in the user's specifics. Where data is missing, use clearly marked placeholders like `[current p95 latency]` rather than invented numbers.
5. **Name the Grove principle behind each recommendation** in a line, so the user can reason with it next time without the skill.
6. **End with the decisions that are theirs** (see "What Claude does vs. what you decide").

## Routing

| The user is asking about… | Grove concept | Read |
|---|---|---|
| "I'm in meetings all day", where to spend time, doing IC work as a manager | Leverage, meddling | this file + `references/examples.md` |
| 1:1s, staff meetings, ops reviews, decision meetings | Process- vs. mission-oriented meetings | `references/meetings.md` |
| Goals, quarterly planning, OKRs | Objectives and key results | `references/okrs.md` |
| Team metrics, dashboards, "how do I know if we're on track", catching problems early | Indicators, limiting step | `references/indicators.md` |
| How closely to manage someone, delegating, micromanagement, new lead | Task-relevant maturity, monitoring | this file + `references/examples.md` |
| Writing or delivering a review, underperformance conversations | Performance appraisal | `references/performance-reviews.md` |
| Disengaged report, "can't or won't", what drives people | Motivation | `references/motivation.md` |
| Weekly self-review, 1:1 notes template | Templates | `references/templates.md` |

## The spine: concepts every answer can draw on

### 1. Management as production

Treat the team like Grove's "breakfast factory": inputs move through steps to finished output, and one step (the *limiting step*) sets the pace. The manager's job is to find that step, protect it, and catch defects at the **lowest-value stage**, where fixing them is cheapest. For engineering teams that's usually design review before build, or a failing test before deploy. Fixing a defect in production is the most expensive place to find it.

Manager activities fall into three buckets: **information gathering, information giving, and decision-making** (plus being a role model, which runs through all three). A manager who only does the second is a broadcaster. One who only does the third without the first is guessing.

### 2. Leverage

Leverage = output generated per unit of the manager's time. Grove identifies high leverage when:

- **many people are affected** by one activity (a planning decision, a team-wide standard, training)
- **one person's output is affected over a long period** by a brief intervention (a well-timed piece of feedback, a clear objective)
- **the manager supplies a unique skill or piece of information** no one else has

**Negative leverage** is real, and the three biggest sources are:

- **Meddling:** doing or redoing work a subordinate owns. It teaches them to wait for you and destroys their ownership.
- **Delaying decisions:** a decision one person holds blocks everyone downstream. The cost multiplies by headcount.
- **Going into meetings unprepared:** wastes the time of everyone else in the room.

**Grove's practical test:** for each recurring activity, ask "does this raise team output, and would it happen without me?" Cut or delegate the ones that fail both.

Two levers for raising the leverage of the manager's own time:

- **Batch similar work** (1:1s on the same days, reviews in one block).
- **Use a calendar as a production schedule:** reserve time for the high-leverage, non-urgent work so it isn't squeezed out by interrupts.

### 3. Task-relevant maturity (TRM)

The right management style depends on the person's maturity **at this specific task**, not their seniority overall. A strong senior engineer newly made a tech lead is low-TRM at leadership.

| TRM | Grove's style | What it looks like |
|---|---|---|
| Low | Structured, task-oriented | Say what, when, and how. Short check-in loops. |
| Medium | Individual-oriented | Two-way discussion, support, reasoning together. Check results more than process. |
| High | Minimal involvement | Agree on objectives, then monitor lightly. |

A mismatch is expensive both ways:

- **Directive style on a high-TRM person:** they experience it as micromanagement and disengage.
- **Hands-off style on a low-TRM person:** they fail, and nobody notices until it's late.

Reassess when the task changes, the environment changes, or performance drops. The goal is to move people up the TRM curve so that management can step back, not to stay in a fixed style.

### 4. Delegation and monitoring

**Delegation without follow-through is abdication.** Once you hand something off, you still own its outcome, so you monitor. How you monitor matters:

- **Monitor at the lowest-value stage.** Check the design doc or the first PR, not the finished launch.
- **Sample, don't inspect everything.** Spot-check at varied intervals and depths. Inspecting everything is meddling. Inspecting nothing is abdication.
- **Scale monitoring frequency to TRM.** Monitor more often and in more detail at low TRM; set objectives and review results occasionally at high TRM.
- **Delegate what you know well, not only what you don't want to do.** You can only monitor what you understand. Delegating unfamiliar work and then not monitoring it is the most common failure.
- **Don't take it back.** If the work is going wrong, coach or redirect, and make the fix theirs.

Delegation done this way is high-leverage *because* it develops the person; delegation done as a dump is negative leverage disguised as efficiency.

### 5. Output over activity

Measure what the team produces (shipped changes, incidents resolved, customer-facing reliability), not how busy it looks. Activity indicators are acceptable only when output is too slow or noisy to measure, and they should be paired with an output check. This principle shows up in OKRs, indicators, and performance reviews alike.

## What Claude does vs. what you decide

| Claude does | You decide |
|---|---|
| Applies Grove's frameworks to your specific team and situation | Which priorities and trade-offs fit your org |
| Drafts rhythms, agendas, OKRs, indicator sets, review text | The final wording and what you actually commit to |
| Flags leverage traps, TRM mismatches, unpaired metrics | How to weigh people context Claude doesn't have |
| Proposes how closely to monitor delegated work | Whom to trust with what, and when to step in |
| Prepares structure and talking points for hard conversations | The judgment calls in the room and any HR/legal process |

## Skill boundaries

**Does well:** structuring management systems, meeting cadences, OKRs, team metrics, delegation plans, performance-review drafts and delivery plans, diagnosing leverage and TRM problems.

**Doesn't do:**

- Know your people, politics, or history beyond what you share. Outputs are only as good as the specifics given.
- Replace HR, legal, or company policy for performance improvement plans, terminations, compensation, or anything with employment-law implications.
- Account for compensation design or company-specific review calibration processes.
- Answer questions outside Grove's system. For feedback style, pair this skill with a feedback framework (e.g., radical-candor).

**Accuracy note:** some popular "Grove" advice actually comes from later practitioners. Two examples are John Doerr's *Measure What Matters* (OKR scoring conventions like 0.7 = success) and Google's OKR practice. When a recommendation comes from them rather than from Grove, say so.

## References

- Grove, Andrew S. *High Output Management*. Random House, 1983; Vintage revised ed., 1995.
- Doerr, John. *Measure What Matters*. Portfolio, 2018. (OKR practice after Grove)

## Related skills

- `radical-candor`: feedback methodology
