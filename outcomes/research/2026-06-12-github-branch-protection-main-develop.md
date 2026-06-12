---
type: outcome
status: active
created: 2026-06-12
updated: 2026-06-12
agent: "Cursor Composer"
project: "p-scr"
tags: [github, branch-protection, code-review, workflow]
sensitivity: internal
source: "Cursor chat — GitHub merge approval for main/develop"
---

# GitHub Branch Protection: Require Approval on main/develop

## Context

The `p-scr` repo already has `.github/CODEOWNERS` assigning `@kenny8889` as the required reviewer for all paths. CODEOWNERS alone does not block merges — GitHub **branch protection rules** (or **rulesets**) must also be enabled on `main` and `develop`.

## Outcome

### What is already in the repo

`.github/CODEOWNERS`:

```text
*   @kenny8889
```

This declares who must approve PRs, but enforcement requires branch protection.

### Required GitHub settings (per branch: `main`, `develop`)

**Path:** Repository → **Settings** → **Branches** → **Add rule** (or **Rules** → **Rulesets** on newer UI).

Enable at minimum:

| Setting | Value |
|---------|-------|
| Require a pull request before merging | On |
| Required approvals | ≥ 1 |
| Require review from Code Owners | On (pairs with CODEOWNERS) |
| Dismiss stale approvals when new commits are pushed | Recommended |
| Do not allow bypassing the above settings | Recommended (admins included) |

Optional but useful:

- Require status checks to pass before merging (CI)
- Require branches to be up to date before merging

### Rulesets (newer GitHub UI)

1. **Settings** → **Rules** → **Rulesets** → **New branch ruleset**
2. Target branches: `main`, `develop`
3. Enable **Require a pull request before merging**
4. Set minimum approvals and **Require approval from owners**

### CLI alternative (`gh`)

```bash
gh api repos/OWNER/REPO/branches/main/protection -X PUT \
  -f required_pull_request_reviews[required_approving_review_count]=1 \
  -f required_pull_request_reviews[require_code_owner_reviews]=true \
  -f required_pull_request_reviews[dismiss_stale_reviews]=true \
  -f enforce_admins=true \
  -f required_status_checks=null \
  -f restrictions=null
```

Repeat for `develop`. Replace `OWNER/REPO` with `kenny8889/p-scr`.

### How the pieces fit together

```text
PR → target main/develop
  → branch protection: PR required + N approvals
  → code owners: @kenny8889 must approve (if enabled)
  → merge allowed
```

### Notes

- Requires **Admin** on the repository (or org-level ruleset access).
- If the PR author is the sole code owner, consider requiring 2 approvals or disallowing self-approval.
- Org repos may inherit rules from **Organization Settings → Rules**; check there if repo-level rules seem ignored.

## Next Actions

- Apply branch protection rules on GitHub for `main` and `develop` (UI or `gh`).
- Verify with a test PR: merge button should stay disabled until `@kenny8889` approves.
- Then commit any pending code-repo changes.

## Links

- `.github/CODEOWNERS` — code owner definition
- Remote: https://github.com/kenny8889/p-scr
- GitHub docs: [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- Related: [[outcomes/research/2026-06-11-p-scr-setup-and-usage.md]]
