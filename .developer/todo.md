# Developer TODO List

Active work items for the Paperclip fork. Priority: **Now** = this sprint, **Backlog** = future.

## Completed (this cycle)

- [x] **Skills management system** — three-tier skill access (built-in core/optional, company, agent-local), Skills page, agent config section, CLI install, run visibility. Merged to `master`.
- [x] **Sidebar badges / inbox dismissals / project improvements** — persistent server-side dismissals, stale badge fixes, project color/archive/delete, adapter config field preservation. Merged to `master`.
- [x] **Portable agent paths** — `AGENT_HOME` auto-injected by all adapters from `cwd`. All AGENTS.md files updated to use `$AGENT_HOME`. Cursor rule added.
- [x] **Claude Code permissions** — `.claude/settings.json` with pre-approved tools for autonomous agent execution.
- [x] **Migration cleanup** — resolved duplicate table creation from cross-branch drizzle-kit generation. Cursor rule added.

---

## Now

### 1. Direct chat with an agent

**Priority:** Now — foundational feature for user-agent interaction.

**Status:** Not implemented. No chat routes, pages, or components exist.

**Notes:** The adapter outputs `stream-json` — the chat service needs to either poll the run transcript or pipe the adapter's stdout via SSE/WebSocket to the browser. The agent is woken with the user's message as the prompt, and the run output becomes the reply.

- [ ] Add a `Chat` tab to `AgentDetail.tsx` (alongside Dashboard, Configuration, Runs)
- [ ] Add route `/:companyPrefix/agents/:agentId/chat` in `ui/src/App.tsx`
- [ ] Create `server/src/routes/chat.ts` with endpoints:
  - `POST /agents/:agentId/chat/messages` — send a message (triggers agent run)
  - `GET /agents/:agentId/chat/messages` — list message history
  - `GET /agents/:agentId/chat/messages/:messageId/stream` — SSE stream for the agent's response
- [ ] Create `server/src/services/chat.ts` — persist messages, invoke the agent with `wakeReason: "chat_message"`, stream or poll the run output back
- [ ] Add `chat_messages` table to `packages/db/src/schema/` with columns: id, agent_id, company_id, role (user/assistant), content, run_id (nullable), created_at
- [ ] Generate migration: `pnpm db:generate`
- [ ] Add shared types/validators in `packages/shared/src/types/chat.ts` and `packages/shared/src/validators/chat.ts`
- [ ] Build UI: message list + input box, display streamed agent response in real-time
- [ ] Add `PAPERCLIP_CHAT_MESSAGE_ID` env var so the agent knows it was woken by a chat message

### 2. Preserve current tab when switching between agents

**Priority:** Now — quick UI fix, improves navigation.

**Status:** Routes encode tabs (`/agents/:agentId/dashboard`, `/configuration`, `/runs`). Navigation links always reset to the root instead of preserving the active tab.

- [ ] Audit `ui/src/components/Sidebar.tsx` and agent list components — find where links to agent detail pages are built
- [ ] When navigating to a different agent, replace only the `:agentId` segment while keeping the rest (tab + sub-path) intact. Use `useLocation` + `useNavigate` from React Router.
- [ ] If the target agent doesn't support the current tab, fall back to `/dashboard`

### 3. Global env file for agents

**Priority:** Now — important for CLI tools (gog, etc.) that need shared credentials across all agents.

**Status:** Agent env vars are stored per-agent in `adapterConfig.env` (jsonb). No file-based global env exists. `AGENT_HOME` is now auto-injected.

**Notes:** The key use case is shared credentials (e.g. `GOG_KEYRING_PASSWORD`) that every agent needs for CLI tools. Currently these are duplicated in each agent's env config. A single `.env` file that all agents inherit from would eliminate that duplication.

- [ ] Add a `globalEnvFile` field to the Paperclip instance config (`packages/shared/src/` config types)
- [ ] In `cli/`, add `pnpm paperclipai configure --section env` to set the global env file path
- [ ] In adapter execution (each `packages/adapters/*/src/server/execute.ts`), load and parse the global env file before merging per-agent `config.env` — precedence: global file < per-agent config < auto-injected vars (AGENT_HOME, PAPERCLIP_*)
- [ ] Add validation in `pnpm paperclipai doctor` to warn if the configured global env file path doesn't exist
- [ ] Support `.env` format (KEY=VALUE, comments with #, no export prefix)

---

## Backlog

### 4. Plan mode for agents

**Priority:** Backlog — useful but not blocking current workflows.

**Status:** Plan mode exists only in the Cursor adapter (`normalizeMode()` in cursor-local). Other adapters have no plan mode concept.

**Notes:** Claude Code doesn't have a `--plan` flag — plan mode would be injected as a prompt prefix (e.g. "You are in plan mode. Analyze and propose a plan. Do not make changes.") rather than a CLI flag.

- [ ] Add `mode?: "plan" | "act"` field to shared adapter types in `packages/adapter-utils/src/types.ts`
- [ ] Claude adapter: inject plan mode as a prompt prefix when `config.mode === "plan"`
- [ ] Codex/OpenCode adapters: same pattern
- [ ] UI: add "Mode" dropdown (Plan / Act) to `AgentConfigForm.tsx`
- [ ] On-demand invocation: allow passing `mode: "plan"` from the Run button without changing agent defaults
- [ ] Set `PAPERCLIP_MODE=plan` in execution env so agents can introspect it

### 5. Agent email addresses

**Priority:** Backlog — ambitious, split into phases.

**Status:** No email/mail for agents. `agents` DB schema has no email field.

**Phase 1 — DB + display:**
- [ ] Add `email` column to `agents` table (nullable, unique)
- [ ] Auto-generate email like `<shortname>@<company-domain>` on agent creation
- [ ] Display email in `AgentDetail.tsx` agent info header

**Phase 2 — Outbound:**
- [ ] Add `mailConfig` section to instance config (SMTP host/port/credentials)
- [ ] `pnpm paperclipai configure --section mail` in CLI
- [ ] `server/src/services/mail.ts` — agents call via Paperclip API to send email

**Phase 3 — Inbound:**
- [ ] Webhook route to receive inbound emails from provider
- [ ] Route to target agent as inbox item or wake trigger
