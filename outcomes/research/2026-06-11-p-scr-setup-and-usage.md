---
type: outcome
status: active
created: 2026-06-11
updated: 2026-06-11
agent: "Cursor Composer"
project: "p-scr"
tags: [setup, documentation, workflow]
sensitivity: internal
source: "Cursor chat — Personal SCR configuration session"
---

# Personal SCR Setup and Pre-Commit Workflow

## Context

The `p-scr` repository was initialized with `README.md` and `SCHEMA.md` but lacked the full directory structure, index, log, and templates. The user configured this repo as their Personal SCR for cross-agent shared memory and needed documented repo URL, local clone path, and a repeatable prompt for saving outcomes before committing code repos.

## Outcome

Personal SCR is configured for this machine with:

| Item | Value |
|------|-------|
| Remote repo | `https://github.com/kenny8889/p-scr` |
| Local clone | `/Users/ouyang/AI-coding/rtadev-platform/p-scr` |
| Optional env | `SCR_ROOT=/Users/ouyang/AI-coding/rtadev-platform/p-scr` |

**Document roles:**

- `README.md` — usage guide: clone setup, folder purposes, agent sync rules, copy-paste prompts
- `SCHEMA.md` — format spec: frontmatter, page types, naming, index/log rules, git workflow

**Pre-commit workflow (code repo → SCR first):**

1. Ask the agent to deliver a distilled outcome to SCR (not raw chat).
2. Agent runs `git pull --ff-only` in the SCR clone.
3. Agent creates a markdown file per `SCHEMA.md` under `outcomes/` (or `handoffs/`).
4. Agent updates `index.md` and appends `log.md`.
5. Agent commits and pushes SCR.
6. Then commit/push the code repository.

**Minimal prompt (one-liner):**

```text
Deliver this to my Personal SCR as an outcome before I commit the code repo.

Repo: https://github.com/kenny8889/p-scr
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr

cd /Users/ouyang/AI-coding/rtadev-platform/p-scr
Follow SCHEMA.md, pull first, update index.md and log.md, then commit and push.
```

**Rules to remember:**

- Store outcomes, not transcripts.
- Always pull before write; push after commit.
- No secrets or sensitive customer data.
- On git conflict, stop and ask the user.

## Next Actions

- Initialize remaining empty folders (`inbox/`, `handoffs/`, `assets/`, `archive/`) as needed.
- Commit pending `README.md` and `SCHEMA.md` changes to the remote when ready.
- Use the pre-commit prompt before each code repo commit that produces reusable knowledge.

## Links

- `README.md` — agent prompts and sync rules
- `SCHEMA.md` — file format and workflow spec
- Remote: https://github.com/kenny8889/p-scr
