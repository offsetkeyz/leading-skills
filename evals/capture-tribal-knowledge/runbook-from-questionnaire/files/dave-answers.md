## Nightly warehouse load — knowledge capture

**1. Describe the last 3 times this went wrong.**

a) Two weeks ago. Got paged at 3am, `nightly_wh_load` DAG in Airflow red on task `load_orders`. Log said `psycopg2.errors.LockNotAvailable: could not obtain lock on relation "orders_staging"`. That's almost always someone's ad-hoc query from the analytics team holding a lock. I ran `SELECT pid, usename, query_start, query FROM pg_stat_activity WHERE query ILIKE '%orders_staging%';` on the warehouse, saw a 4-hour-old query from a Looker service account, killed it with `SELECT pg_terminate_backend(<pid>);`, cleared the task in Airflow, it went green in ~12 min.

b) Last month. `extract_shopify` failed with HTTP 429. Shopify rate limiting. Don't do anything, it retries 3 times with backoff and usually recovers by itself. If it fails all 3 retries, wait 30 min and clear the task. Don't clear it immediately — you just eat more rate limit.

c) In March. Load "succeeded" but row counts in `fct_orders` were like 40% of normal. Turned out the upstream S3 export from the payments vendor was partial — the file landed but was truncated. There's no check for this. I only caught it because the finance dashboard looked weird. Fix was rerunning `extract_payments` with `--date` for that day after vendor re-sent the file. You have to email the vendor, their support is payments-support@vendor.example, takes about a day.

**2. Paste the exact error from one of them.**
```
psycopg2.errors.LockNotAvailable: could not obtain lock on relation "orders_staging"
```

**3. Steps as you actually do them.**
Check Airflow UI, find red task, read the log, figure out which of the 3 it is, fix, clear task (NOT the whole DAG run — clearing the DAG reruns extract which re-pulls from Shopify and burns rate limit).

**4. Which step would a newcomer get stuck on?**
Figuring out it's a lock. The error doesn't always say LockNotAvailable — sometimes it just times out after 30 min with `canceling statement due to statement timeout`. Same cause usually.

**5. What do you check before starting?**
That the warehouse isn't in its Sunday maintenance window (Sun 02:00–04:00 UTC). If it is, everything fails, just wait.

**6. Scripts.**
`scripts/backfill.py --date YYYY-MM-DD --task <task_id>` — reruns one task for one day. Always pass `--task`, otherwise it backfills every task for that day including the Shopify extract. I think it's idempotent for load tasks, not 100% sure for `extract_payments`.

**7. Loud or silent failures?**
Locks and 429s are loud. Partial upstream files are silent — that's the scary one.

**8. Quick vs long?**
If it's a lock, 15 min. If it's upstream data, could be a day+.

**9. Shortcut?**
I kill Looker queries without asking. It's fine because Looker just retries. I would NOT kill a query from a human user account — ask in #analytics first.

**10. Weirdest case?**
Once DST changed and the DAG ran twice. Didn't break anything because loads are upserts.

**11. What did you get wrong when new?**
Cleared the whole DAG run. Got Shopify rate limited for 6 hours.

**12. Never do?**
Never `TRUNCATE` staging tables manually. The load depends on staging rows from previous day for dedup.

**13. Stop and escalate?**
Row counts way off with no error → that's upstream data, escalate to data eng lead, don't try to fix by rerunning. Anything involving the payments vendor.

**14. What didn't I ask?**
There's a Slack alert in #data-alerts for row count drops but it's been broken since the migration, so don't trust that it'd fire.
