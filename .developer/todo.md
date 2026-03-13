# Developer TODO List

Derived from `note.txt`. Each item includes codebase context so an agent can start working immediately.

---

## 1. Direct chat with an agent

**Status:** Not implemented. No chat routes, pages, or components exist.

- [ ] Add a `Chat` tab to `AgentDetail.tsx` (alongside Dashboard, Configuration, Runs)
- [ ] Add route `/agents/:agentId/chat` in `ui/src/App.tsx`
- [ ] Create `server/src/routes/chat.ts` with `POST /agents/:agentId/chat/messages` endpoint
- [ ] Create `server/src/services/chat.ts` — persist messages, invoke the agent with the user's message as a wake reason, stream or poll the run output back
- [ ] Add `chat_messages` table to `packages/db/src/schema/` with columns: id, agent_id, company_id, role (user/assistant), content, run_id (nullable), created_at
- [ ] Generate migration: `pnpm db:generate`
- [ ] Add shared types/validators in `packages/shared/src/types/chat.ts` and `packages/shared/src/validators/chat.ts`
- [ ] Build UI: message list + input box, poll or stream run output to display as the agent's reply

---

## 2. Brainstorming chat that produces a project with tasks and a plan

**Status:** No chat feature exists. Projects/issues exist in DB but no "chat-to-plan" flow.

- [ ] Extend the chat feature (TODO #1) with a "brainstorm" mode that collects free-form goals
- [ ] After brainstorm session ends, add a "Create project from chat" action in the UI
- [ ] Server endpoint: `POST /companies/:companyId/projects/from-chat` — accepts a `chatId` or message transcript, uses an LLM call to extract project name, description, and a list of issue titles/descriptions
- [ ] Auto-create the project and issues, then redirect the user to the new project page in `ProjectDetail.tsx`
- [ ] The generated plan should appear as issues with `status: "todo"` in the project's issue list

---

## 3. Skills must be CLI commands controlled by env files loaded to agents

**Status:** Skills are discovered from filesystem directories containing `SKILL.md` files. Install runs a command via `POST /companies/:companyId/skills/install`. No env file loading per skill.

- [ ] **Skills as CLI commands:** Enforce that each skill's `SKILL.md` frontmatter includes a `command` field (the CLI command agents use to invoke it). Validate this in `server/src/services/skills.ts` during seeding/discovery.
- [ ] **Per-skill env file support:** Add an optional `env_file` field to the `skills` DB schema (`packages/db/src/schema/skills.ts`). When resolving skills for execution in `skillService.resolveForExecution()`, load the referenced `.env` file and merge its vars into the agent's execution environment.
- [ ] **Global env files for skills:** Add a `globalSkillEnvFile` field to company or instance config (check `packages/shared/src/types/` for the config shape). In `server/src/services/heartbeat.ts` (where env is assembled before agent runs), load this file and merge it before per-agent env vars.
- [ ] Update `AgentSkillsSection.tsx` and `ui/src/pages/Skills.tsx` to display the `command` field and env file path per skill.
- [ ] Generate migration for any schema changes: `pnpm db:generate`

---

## 4. Global env files for agents

**Status:** Agent env vars are stored per-agent in `adapterConfig.env` (jsonb). No file-based global env exists.

- [ ] Add a `globalEnvFile` field to the Paperclip instance config (`packages/shared/src/` config types)
- [ ] In `cli/`, add `pnpm paperclipai configure --section env` to set the global env file path
- [ ] In adapter execution (each `packages/adapters/*/src/server/execute.ts`), load and parse the global env file before merging per-agent `config.env` — global file is lowest precedence, per-agent config overrides it
- [ ] Add validation in `pnpm paperclipai doctor` to warn if the configured global env file path doesn't exist

---

## 5. Plan mode: agents can be invoked in plan mode, injected into commands at inference time

**Status:** Plan mode exists only in the Cursor adapter (`packages/adapters/cursor-local/src/server/execute.ts`, `normalizeMode()` at line 65). Other adapters (claude-local, codex-local, etc.) have no plan mode concept.

- [ ] **Standardize plan mode in adapter-utils:** Add a `mode?: "plan" | "act"` field to the shared adapter execution types in `packages/adapter-utils/src/types.ts`
- [ ] **Claude adapter plan mode:** In `packages/adapters/claude-local/src/server/execute.ts`, check `config.mode === "plan"` and append `--plan` flag (or equivalent prompt prefix) to the Claude CLI invocation
- [ ] **Codex adapter plan mode:** Same pattern in `packages/adapters/codex-local/src/server/execute.ts`
- [ ] **UI:** Add a "Mode" dropdown (Plan / Act) to `AgentConfigForm.tsx` that maps to the `mode` field in `adapterConfig`
- [ ] **On-demand invocation:** When invoking an agent from the UI (e.g., from AgentDetail dashboard "Run" button), allow passing `mode: "plan"` so a single run can be in plan mode without changing the agent's default config
- [ ] **Wake reason injection:** In `server/src/services/heartbeat.ts`, when mode is `plan`, set `PAPERCLIP_MODE=plan` in the execution env so agents can introspect it in their skill scripts

---

## 6. Preserve current tab when switching between agents

**Status:** Routes already encode tabs (`/agents/:agentId/dashboard`, `/agents/:agentId/configuration`, `/agents/:agentId/runs`). The issue is likely in how navigation links in the sidebar or agent list are constructed — they may always link to the root `/agents/:agentId` instead of preserving the active tab.

- [ ] Audit `ui/src/components/Sidebar.tsx` and any agent list components — find where links to agent detail pages are built
- [ ] When navigating to a different agent, replace only the `:agentId` segment of the current path while keeping the rest (tab + sub-path) intact. Use `generatePath` from React Router or string manipulation on `location.pathname`.
- [ ] If the target agent doesn't support the current tab (e.g., switching to an agent with no runs), fall back to `/dashboard`

---

## 7. Remove agent directory clutter — make `agent.md` the core agent document

**Status:** No system-wide "agent directory" feature exists. The claude-local adapter has an `instructionsFilePath` config field that loads a markdown file as a system prompt prefix. This is per-adapter and per-agent — not standardized.

- [ ] **Standardize `agent.md` as first-class:** Add an `instructionsFile` field to the shared adapter config types in `packages/adapter-utils/src/types.ts`, defaulting to `agent.md` relative to the agent's workspace
- [ ] **All adapters read it:** Update each `packages/adapters/*/src/server/execute.ts` to load `agent.md` from the agent workspace and inject it as a system prompt prefix (claude-local already has `instructionsFilePath` — generalize this pattern)
- [ ] **UI simplification:** In `AgentConfigForm.tsx`, surface a single "Agent Instructions" textarea that edits `agent.md` directly (instead of the current per-adapter `instructionsFilePath` config field)
- [ ] **Remove stale per-adapter fields:** Once `agent.md` is the standard, deprecate the per-adapter `instructionsFilePath` config field in claude-local's config fields UI (`ui/src/adapters/claude-local/config-fields.tsx`)

---

## 8. Give all agents an email address — configure mail account

**Status:** No email/mail for agents. `agents` DB schema has no email field. Auth schema has email for users only.

- [ ] Add `email` column to the `agents` table in `packages/db/src/schema/agents.ts` (nullable, unique)
- [ ] Generate migration: `pnpm db:generate`
- [ ] **Auto-assign emails on agent creation:** In `server/src/services/` agent creation logic, auto-generate an email like `<agent-shortname>@<company-domain>` if a company domain is configured
- [ ] **Mail configuration:** Add a `mailConfig` section to instance config (SMTP host/port/credentials). Add `pnpm paperclipai configure --section mail` in the CLI.
- [ ] **Inbound mail routing:** Add a route or service that receives inbound emails (via webhook from an email provider) and creates an inbox item or wake request for the target agent
- [ ] **Outbound mail:** In `server/src/services/`, add a `mailService.ts` that agents can call (via the Paperclip skill/API) to send email on behalf of their address
- [ ] Display the agent's email address in `AgentDetail.tsx` (Dashboard tab, in the agent info header)
