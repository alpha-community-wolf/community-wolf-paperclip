# Mark Tools

## Working Directories

- Home: `$AGENT_HOME`
- Workspace: `$AGENT_HOME/workspace`

## Repository Layout

Clone each engineering repository into this folder. Recommended layout:

- `$AGENT_HOME/workspace/frontend`
- `$AGENT_HOME/workspace/backend`
- `$AGENT_HOME/workspace/shared`
- `$AGENT_HOME/workspace/infra`
- Unless explicitly told otherwise, clone repositories into `$AGENT_HOME/workspace/<repo-name>`.

## Git Workflow Rules

- You may create and switch to new working branches freely.
- Never push directly to `main`.
- Never push directly to `staging`.
- Do not merge into `main` or `staging` yourself unless the user explicitly instructs you to do so.
- The default allowed delivery path is: create a branch, push that branch, and open a pull request into `staging`.

## Commit Best Practices

- Commit on meaningful progress, not after every tiny edit and not only at the very end.
- Prefer one logical change per commit.
- Keep commits reviewable and scoped.
- Before committing, review what changed and make sure the diff matches the task.
- Do not include unrelated files or cleanup in the same commit unless it is directly required.
- Use clear commit messages that explain the intent of the change.
- Good pattern: `<type>: <short purpose>`, for example `feat: add account settings form` or `fix: handle missing auth token`.
- If a task spans multiple distinct improvements, split them into separate commits when practical.
- If tests or checks exist for the changed area, run the relevant ones before committing when possible.
- Do not rewrite shared history or force-push unless the user explicitly instructs you to do so.

## Pull Request Best Practices

- Open pull requests into `staging` by default.
- Keep each pull request focused on one task, bugfix, or coherent improvement.
- Do not mix unrelated changes in the same pull request.
- Use a clear pull request title that matches the real outcome of the work.
- In the pull request description, summarize what changed, why it changed, and any important tradeoffs or follow-up work.
- Include a short test note describing what was verified, or clearly say what was not tested.
- If the change affects multiple repositories or systems, explain the end-to-end impact briefly.
- Raise blockers, risks, or migration steps clearly instead of hiding them.
- Do not merge the pull request yourself unless the user explicitly instructs you to do so.

## Default Expectations

- Work from this root when implementation spans more than one repository.
- Follow each repo's local instructions and tooling.
- Escalate cross-repo design questions to Tony rather than guessing.
