# Paperclip OSS PR Backlog

Potential contributions to upstream `paperclip` open source repository.
This file lives outside the repo so we can track ideas safely before turning them into PRs.

## How We Use This

- Add any idea quickly under `Inbox`.
- Move strong candidates into `Prioritized`.
- When we start implementation, move to `In Progress`.
- After merge/opening upstream PR, move to `Done`.

## Prioritized

| ID | Title | Area | Why It Matters | Effort | Status | Notes |
|---|---|---|---|---|---|---|
| PCLIP-001 | Fix project deletion flow (delete in projects does nothing) | UI + API | Core CRUD is blocked; users cannot clean up stale projects | M | Candidate | Repro: deleting a project from projects view is impossible |
| PCLIP-002 | Fix stuck inbox alert badges that cannot be cleared | UI state management | Persistent false alerts erode trust and create noise | M | Candidate | Likely badge state invalidation / event sync issue |
| PCLIP-003 | Make goal deletion reliable in adapter editor | Adapter editor UX | Users cannot easily remove goals during editing workflows | M | Candidate | Verify whether delete action is hidden, disabled, or no-op |
| PCLIP-004 | Preserve workspace and instruction paths on adapter save | Adapter persistence | Saving currently drops configured paths, causing data loss | L-M | Candidate | Regression risk is high; should include save/load roundtrip test coverage |

## Inbox

- [ ] **Per-agent/per-company skill allowlisting** — The platform currently symlinks every skill in `skills/` to every agent unconditionally. Maintainer-focused skills (`create-agent-adapter`, `paperclip-create-agent`, `release`, `release-changelog`) get injected into all company agents, including ones that have no business seeing them. A skill allowlist in `adapterConfig` (or a company-level config) would let operators scope which skills each agent receives.

## In Progress

- [ ] _No active items_

## Done

- [ ] _No completed items yet_

