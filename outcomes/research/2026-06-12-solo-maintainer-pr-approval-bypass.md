---
type: outcome
status: active
created: 2026-06-12
updated: 2026-06-12
agent: "Cursor Composer"
project: "p-scr"
tags: [github, branch-protection, codeowners, admin-bypass, solo-maintainer]
sensitivity: internal
source: "Cursor chat — why @kenny8889 cannot self-approve PRs and Admin bypass"
---

# Solo Maintainer: PR Self-Approval Blocked and Admin Bypass

## Context

After CODEOWNERS changed to `@kenny8889` (PR #5), PR #6 showed **Approve** greyed out for the `kenny8889` account. The user is both PR author and sole Code Owner — a common deadlock for single-maintainer repos.

## Outcome

### Why Approve is disabled

| Factor | Effect |
|--------|--------|
| PR author | GitHub blocks authors from approving their own PR |
| CODEOWNERS `* @kenny8889` | Merge requires Code Owner approval |
| Author = only Code Owner | Required approver cannot approve → deadlock |

**Approve stays grey; Comment still works.** This is expected, not an account permission bug.

### Admin bypass (solo maintainer workflow)

Uncheck branch protection option:

**「Do not allow bypassing the above settings」**

Then repo **Admin** can **Merge pull request** without clicking Approve.

```text
feature/kenny8889 → open PR → develop
  → (skip Approve)
  → Admin Merge on GitHub
```

This does **not** re-enable self-Approve; it allows merge without approval.

### Alternatives (if strict review is required)

1. Add a second CODEOWNERS entry (e.g. `@kenny8889 @lpcaitt`) — another person approves your PRs.
2. Keep bypass disabled — only non-author reviewers can merge.
3. Separate accounts: one opens PR, one approves (team setup).

### Recommendation for this repo

For a **single maintainer** Personal SCR:

- Use **Admin bypass merge** for routine SCR deliveries, or
- Add a **second Code Owner** if a colleague should review every PR.

Update `docs/personal-scr-usage.md` §四 FAQ if this becomes the standard solo workflow.

## Next Actions

- Confirm branch protection on `develop`/`main`: bypass allowed for Admin if using direct merge.
- Optional: document Admin bypass in `docs/personal-scr-usage.md`.
- If team grows: add second CODEOWNERS reviewer and re-enable "Do not allow bypassing".

## Links

- `.github/CODEOWNERS` — `* @kenny8889`
- [docs/personal-scr-usage.md](../../docs/personal-scr-usage.md)
- Related: [[outcomes/research/2026-06-12-codeowners-reviewer-kenny8889.md]]
- PR #6 (merged): https://github.com/kenny8889/p-scr/pull/6
