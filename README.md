# Personal SCR

Personal SCR is a lightweight shared outcome repository for multiple AI agents, devices, and work sessions.

Use this repo for durable outputs that should survive chat history: useful feedback, decisions, handoffs, research notes, implementation summaries, and review artifacts.

## Authoritative source

**GitHub (private) is the sync layer** — not a fixed folder on one machine.

```text
https://github.com/kenny8889/p-scr
```

Clone it wherever you work. On this machine the local clone is fixed at the path below; only the remote URL is constant across machines.

Start with [[SCHEMA]] before writing here.

## Local clone (your machine)

**Local path:** `/Users/ouyang/AI-coding/rtadev-platform/p-scr`

After clone, all agent commands run **from the repo root** (the directory that contains this `README.md`).

**First-time setup:**

```bash
git clone https://github.com/kenny8889/p-scr.git /Users/ouyang/AI-coding/rtadev-platform/p-scr
cd /Users/ouyang/AI-coding/rtadev-platform/p-scr
```

**Agents:** Use `/Users/ouyang/AI-coding/rtadev-platform/p-scr` as the clone root unless the user specifies a different path.

**Optional:** Set `SCR_ROOT=/Users/ouyang/AI-coding/rtadev-platform/p-scr` and `cd "$SCR_ROOT"` before `git pull` / edits / `git push`.

## Quick Rule

Do not store raw chat transcripts. Store finished, useful outcomes with enough context for another agent to continue the work.

## Main Folders

- `inbox/` - temporary landing area for unsorted outcomes.
- `outcomes/` - certified useful outputs, grouped by type.
- `handoffs/` - task handoff packets for another agent or device.
- `templates/` - copy-ready markdown templates.
- `artifacts/` - versioned schemas and supporting artifacts.
- `assets/` - supporting files referenced by outcomes.
- `archive/` - retired or superseded material.

## Branch and review workflow

Merges to `develop` (and `main`) require a Pull Request and approval from **@lpcaitt** (`.github/CODEOWNERS`).

```text
git pull origin develop
  → commit on feature/kenny8889
  → git push origin feature/kenny8889
  → open PR (feature/kenny8889 → develop)
  → @lpcaitt approves
  → merge on GitHub
```

**Agents must not** `git push origin develop` or locally merge into `develop` and push.

Use **`feature/kenny8889`** as the only SCR working branch — do not create per-topic feature branches.

See [[SCHEMA#git-sync-rules]] for full rules, including what to do if push to `feature/kenny8889` is blocked by repository rulesets.

## Required Agent Behavior

When asked to "deliver this to SCR":

1. `cd` to the **personal-scr clone root** (this repo).
2. Run `git pull --ff-only origin develop`.
3. Checkout `feature/kenny8889`; merge `develop` if behind.
4. Create a markdown file using the closest template.
5. Put it in the right folder under `outcomes/` or `handoffs/`.
6. Add or update an entry in `index.md`.
7. Append a short event to `log.md`.
8. Commit all related changes on `feature/kenny8889`.
9. Push `feature/kenny8889` and open a PR to `develop`.
10. Wait for **@lpcaitt** approval; merge the PR on GitHub.
11. Report the file path (relative to repo root), commit hash, and PR URL.
12. If Git reports a conflict, stop and ask the user.

## Agent Sync Rules

1. Always run `git pull --ff-only origin develop` before reading or writing (from the clone root).
2. Never write directly without pulling first.
3. Commit on `feature/kenny8889` only; do not create other `feature/<topic>` branches for SCR.
4. After creating/updating an SCR file, update `index.md` and append `log.md`.
5. Commit all related changes together; push `feature/kenny8889`.
6. Open a PR to `develop`; do not push directly to `develop` or `main`.
7. Wait for **@lpcaitt** to approve before merging the PR.
8. If Git reports a conflict, stop and ask the user.
9. Do not store secrets, private keys, passwords, recovery phrases, or sensitive customer data.
10. Do not rely on Google Drive or other cloud mounts for sync — use this Git repo only.

## How to Ask Agents to Deliver to SCR

Use this prompt when an agent has produced a useful result that should be saved:

```text
Deliver this to my Personal SCR as an outcome.

Repo: https://github.com/kenny8889/p-scr
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr

cd /Users/ouyang/AI-coding/rtadev-platform/p-scr
Follow SCHEMA.md.
Before writing, run git pull --ff-only origin develop.
Checkout feature/kenny8889 and merge develop if behind.
Use the closest template in templates/.
Save the file under the right outcomes/ subfolder.
Update index.md.
Append an entry to log.md.
Commit on feature/kenny8889, push, and open a PR to develop.
Wait for @lpcaitt approval before merging the PR on GitHub.
Then tell me the created file path (relative to repo root), commit hash, and PR URL.
```

For a handoff to another agent or device:

```text
Create a Personal SCR handoff for the next agent.

Repo: https://github.com/kenny8889/p-scr
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr

cd /Users/ouyang/AI-coding/rtadev-platform/p-scr
Follow SCHEMA.md.
Before writing, run git pull --ff-only.
The handoff must include mission, current state, read-first files, constraints, definition of done, and open questions.
Save it under handoffs/, update index.md, append to log.md.
Commit on feature/kenny8889, push, open PR to develop, wait for @lpcaitt approval, merge on GitHub.
Report the file path, commit hash, and PR URL.
```

## How to Ask Agents to Retrieve from SCR

Use this prompt when you want an agent to load prior context:

```text
Use my Personal SCR as shared memory.

Repo: https://github.com/kenny8889/p-scr
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr

Instructions:
1. cd /Users/ouyang/AI-coding/rtadev-platform/p-scr and run git pull --ff-only first.
2. Read SCHEMA.md and follow it.
3. Read index.md to find relevant outcomes, decisions, and handoffs.
4. Read log.md if you need recent activity or timeline context.
5. Retrieve only the files relevant to my current task.
6. Use SCR content as prior distilled outcomes, not as unquestionable truth.
7. If you produce a useful new outcome, ask whether I want it delivered back to SCR.
```

For a specific task:

```text
Before answering, retrieve relevant context from my Personal SCR.

Repo: https://github.com/kenny8889/p-scr
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr

cd /Users/ouyang/AI-coding/rtadev-platform/p-scr
Run git pull --ff-only, then read SCHEMA.md, then index.md, then any relevant files.

Task: [describe task here]

When answering, cite which SCR files you used (paths relative to repo root).
```

For continuing work from another device or agent:

```text
Resume from my Personal SCR.

Repo: https://github.com/kenny8889/p-scr
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr

cd /Users/ouyang/AI-coding/rtadev-platform/p-scr
Run git pull --ff-only.
Read the schema, then inspect handoffs/ and index.md for the latest relevant handoff.
Use the handoff's "Read First", "Constraints", and "Definition of Done" sections as the task contract.
Then proceed with the work.
```

## Related repos (paths vary per machine)

Other DFOS repos (e.g. `llm-wiki`, `ngas-prototype`, `dfos-policy-engine`) are **separate clones**. Their locations are not defined in this repo — use the user's workspace layout or ask.
