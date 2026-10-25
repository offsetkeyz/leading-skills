# Performance reviews (Grove)

Grove calls the performance review possibly the single most important form of task-relevant feedback a manager gives. Its purpose is to **improve the subordinate's performance**. It is not a verdict, and it is not a compensation document, even though it often feeds one.

Two separate jobs: **assessing** performance, and **delivering** the assessment.

## Assessing

- **Assess output first, then the activities behind it.** Ask what the person produced and what their team or area produced, then ask which internal measures (for example, how they handle design, mentoring, and on-call) explain those results. Knowledge work needs both, because output alone can lag or reflect luck, and activity alone rewards busyness.
- **Separate performance from potential.** Rate what was done in this period. Grove warned against grading on potential, credentials, or effort ("tries hard").
- **Account for the time lag.** Results this period may reflect last period's work, and a new person's results may not have shown up yet.
- **Watch for the halo effect,** where one strong or weak trait colors the whole review.
- **Don't save it up.** If something in the review is news to the person, the manager failed at earlier feedback. The review should consolidate, not surprise.
- **Pick a few messages.** Grove's advice is that a review can't fix everything. Choose the few points that will most raise this person's output, and make those land.
- **Don't neglect stars.** Grove noted managers spend review effort on low performers, but the highest return on improvement often comes from raising strong performers further.

## Delivering

**The three L's:**

- **Level with them.** Be direct and honest, without euphemism.
- **Listen.** Make sure the message is actually received, with its weight, not just heard. "Listen" means using all your senses to confirm the person has absorbed it.
- **Leave yourself out.** The meeting is about them and their performance, not your anxiety about the conversation.

**Mechanics:**

- **Send the written review shortly before the meeting,** so the person can absorb it privately and come ready to discuss rather than react. Grove recommended this.
- **Plan the conversation in advance:** the few messages, the evidence for each, and what you want them to do differently.
- **Agree on specific next steps** and check on them in later 1:1s.

### When the message is "performance is not acceptable"

Grove describes stages a person may move through on hearing a problem:

1. Ignoring it
2. Denying it ("that's not a problem")
3. Blaming others or circumstances
4. Assuming responsibility ("it is my problem")
5. Finding a solution

The manager's goal for the meeting is to get to **stage 4, assumption of responsibility**. If the person gets there, the review succeeded, even if the solution takes more work. Don't skip ahead to solutions while they're still in denial or blame, because the solution won't stick.

## Review draft template

```markdown
## Performance review: [Name], [Role], [Period]

### Summary (2–3 sentences)
[Overall assessment rooted in output this period.]

### Output this period
- [Delivered result + measurable impact]
- [Delivered result + measurable impact]
- [Team/area output they influenced]

### How they got there (activities / behaviors)
- Strengths: [specific, with an example]
- Areas limiting their output: [specific, with an example and its effect]

### The few things that matter most next period
1. [Message 1 → what "better" looks like, how we'll know]
2. [Message 2 → …]

### Support from me
- [What the manager will do: monitoring cadence, coaching, scope changes]
```

Placeholders stay placeholders when data is missing. Never invent results for a real person.

## Worked example (engineering)

**Context:** Jessica is a senior SRE. This period she led the migration to a new alerting stack on time, which cut pages by 40%. Her design reviews are thorough. However, she rewrote two junior engineers' PRs herself instead of coaching them, and one junior has stopped asking her questions.

**Assessment:**

- **Output is strong.** The migration landed, and pages dropped 40%.
- **The limiting behavior** is that she meddles in juniors' work. That's negative leverage: her personal output goes up while the team's capability stalls, and at her level the team's capability is part of her output.
- **Few messages:**
  1. Keep the reliability results; they're the bar.
  2. Shift from fixing to coaching. Success looks like the juniors' PRs passing her review on the second pass, because they made the changes and she didn't.

**Delivery plan:**

- Send the written review the day before.
- In the meeting, level with her: "The migration is the best result on the team this half. The thing holding back your impact is how you handle junior PRs."
- Listen for denial ("it was faster") versus ownership.
- Close on one concrete practice: comment on the PR and don't push commits to it. Review progress in the next three 1:1s.
