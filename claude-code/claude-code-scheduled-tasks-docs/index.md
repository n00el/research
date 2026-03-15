# Run Prompts on a Schedule — Claude Code Scheduled Tasks

**Author:** Anthropic (Claude Code documentation)
**Date:** 2026
**Source:** https://code.claude.com/docs/en/scheduled-tasks

---

The page documents how to schedule recurring or one-time prompts within a Claude Code session using the `/loop` command and cron-based scheduling tools. These scheduled tasks can be used to poll deployments, monitor PRs, check build statuses, or set reminders -- all within a running Claude Code session.

## Session-Scoped Tasks

Scheduled tasks live only within the current Claude Code process. They are **lost when the session exits**. For durable scheduling that survives restarts, the docs point to **Desktop scheduled tasks** or **GitHub Actions**.

## `/loop` Command (Recurring Prompts)

The primary interface for scheduling recurring prompts.

- Syntax: `/loop [interval] <prompt>` or `/loop <prompt> every <interval>`
- **Default interval** if none specified: every 10 minutes.
- Supported time units: `s` (seconds), `m` (minutes), `h` (hours), `d` (days).
- Seconds are rounded up to the nearest minute (cron has 1-minute granularity).
- Non-clean intervals (e.g., `7m`, `90m`) are rounded to the nearest clean value.
- Can loop over other commands/skills, e.g., `/loop 20m /review-pr 1234`.

## One-Time Reminders

Described in natural language (no `/loop` needed).

Examples:
- `remind me at 3pm to push the release branch`
- `in 45 minutes, check whether the integration tests passed`

Claude schedules a single-fire cron task that auto-deletes after execution.

## Managing Tasks

Natural language queries work: `what scheduled tasks do I have?` or `cancel the deploy check job`.

Underlying tools:
- **`CronCreate`** -- Schedule a new task (accepts 5-field cron expression, prompt, recurrence flag).
- **`CronList`** -- List all scheduled tasks with IDs, schedules, and prompts.
- **`CronDelete`** -- Cancel a task by its 8-character ID.

Maximum of **50 scheduled tasks** per session.

## Execution Behavior

- The scheduler checks every second for due tasks and enqueues them at **low priority**.
- Tasks fire **between user turns**, never while Claude is mid-response.
- If Claude is busy when a task is due, the prompt waits until the current turn ends.
- All times are interpreted in **local timezone**, not UTC.

## Jitter

To avoid API thundering-herd problems, a deterministic offset is added to fire times:

- **Recurring tasks:** up to 10% of their period late, capped at 15 minutes (e.g., an hourly job may fire between `:00` and `:06`).
- **One-shot tasks:** scheduled for top/bottom of the hour may fire up to 90 seconds early.
- The offset is derived from the task ID (deterministic per task).
- Tip: pick a non-round minute (e.g., `3 9 * * *` instead of `0 9 * * *`) to avoid one-shot jitter.

## Three-Day Expiry

Recurring tasks **automatically expire after 3 days**. They fire one final time, then self-delete. To extend beyond 3 days: cancel and recreate the task, or use Desktop scheduled tasks for durable scheduling.

## Cron Expression Reference

Standard 5-field format: `minute hour day-of-month month day-of-week`.

- Supports wildcards (`*`), single values, steps (`*/15`), ranges (`1-5`), and comma-separated lists.
- Day-of-week: `0` or `7` = Sunday, through `6` = Saturday.
- Extended syntax (`L`, `W`, `?`, name aliases like `MON`/`JAN`) is **not supported**.
- When both day-of-month and day-of-week are set, either matching triggers the task (vixie-cron semantics).

## Disabling the Scheduler

Set the environment variable `CLAUDE_CODE_DISABLE_CRON=1` to disable all scheduling. This makes the cron tools and `/loop` unavailable and stops any already-scheduled tasks.

## Limitations

- Tasks only fire while Claude Code is running and idle.
- **No catch-up** for missed fires -- if a scheduled time passes while Claude is busy, it fires once when idle, not once per missed interval.
- **No persistence** across restarts.
- For unattended cron-driven automation, the docs recommend GitHub Actions with a `schedule` trigger or Desktop scheduled tasks.
