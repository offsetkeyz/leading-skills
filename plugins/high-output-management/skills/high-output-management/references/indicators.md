# Indicators

Grove's fix for "how do I know what's going on without inspecting everything?" Indicators let a manager see the production line at a glance and catch problems early, at the lowest-value stage.

## Principles

1. **Few and focused.** Pick the 5–6 indicators that matter most for the team's output. More than that becomes noise, and the team stops looking.
2. **Indicators direct attention.** Whatever you measure, people will optimize, so choose deliberately.
3. **Pair every indicator with its counter-indicator.** Measuring one effect invites over-reaction to it, so track the opposite effect as well. Grove's classic pair is inventory levels with shortages. Engineering pairs:

| Indicator | Counter-indicator |
|---|---|
| Deploy frequency | Change failure rate |
| Tickets closed | Reopen rate / escaped defects |
| Time to restore service | Repeat incidents on the same cause |
| On-call pages reduced | Missed real alerts / detection gaps |
| Lead time for new work | Post-release incidents in the first 30 days |
| PR review turnaround | Defects caught after merge |

4. **Prefer leading indicators.** Grove valued indicators that predict trouble before it lands. Engineering examples:

| Leading indicator | What it predicts |
|---|---|
| PRs waiting >2 days for review | Missed delivery dates |
| Error-budget burn rate | An SLO breach |
| Rising alert noise | On-call burnout |
| Upcoming vendor API deprecations | Integration breakage |

5. **Measure output where possible.** For knowledge work, output is hard to count, so use a mix of output measures and a few activity measures, and say which is which.
6. **Show trends, not snapshots.** A single number hides direction. Grove recommended these displays:
   - **Trend charts:** a metric over time, compared against target and against last period.
   - **Stagger charts:** each successive forecast plotted alongside the actuals, so you can see whether forecasts are getting better or keep slipping.

## Using indicators in the management rhythm

- **Staff meeting:** review the indicator set first, then spend discussion time on anything off-trend.
- **1:1s:** the report brings their own indicators, which keeps 1:1s on output rather than activity.
- **Monitoring delegated work:** an indicator going off-trend is the trigger to look closer. That's sampling done well (see SKILL.md, section 4).

## Black-box view

Grove's "black box" frames the team as a box with inputs and outputs: you can't watch everything inside, so you watch the outputs, plus a few internal points that predict them. Choose those internal points at the **limiting step**, the stage that sets throughput. On many engineering teams that's code review, QA or staging, or a single scarce expert.

## Building an indicator set (procedure)

1. **Write the team's output in a sentence.** For example: "reliable integrations that deliver customer data to the detection platform."
2. **Map the flow** from intake to delivery, and mark the limiting step.
3. **Pick 1–2 output indicators**, 1–2 leading indicators at or before the limiting step, and the counter-indicator for each.
4. **For each indicator, record:** a definition, the data source, the owner, the review cadence, and the threshold that triggers action.
5. **Automate collection** wherever possible (CI, ticketing, paging, and observability APIs), so reviewing them costs minutes, not hours.

Output table format:

| Indicator | Type | Definition | Source | Pair | Threshold → action |
|---|---|---|---|---|---|
