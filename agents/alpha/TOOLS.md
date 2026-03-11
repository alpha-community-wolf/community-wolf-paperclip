# Alpha Tools

## Working Directories

- Home: `$AGENT_HOME`
- Workspace: `$AGENT_HOME/workspace`

## Organizational Scope

- Marketing lead: Corey
- Engineering lead: Tony
- Founder social: Steve
- Engineering execution: Mark

## Default Expectations

- Use Paperclip APIs and task flow for coordination.
- Keep strategic notes and drafts in `$AGENT_HOME`.
- Keep any ad hoc files in `$AGENT_HOME/workspace`.

## External Tools

### `gog` for Google Workspace

- `gog` is installed on this machine and can be used for Google Workspace tasks.
- Use it for Gmail, Calendar, Drive, Docs, Sheets, Slides, Contacts, and related Google services when the work requires reading or sending email, checking calendars, or accessing Drive content.
- Prefer read actions first when gathering context, and be explicit before sending, deleting, or changing anything.
- In the Paperclip agent environment, check auth by testing the real environment first before declaring a blocker.

First checks:

- `which gog`
- `env | rg '^GOG_'`
- `gog auth list`
- `gog whoami`

Common patterns:

- `gog whoami`
- `gog gmail search "from:someone@example.com newer_than:7d"`
- `gog send --to someone@example.com --subject "..." --body "..."`
- `gog calendar events primary`
- `gog calendar create primary --summary "..." --from "2026-03-10T13:00:00" --to "2026-03-10T13:30:00"`
- `gog drive search "quarterly plan"`

Auth notes:

- If access is already configured, `gog` can use the stored Google account credentials on this machine.
- The agent environment may already include `GOG_KEYRING_PASSWORD`. Check `env | rg '^GOG_'` and `gog auth list` before treating keyring access as blocked.
- If `gog auth list` works and shows `alpha@communitywolf.com`, proceed directly with the requested Gmail or Calendar action.
- If `gog` still reports that login, consent, or keyring access is required after those checks, ask the human operator to complete the interactive auth step.
- When multiple Google accounts are present, specify the intended one with `gog -a <email> ...`.

Email workflow:

- For "send an email" tasks, use `gog gmail send` directly rather than stopping after a generic auth check.
- Preferred sender account: `alpha@communitywolf.com`
- Example:
  `gog gmail send -a alpha@communitywolf.com --to michael@communitywolf.com --subject "..." --body "..."`
- After sending, capture the returned `message_id` and include it in the Paperclip issue comment when closing the task.
