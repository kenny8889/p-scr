---
type: outcome
status: active
created: 2026-06-12
updated: 2026-06-12
agent: "Cursor Composer"
project: "p-scr"
tags: [documentation, workflow, pr, codeowners, readme, schema]
sensitivity: internal
source: "Cursor chat — align README/SCHEMA with PR approval workflow"
---

# README and SCHEMA: PR Approval Workflow Update

## Context

`develop` and `main` merges now require a Pull Request and **@lpcaitt** approval (`.github/CODEOWNERS` + branch protection). `README.md` and `SCHEMA.md` still described a simple commit-and-push flow, which could mislead agents into direct-pushing `develop` and bypassing review.

## Outcome

### What changed

**`SCHEMA.md` — Agent Workflow & Git Sync Rules**

- Delivery steps now use `feature/kenny8889` → PR → `develop` → @lpcaitt approval → GitHub merge.
- Explicitly forbids `git push origin develop` and local merge-then-push.
- Documents `feature/*` ruleset push workaround (delete stale remote branch, re-push).

**`README.md` — Branch workflow & prompts**

- New **Branch and review workflow** section with flow diagram.
- Updated Required Agent Behavior, Agent Sync Rules, and copy-paste deliver/handoff prompts.

### Canonical agent deliver prompt (short)

```text
Deliver this to my Personal SCR as an outcome before I commit the code repo.
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr
Follow SCHEMA.md, pull develop first, commit on feature/kenny8889, open PR to develop,
wait for @lpcaitt approval, then report file path, commit hash, and PR URL.
```

### Key rule (no local merge to develop)

```text
develop → feature/kenny8889   ✅ sync before writing (if behind)
feature/kenny8889 → develop   ❌ never locally; merge only via approved PR on GitHub
```

## Next Actions

- Merge PR to `develop` after @lpcaitt approves.
- Optionally move this workflow summary into a `decisions/` record if treated as a long-term policy.

## Links

- `README.md` — usage guide and deliver prompts
- `SCHEMA.md` — agent workflow and git sync rules
- `.github/CODEOWNERS` — `* @lpcaitt`
- Related: [[outcomes/research/2026-06-12-develop-push-bypasses-codeowner-approval.md]]
