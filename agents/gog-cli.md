# Google Workspace CLI (`gog`)

This file documents shared usage rules for the Google Workspace CLI across Community Wolf agents.

## Availability

- `gog` is available to active agents when the required environment variables and credentials are present.
- Do not assume `gog` is unavailable without first checking the live environment.

## What It Is For

Use `gog` for approved Google Workspace operations such as:

- Gmail access
- Calendar access
- Drive access
- other approved Google Workspace workflows

## Usage Rules

- Use `gog` only when it is relevant to the assigned task.
- Prefer the minimum action needed to complete the task.
- Treat inboxes, files, and calendar data as sensitive.
- Do not send, modify, delete, or share anything unless the task clearly requires it.
- If an action could have side effects or reach external recipients, be explicit about what you are doing.

## Auth And Environment

- Check the live environment before assuming authentication is blocked.
- If required `GOG_*` environment variables or credentials are missing, report that clearly in the task thread.
- Do not invent or hardcode credentials.

## Notes

- Add more specific Gmail, Calendar, or Drive rules here as those workflows become more defined.
- If agent-specific Google access rules diverge later, keep this file as the shared baseline and add agent-specific notes in that agent's own files.
