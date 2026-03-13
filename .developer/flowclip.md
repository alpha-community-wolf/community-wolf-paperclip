Flow Clip - Continuous flow of work and chat

A fork of the Paperclip project.

## Philosophy

Agents should be lightweight by default. The agent folder is not a knowledge base — it is a bootstrap surface. The only file that gets injected automatically is AGENTS.md; everything else the model reads on demand. Heavy context belongs in skills (installed in `~/.agents/skills/`), not in the agent directory.

## Minimal Agent Structure

```
agents/<name>/
  AGENTS.md        <- sole injected instruction file; self-contained bootstrap
  HEARTBEAT.md     <- heartbeat procedure (model reads on demand)
  opencode.json    <- adapter config
  workspace/       <- where the agent does all its work
```

That's it. Four items per agent.

## What We Cut (and Why)

- **SOUL.md** — Personality and operating-posture guidance. Nice flavour, but costs context window tokens for minimal operational value. The model's behaviour is shaped well enough by AGENTS.md rules and the Paperclip skill.
- **TOOLS.md** — Tool documentation and workflow rules. Replaced by skills injection at runtime and the shared `gog-cli.md` reference. Any critical content (context source paths for marketing agents, git workflow rules for engineering agents) was folded back into AGENTS.md directly.
- **memory/ and life/ directories** — Empty placeholder folders that only held a README. The `para-memory-files` skill creates these on demand when it actually needs them.

## Where the Weight Lives Now

- **AGENTS.md** — Identity, role, rules, Paperclip operations, memory skill reference, heartbeat reference, and any agent-specific context sources. Self-contained enough to bootstrap without reading other files.
- **HEARTBEAT.md** — The wake-loop procedure. Kept separate so it can be updated independently and only loaded when the model needs it.
- **Skills (`~/.agents/skills/`)** — The Paperclip skill carries the full heartbeat protocol and API reference. Other skills handle memory, tool integrations, and domain knowledge. This is where heavyweight context belongs.
- **workspace/** — All working artifacts, cloned repos, drafts, and output. The agent's scratch space.
