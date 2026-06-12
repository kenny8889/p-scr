# Personal SCR 使用指南

本文说明如何开启 GitHub 分支审批、如何按个人环境更新 `README.md` / `SCHEMA.md`，以及 Agent 对话完成后如何将成果交付到 Personal SCR。

**推荐阅读顺序：** 一（GitHub 开启分支审批）→ 二（更新 README / SCHEMA）→ 三（Agent 提示词）

**仓库地址：** https://github.com/kenny8889/p-scr  
**本机克隆路径：** `/Users/ouyang/AI-coding/rtadev-platform/p-scr`

更完整的格式规范见项目根目录的 [SCHEMA.md](../SCHEMA.md)。

---

## 一、GitHub 开启分支审批

Personal SCR 的合并流程要求：所有进入 `develop` / `main` 的变更必须经过 **Pull Request**，并由 **@lpcaitt** 审批（见 `.github/CODEOWNERS`）。

需要配置三层机制，缺一不可：

```text
CODEOWNERS（指定审批人）
    +
分支保护 / Rulesets（强制 PR + 审批）
    +
正确工作流（feature/kenny8889 → PR → 合并 develop）
```

### 1.1 配置 CODEOWNERS

仓库已包含 `.github/CODEOWNERS`：

```text
# 所有提交到 develop 或 main 分支的 PR，
# 都必须由 @lpcaitt 审批通过，才能合并
*   @lpcaitt
```

含义：任意路径的 PR，只要目标是 `develop` 或 `main`，都需要 @lpcaitt 作为 Code Owner 审批。

> CODEOWNERS **只作用于 Pull Request**，不能阻止直接 `git push` 到受保护分支。必须配合下面的分支保护规则。

### 1.2 配置分支保护（main / develop）

**路径：** 仓库 → **Settings** → **Branches** → **Add rule**  
（或新版 UI：**Settings** → **Rules** → **Rulesets**）

对 `main` 和 `develop` 分别启用：

| 设置项 | 建议值 |
|--------|--------|
| Require a pull request before merging | ✅ 开启 |
| Required approvals | ≥ 1 |
| Require review from Code Owners | ✅ 开启 |
| Dismiss stale approvals when new commits are pushed | ✅ 建议开启 |
| **Do not allow bypassing the above settings** | ✅ **强烈建议开启（含 Admin）** |

最后一项很关键：若不开启，仓库 Admin 仍可直推 `develop`，绕过 @lpcaitt 审批。

### 1.3 审批流程示意

```text
你在 feature/kenny8889 上 commit
        ↓
git push origin feature/kenny8889
        ↓
在 GitHub 开 PR：feature/kenny8889 → develop
        ↓
@lpcaitt Review 并 Approve
        ↓
在 GitHub 上 Merge PR（不要本地 merge develop 再 push）
        ↓
develop 更新完成
```

### 1.4 禁止的操作

| ❌ 不要做 | 原因 |
|-----------|------|
| `git push origin develop` | 绕过 PR 和 Code Owner 审批 |
| 本地 `git merge feature/kenny8889` 到 develop 再 push | 同上 |
| 为每次 SCR 新建 `feature/<topic>` 分支 | 统一使用自己的个人 feature 分支（见下文第二节） |

---

## 二、配置个人分支与工作流（更新 README.md 和 SCHEMA.md）

完成第一节的 GitHub 开启分支审批配置后，**每位使用者还需要根据自己的环境更新 `README.md` 和 `SCHEMA.md`**，再使用第三节的 Agent 提示词。

原因：

- **个人 feature 分支不同** — 如 `feature/kenny8889`、`feature/lpcaitt`、`feature/Atom` 等，每人应固定使用自己的分支，不要混用。
- **PR 目标分支可能不同** — 本仓库当前示例为 `develop`，你的团队也可能用 `main` 或其他集成分支。
- **审批人可能不同** — `.github/CODEOWNERS` 中的 reviewer 需与团队实际角色一致。
- **本机克隆路径不同** — 每台机器上的本地路径各异，Agent 需要明确路径才能 `cd` 到正确目录。

> Agent 交付时以 **`README.md` 和 `SCHEMA.md` 中的最新配置为准**，不要硬记本文中的示例值。本文档中的 `feature/kenny8889`、`develop`、`@lpcaitt` 仅为**当前仓库的示例**。

### 2.1 先确认你的个人配置项

在开始写 SCR 之前，填好下面这张表（可写在团队 wiki 或自己备忘）：

| 配置项 | 说明 | 本仓库当前示例 |
|--------|------|----------------|
| **本地克隆路径** | Agent `cd` 的目录 | `/Users/ouyang/AI-coding/rtadev-platform/p-scr` |
| **个人 feature 分支** | 所有 SCR commit 的工作分支 | `feature/kenny8889` |
| **PR 目标分支** | 合并 SCR 的远程分支 | `develop` |
| **审批人** | CODEOWNERS 指定的 GitHub 用户 | `@lpcaitt` |
| **远程仓库** | GitHub 仓库 URL | `https://github.com/kenny8889/p-scr` |

### 2.2 需要更新 README.md 的位置

打开 [README.md](../README.md)，按你的实际配置修改以下部分：

| 章节 | 改什么 |
|------|--------|
| **Local clone** | 本机克隆路径、`git clone` 示例命令 |
| **Branch and review workflow** | 个人 feature 分支名、PR 目标分支、审批人 |
| **Required Agent Behavior** | 流程中的分支名与 pull 来源（如 `origin develop`） |
| **Agent Sync Rules** | 同上 |
| **How to Ask Agents to Deliver to SCR** | 复制粘贴提示词中的路径、分支名、目标分支、审批人 |

**示例（把分支改成你自己的）：**

```text
# 改前（他人示例）
Checkout feature/kenny8889 and merge develop if behind.
Commit on feature/kenny8889, push, and open a PR to develop.
Wait for @lpcaitt approval ...

# 改后（你的配置）
Checkout feature/lpcaitt and merge develop if behind.
Commit on feature/lpcaitt, push, and open a PR to develop.
Wait for @lpcaitt approval ...
```

若 PR 目标是 `main` 而非 `develop`，把所有 `develop` 替换为你的目标分支，并把 `git pull --ff-only origin develop` 改为 `git pull --ff-only origin main`。

### 2.3 需要更新 SCHEMA.md 的位置

打开 [SCHEMA.md](../SCHEMA.md)，同步修改 Agent 会读取的规范：

| 章节 | 改什么 |
|------|--------|
| **Agent Workflow** | `git pull` 来源分支、checkout 的 feature 分支、PR 目标、审批人 |
| **Git Sync Rules → Branch and review workflow** | 流程图、规则表中的分支名与审批人 |
| **Git Sync Rules → General rules** | push 的分支名、禁止直推的目标分支 |

**核心原则（分支名可变，规则不变）：**

```text
<目标分支> (pull) → <个人feature分支> (commit) → push → PR → <审批人> approves → GitHub 合并
```

**禁止项（与分支名无关）：**

- 不要 `git push origin <目标分支>`
- 不要在本地 merge 到 `<目标分支>` 再 push
- 不要为他人创建或占用别人的 `feature/<用户名>` 分支

### 2.4 同步更新 .github/CODEOWNERS（如审批人不同）

若你的审批人不是 `@lpcaitt`，还需修改 [.github/CODEOWNERS](../.github/CODEOWNERS)：

```text
# 所有提交到 <目标分支> 的 PR，都必须由 @你的审批人 审批通过，才能合并
*   @你的审批人
```

此文件变更本身也要走 PR 流程合并进目标分支。

### 2.5 建议的一次性检查清单

- [ ] 已创建并 checkout 自己的 `feature/<用户名>` 分支
- [ ] `README.md` 中路径、分支名、审批人已改为自己的配置
- [ ] `SCHEMA.md` 中 Agent Workflow 与 Git Sync Rules 已同步
- [ ] `.github/CODEOWNERS` 审批人与团队约定一致
- [ ] 第一节的分支保护规则已覆盖你的 PR 目标分支（`develop` / `main` 等）
- [ ] 用一次测试 PR 验证：未审批时无法合并

完成以上步骤后，再使用第三节的交付提示词（提示词内容应与更新后的 `README.md` 一致）。

---

## 三、Agent 对话完成后交付到 Personal SCR

> **使用前请确认：** 已按第二节更新 `README.md` 和 `SCHEMA.md`。以下提示词以本仓库当前配置为例；你应使用 README 里更新后的版本。

### 3.1 什么时候该 Deliver？

当一次 Agent 对话产生了**可复用的成果**，例如：

- 需求文档、设计决策、调研结论
- 代码审查要点、实现总结
- 交给下一个 Agent 的 handoff

**不要**把整段聊天记录原样存入 SCR，只存提炼后的 outcome。

### 3.2 标准流程

```text
1. git pull --ff-only origin develop
2. git checkout feature/kenny8889（若落后则 git merge develop）
3. 按 templates/ 模板写 markdown，放入 outcomes/ 或 handoffs/
4. 更新 index.md 和 log.md
5. git commit（在 feature/kenny8889 上）
6. git push origin feature/kenny8889
7. 开 PR → develop
8. 等 @lpcaitt Approve
9. 在 GitHub Merge PR
10. 再去提交你的代码仓库
```

> **注意：** 不需要、也不应该在本地先把 `feature/kenny8889` 合并到 `develop`。合入 `develop` 只在 GitHub PR 合并时发生。

### 3.3 交付提示词（标准版）

Agent 对话结束后，在提交代码仓库**之前**，发送：

```text
Deliver this to my Personal SCR as an outcome before I commit the code repo.

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

### 3.4 交付提示词（精简版）

> 分支名、目标分支、审批人、本地路径以第二节更新后的 `README.md` / `SCHEMA.md` 为准。下方为当前仓库示例。

```text
Deliver this to my Personal SCR as an outcome before I commit the code repo.
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr
Follow SCHEMA.md, pull develop first, commit on feature/kenny8889, update index.md and log.md,
open PR to develop, wait for @lpcaitt approval, then report file path, commit hash, and PR URL.
```

### 3.5 Handoff 提示词（交给下一个 Agent）

```text
Create a Personal SCR handoff for the next agent.

Repo: https://github.com/kenny8889/p-scr
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr

cd /Users/ouyang/AI-coding/rtadev-platform/p-scr
Follow SCHEMA.md.
Before writing, run git pull --ff-only origin develop.
Checkout feature/kenny8889 and merge develop if behind.
The handoff must include mission, current state, read-first files, constraints,
definition of done, and open questions.
Save it under handoffs/, update index.md, append to log.md.
Commit on feature/kenny8889, push, open PR to develop, wait for @lpcaitt approval, merge on GitHub.
Report the file path, commit hash, and PR URL.
```

### 3.6 从 SCR 读取上下文的提示词

```text
Use my Personal SCR as shared memory.

Repo: https://github.com/kenny8889/p-scr
Local clone: /Users/ouyang/AI-coding/rtadev-platform/p-scr

cd /Users/ouyang/AI-coding/rtadev-platform/p-scr
Run git pull --ff-only origin develop first.
Read SCHEMA.md, then index.md, then files relevant to my task.
Cite which SCR files you used (paths relative to repo root).
```

### 3.7 Agent 应回报的信息

交付完成后，Agent 应告知：

| 项 | 示例 |
|----|------|
| 文件路径 | `outcomes/drafts/2026-06-12-login-requirements.md` |
| Commit hash | `562e480...` |
| PR 链接 | `https://github.com/kenny8889/p-scr/pull/3` |

你需要做的：通知或等待 **@lpcaitt** 在 PR 上 Approve，PR 合并后再去提交代码仓库。

### 3.8 文件存放位置速查

| 类型 | 目录 | 模板 |
|------|------|------|
| 调研、总结、草稿 | `outcomes/research/`、`outcomes/drafts/` 等 | `templates/outcome.md` |
| 任务交接 | `handoffs/` | `templates/handoff.md` |
| 决策记录 | `outcomes/decisions/` | `templates/decision.md` |

每次交付还需更新：

- `index.md` — 导航索引
- `log.md` — 活动日志（append-only）

---

## 四、常见问题

**Q：换了 feature 分支名后 Agent 仍用旧分支？**  
A：检查 `README.md` 和 `SCHEMA.md` 是否已更新；交付提示词应与 README 中的复制粘贴版本一致。

**Q：能在本地先 merge 到 develop 吗？**  
A：不能。只在 GitHub PR 合并时更新 `develop`。

**Q：PR 合并不了，提示 Waiting on code owner review？**  
A：正常，需 @lpcaitt 在 PR 页面点 **Approve**。

**Q：Admin 能跳过审批吗？**  
A：若未开启 "Do not allow bypassing"，Admin 可以直推。建议开启该选项。

---

## 相关文档

- [README.md](../README.md) — **需按个人配置更新**；Agent 交付提示词以此为准
- [SCHEMA.md](../SCHEMA.md) — 文件格式与 Git 同步规范
- [outcomes/research/2026-06-12-github-branch-protection-main-develop.md](../outcomes/research/2026-06-12-github-branch-protection-main-develop.md) — 分支保护详细说明
- [outcomes/research/2026-06-12-develop-push-bypasses-codeowner-approval.md](../outcomes/research/2026-06-12-develop-push-bypasses-codeowner-approval.md) — 为何直推会绕过审批
