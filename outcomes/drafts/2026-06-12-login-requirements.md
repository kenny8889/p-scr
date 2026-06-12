---
type: outcome
status: draft
created: 2026-06-12
updated: 2026-06-12
agent: "Cursor Composer"
project: "rtadev-platform"
tags: [login, authentication, prd, requirements]
sensitivity: internal
source: "Cursor chat — login requirements document"
---

# Login Feature Requirements (PRD)

## Context

A reusable product requirements document for a standard Web/App login flow was needed before implementing authentication in the code repo. No project-specific auth stack exists in SCR yet; this draft covers account-password login, session management, security controls, API contracts, and acceptance criteria for a typical B2B/B2C application.

## Outcome

### Goals (P0)

- Account + password login with validated session
- Redirect unauthenticated users to `/login` with safe in-app `redirect`
- Logout with immediate server-side session invalidation
- Brute-force protection: lock account for 15 minutes after 5 failed attempts
- Unified error message for wrong credentials (no account enumeration)

### Out of scope (this phase)

- Social/OAuth login, MFA, SSO, biometrics

### Functional requirements (summary)

| Area | Key requirements |
|------|------------------|
| Login page | `/login`; links to register and forgot-password |
| Form | Account (email or phone TBD); masked password; optional show/hide; optional remember-me |
| Success | Issue token/session; redirect to `redirect` if safe in-app path, else home |
| Logout | Clear client credential; invalidate server session |
| Session | Default 2h idle expiry; remember-me extends to 7 days; 401 → re-login |
| Security | HTTPS only; bcrypt/argon2 password hash; IP rate limit 20/min; login audit log |

### API sketch

```text
POST /api/v1/auth/login     — account, password, rememberMe
POST /api/v1/auth/logout    — Bearer token
GET  /api/v1/auth/me        — current user
```

Error codes: `10001` wrong credentials; `10002` account locked (429).

### Core data fields

**User:** `id`, `email`, `phone`, `password_hash`, `status` (active/disabled), `failed_login_count`, `locked_until`, `last_login_at`

**LoginLog (optional):** `user_id`, `account`, `ip`, `user_agent`, `result`, `created_at`

### Edge cases

- Disabled account → explicit message to contact admin
- Already logged in visiting `/login` → redirect home or safe `redirect`
- External `redirect` URLs → ignored (open-redirect prevention)
- Double-submit → single in-flight request with loading state

### Acceptance checklist

- [ ] Valid credentials login and redirect
- [ ] Wrong password does not reveal whether account exists
- [ ] 5 failures → 15-minute lockout
- [ ] Logout invalidates session
- [ ] Protected routes redirect to login with `redirect`
- [ ] HTTPS + hashed passwords
- [ ] Desktop and mobile layouts work

### Suggested milestones

| Phase | Scope | Estimate |
|-------|-------|----------|
| M1 | Login UI + login API + session | 3 days |
| M2 | Logout, expiry, redirect | 1 day |
| M3 | Lockout, rate limit, audit log | 2 days |
| M4 | Integration, QA, security review | 2 days |

### Open questions

1. Account type: email only, phone only, or both?
2. Session: JWT vs server-side session?
3. Single-session per account (kick other devices)?
4. Register and forgot-password in same release?
5. Admin disable/unlock in back office?

## Next Actions

- Confirm open questions with product/tech lead before implementation.
- Choose `outcomes/drafts/` → move to `outcomes/decisions/` once auth stack is decided.
- Implement in code repo after PRD sign-off.

## Links

- Related SCR: [[outcomes/research/2026-06-11-p-scr-setup-and-usage.md]] (pre-commit SCR workflow)
- Template: [[templates/outcome.md]]
