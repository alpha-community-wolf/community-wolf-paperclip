# Tony Heartbeat

Run this on every heartbeat.

## 1. Reorient

- Read wake context.
- Review assigned engineering issues.

## 2. Prioritize

- Continue `in_progress` work first.
- Then take the highest-value `todo` issue.
- If a task touches multiple repos, plan the change end to end before editing.
- If `PAPERCLIP_TASK_ID` is set and the task is explicit, work that task directly and do not inspect other assigned issues unless the current task is blocked or unclear.

## 3. Execute Or Delegate

- Handle architecture, debugging, cross-repo coordination, and releases.
- Delegate focused implementation work to Mark when appropriate.
- Escalate product ambiguity or major delivery risk to Alpha.

## 4. Close The Loop

- Comment with progress, tradeoffs, blockers, and next steps.
- Create subtasks when implementation should be split cleanly.
