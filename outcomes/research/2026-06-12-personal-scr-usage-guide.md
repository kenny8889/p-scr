---
type: outcome
status: active
created: 2026-06-12
updated: 2026-06-12
agent: "Cursor Composer"
project: "p-scr"
tags: [documentation, usage-guide, workflow, github, agents]
sensitivity: internal
source: "Cursor chat — Personal SCR usage guide and doc cleanup"
---

# Personal SCR Usage Guide (docs/)

## Context

Team members needed a single onboarding doc covering GitHub branch approval setup, per-user README/SCHEMA configuration, and Agent deliver prompts. The repo also removed the `feature-个人分支限制` ruleset from GitHub, making prior `feature/*` push workarounds obsolete.

## Outcome

### New user guide

Created **`docs/personal-scr-usage.md`** with four sections:

1. **GitHub 开启分支审批** — CODEOWNERS, `main`/`develop` branch protection
2. **配置个人分支与工作流** — each user updates `README.md` / `SCHEMA.md` for their feature branch, PR target, reviewer, local path
3. **Agent 交付提示词** — standard, concise, handoff, and retrieve prompts
4. **常见问题**

### Doc sync (ruleset removal)

Removed obsolete `feature/*` update ruleset references from:

- `docs/personal-scr-usage.md` — deleted §1.3 ruleset + related FAQ
- `README.md` — removed ruleset push workaround mention
- `SCHEMA.md` — removed delete-and-repush `feature/kenny8889` workaround

### Concise deliver prompt (current)

```text
Deliver this to my Personal SCR as an outcome before I commit the code repo.
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr
Follow SCHEMA.md, pull develop first, commit on feature/kenny8889, update index.md and log.md,
open PR to develop, wait for @lpcaitt approval, then report file path, commit hash, and PR URL.
```

Branch names in prompts are **examples**; each user should align with their updated `README.md` / `SCHEMA.md` (see guide §2).

## Next Actions

- Merge PR to `develop` after @lpcaitt approves.
- New team members: read `docs/personal-scr-usage.md` → update README/SCHEMA → use §3 prompts.

## Links

- [docs/personal-scr-usage.md](../../docs/personal-scr-usage.md) — primary usage guide
- [README.md](../../README.md), [SCHEMA.md](../../SCHEMA.md) — per-user workflow config
- Related: [[outcomes/research/2026-06-12-readme-schema-pr-workflow-update.md]]
