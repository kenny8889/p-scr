# Personal SCR Schema

This file defines the rules for a lightweight personal Shared Certified Repository (SCR).

## Purpose

Personal SCR is the shared memory for useful outcomes created by AI agents across chats, tools, and devices.

It is inspired by the formal DFOS SCR, but simplified for personal work:

- It stores durable outcomes, not active task chatter.
- It favors clear markdown files over databases.
- It is agent-readable and human-browsable.
- It lets another agent understand what was decided, produced, or handed off.

## Core Principles

1. **Outcome over transcript.** Save the distilled result, not the whole conversation.
2. **Enough context to resume.** Every file should explain why it exists and what to do next.
3. **Stable structure.** Agents should use the folder and frontmatter conventions below.
4. **Append-only by default.** Prefer creating a new outcome or adding a changelog instead of silently rewriting history.
5. **No secrets.** Do not store API keys, private keys, passwords, recovery phrases, or sensitive customer data.
6. **GitHub is the sync layer.** Agents must use Git pull/commit/push instead of relying on cloud-drive file synchronization.

## Directory Structure

```text
SCR/
├── README.md
├── SCHEMA.md
├── index.md
├── log.md
├── inbox/
├── outcomes/
│   ├── feedback/
│   ├── decisions/
│   ├── research/
│   ├── drafts/
│   └── code-review/
├── handoffs/
├── templates/
│   ├── outcome.md
│   ├── handoff.md
│   └── decision.md
├── assets/
└── archive/
```

## File Naming

Use kebab-case markdown filenames.

Preferred format:

```text
YYYY-MM-DD-short-topic.md
```

Examples:

- `2026-05-22-ngas-scr-feedback.md`
- `2026-05-22-payment-agent-design-decision.md`
- `2026-05-22-next-agent-handoff.md`

## Page Types

### Outcome

Use for feedback, analysis, summary, draft, review, or reusable thinking.

Required frontmatter:

```yaml
---
type: outcome
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
agent: "agent name or tool"
project: "project or area"
tags: []
sensitivity: internal
source: "chat/session/source if known"
---
```

Required sections:

```markdown
# Title

## Context
Why this exists.

## Outcome
The useful result.

## Next Actions
- Action or "None".

## Links
- Related files, wiki pages, repos, or source references.
```

### Handoff

Use when one agent is passing work to another agent, another device, or a future session.

Required frontmatter:

```yaml
---
type: handoff
status: ready
created: YYYY-MM-DD
updated: YYYY-MM-DD
from_agent: "agent name or tool"
to_agent: "target agent or role"
project: "project or area"
priority: normal
tags: []
sensitivity: internal
---
```

Required sections:

```markdown
# Handoff: Title

## Mission
What the next agent should accomplish.

## Current State
What is already known or done.

## Read First
- Files or links in order.

## Constraints
- Must-follow rules.

## Definition of Done
- Observable completion criteria.

## Open Questions
- Question or "None".
```

### Decision

Use for decisions that should be remembered and cited later.

Required frontmatter:

```yaml
---
type: decision
status: accepted
created: YYYY-MM-DD
updated: YYYY-MM-DD
deciders: []
project: "project or area"
tags: []
sensitivity: internal
---
```

Required sections:

```markdown
# Decision: Title

## Decision
What was decided.

## Rationale
Why this choice was made.

## Alternatives Considered
- Alternative and tradeoff.

## Consequences
- Expected impact.

## Links
- Related outcomes, handoffs, or source material.
```

## Status Values

- `draft` - incomplete and not ready to rely on.
- `active` - current and useful.
- `ready` - handoff is ready for another agent.
- `accepted` - decision is accepted.
- `superseded` - replaced by newer material.
- `archived` - retained for history only.

## Index Rules

Maintain `index.md` as the navigation catalog.

Each entry should include:

```markdown
- [[path/to/file]] - one-line summary | status: active | updated: YYYY-MM-DD
```

Group entries by type:

- Handoffs
- Decisions
- Feedback
- Research
- Drafts
- Code Review

## Log Rules

Maintain `log.md` as an append-only activity log.

Use this format:

```markdown
## [YYYY-MM-DD] type | Short title
- Created/updated: [[path/to/file]]
- Summary: one sentence.
- Next: optional next step.
```

Valid log types:

- `outcome`
- `handoff`
- `decision`
- `update`
- `archive`

## Agent Workflow

When the user says "deliver this to SCR":

1. Run `git pull --ff-only`.
2. Identify the page type: outcome, handoff, or decision.
3. Choose the destination folder.
4. Create a markdown file from the template.
5. Fill all frontmatter fields.
6. Write concise, reusable content.
7. Update `index.md`.
8. Append to `log.md`.
9. Commit all related changes together.
10. Push immediately.
11. Report the created file path and commit hash.

When updating an existing SCR file:

1. Run `git pull --ff-only`.
2. Read the current file first.
3. Preserve useful existing content.
4. Add a changelog or update the `updated` date.
5. Update `index.md` if the summary or status changed.
6. Append to `log.md`.
7. Commit all related changes together.
8. Push immediately.

If Git reports a conflict, stop and ask the user. Do not resolve semantic conflicts silently.

## Git Sync Rules

Repository (authoritative — same on every machine):

```text
https://github.com/kenny8889/p-scr
```

Local clone path is `/Users/ouyang/AI-coding/rtadev-platform/p-scr`. Agents must `cd` to the clone root that contains `README.md` and `SCHEMA.md`.

Rules:

1. Always run `git pull --ff-only` before reading or writing (from the clone root).
2. Never write directly without pulling first.
3. Keep each outcome/handoff/decision plus `index.md` and `log.md` in the same commit.
4. Push after every successful commit.
5. If a conflict occurs, stop and ask the user.
6. Do not use Google Drive or cloud-drive mounts as the authoritative sync layer.
7. Cite SCR files by **repo-relative paths** (e.g. `handoffs/foo.md`) in handoffs and outcomes, not host-specific absolute paths.

## Relationship to Formal DFOS SCR

Formal DFOS SCR is for production-grade certification, regulatory gates, auditability, and multi-agent SDLC promotion.

Personal SCR is for lightweight personal continuity. It records outcomes that are good enough to reuse, but it does not imply production approval unless a file explicitly says so.
