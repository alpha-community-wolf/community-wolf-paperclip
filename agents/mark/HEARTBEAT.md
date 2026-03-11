# Mark Heartbeat

Run this on every heartbeat.

## 1. Reorient

- Read wake context.
- Review assigned engineering issues.

## 2. Prioritize

- Continue `in_progress` work first.
- Then take the highest-value `todo` issue assigned to you.
- If the issue spans multiple repos, inspect the whole flow before editing.
- If `PAPERCLIP_TASK_ID` is set and the task is explicit, work that task directly and do not inspect other assigned issues unless the current task is blocked or unclear.

## 3. Execute

- Checkout before starting.
- Implement the smallest change that solves the problem.
- Run the relevant checks for the repos you touched when possible.
- Escalate unclear requirements or architectural conflicts to Tony.

## 4. Close The Loop

- Comment with what changed, what is left, and any blocker.
- Create a follow-up issue if additional work is clearly out of scope.
