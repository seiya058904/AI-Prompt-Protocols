# 项目收工提示词

> **Purpose:** 在用户明确要求“收工 / 收尾”时，将本轮 Git 成果安全收敛到仓库既有工作流允许的最终状态。
> **Audience:** Codex、Claude Code、Cursor 等 coding agents。

## 触发条件

只有当用户明确表示：

* “执行收工”
* “项目收工”
* “项目收尾”
* “把这次涉及的仓库都收工”

或等价意思时执行。

普通的：

* “看看状态”
* “总结一下”
* “检查是否完成”

不自动触发 commit、push 或其他远端写操作。

## 收工授权边界

用户明确调用本提示词，视为授权完成正常、低风险、task-owned 的 Git 收尾，包括：

* 状态检查
* diff 检查
* `git fetch`
* 为本轮明确成果创建正常 commit
* 对本轮明确成果执行正常 push
* 根据仓库既有工作流完成安全、常规的本地 integration

这不自动授权：

* force push
* rewrite published history
* `reset --hard`
* 丢弃用户修改
* 删除来源不明文件
* 绕过 branch protection
* 绕过 CI
* 生产部署
* Release 发布
* 数据库或线上数据修改
* secret / credential 修改
* 仓库权限或保护规则修改

遇到这些情况必须获得额外明确授权。

## 核心目标

收工的判断标准不是“执行过 commit / push”。

真正目标是：

> **本轮需要保留的成果已经被安全保存，并达到当前仓库正式工作流允许的最终状态，没有遗留本轮未处理工作。**

仓库的正式 integration model 可能不同。

先识别，再执行。

## 1. 识别仓库 Integration Model

根据：

* `AGENTS.md`
* CONTRIBUTING
* branch protection
* CI / workflows
* remote configuration
* 当前 Git 历史与分支结构

判断仓库属于哪类：

### Direct-to-default

允许正常直接进入默认分支。

目标通常是：

```text
local default HEAD == origin/default HEAD
working tree clean
ahead = 0
behind = 0
```

### Feature Branch + Integration

正式成果先在任务分支，然后通过仓库既有 merge / integration 流程进入正式分支。

按照既有流程执行。

### Protected / PR-based

默认分支受保护，正式流程要求 PR。

不要为了“收工漂亮”绕过 branch protection。

将成果推进到仓库允许的最完整安全状态。

如果仍存在需要人工批准、review 或 merge 的正式步骤，在最终报告中明确说明。

### Other

如果仓库有其他明确工作流，以仓库规则为准。

不要强行套用 `main` + direct push 模型。

## 2. 开始前盘点

从真实 repository root 工作。

至少检查：

```text
git rev-parse --show-toplevel
git status --short
git status -sb
git branch --show-current
git branch -vv
git remote -v
git worktree list --porcelain
git log --oneline --decorate -10
```

然后正常：

```text
git fetch
```

如果存在多个 remote，识别正式 upstream。

记录：

* repository root
* 当前 branch
* 默认 branch
* upstream
* HEAD
* remote HEAD
* ahead / behind
* staged / unstaged / untracked
* worktrees
* task branches
* 本轮成果实际在哪里

不要只检查当前 shell 所在 worktree。

## 3. 区分修改归属

将当前内容区分为：

1. 本轮明确产生、应该保留的成果
2. 用户此前已经存在的修改
3. 来源不明或与本任务无关的修改

本轮成果应收工。

用户已有或来源不明的内容：

* 不覆盖
* 不删除
* 不 reset
* 不 clean
* 不偷偷混入当前 commit

如果无法安全区分，停止相关 mutation，并向用户说明具体文件和风险。

## 4. 补齐遗漏 Commit

如果本轮成果仍未提交：

先检查：

```text
git diff --check
git diff
git diff --cached
```

只暂存确认属于本轮的路径：

```text
git add -- <explicit-paths>
```

避免无脑：

```text
git add .
git add -A
```

除非已经确认所有变化全部属于同一个任务。

提交前再次检查：

```text
git diff --cached --check
git diff --cached --stat
git diff --cached
```

然后使用符合仓库现有风格的 commit message。

不要为了制造 clean 而提交来源不明内容。

不要创建空 commit。

### Push 前审计 Outgoing Commits

在任何 push 前，确定 outgoing commit 审计的比较基准（comparison base）。

如果当前 branch 已有 upstream，对照该 upstream 比较：

```text
git log --oneline <upstream>..HEAD
```

如果 branch 尚无 upstream（例如首次 push 前的新 feature/task branch），对照该 task branch 预期派生或整合的 branch / ref 比较——通常是已验证的 remote default branch，或其他仓库定义的 base branch：

```text
git log --oneline <verified-base-ref>..HEAD
```

不能因为尚未配置 upstream 就跳过 outgoing commit 审计。

不要武断假定 base 一定是 `origin/main`。通过 repository workflow、branch history、merge-base、remote default branch 以及当前任务创建分支时的上下文，确定合理的 comparison base。

需要时再检查：

```text
git diff --stat <comparison-ref>...HEAD
```

普通 push 除了当前任务，还可能发布 branch 上已有的、尚未发布的 pre-existing local commits。

只有 outgoing commits 已被理解、且属于已授权的收工范围时，才 push。

如果无法可靠确认新 branch 的 comparison base，且因此无法判断 outgoing commits 的授权范围，则不要 push，并向用户说明。

如果 outgoing 范围内包含无关、预存或不确定的 commits，不要为了“让仓库同步”而推送它们。

不要通过改写历史来移除它们。

保留现状并把问题呈现给用户决策，除非仓库既有工作流提供了另一条明确安全的路径。

## 5. 处理 Branch / Worktree 中的成果

如果本轮成果位于 task branch 或 worktree：

确认：

* 是否已经 commit
* 是否已经安全保存
* 是否已经进入仓库正式 integration flow
* 是否仍存在只有某个 branch/worktree 才有的正式成果

不要因为 task branch 已 push 就自动认为收工完成。

但也不要假定所有仓库都必须立刻把 commit 直接合并到默认分支。

以该仓库正式 integration model 为准。

## 6. Divergence 和高风险情况

如果本地与远端出现分叉，或者必须在以下策略中做业务选择：

* merge
* rebase
* reset
* force push

不要擅自决定。

尤其以下情况必须询问用户：

* 双方都有独有 commit
* published history 需要改写
* 可能覆盖他人远端成果
* 需要 force
* merge conflict 涉及业务语义选择
* 无法判断哪一侧才是正确版本

普通、无歧义、可安全机械解决的冲突可以处理。

涉及不同业务实现或不同用户成果的冲突必须询问。

## 7. Multi-Repository Closeout

如果用户要求对多个仓库统一收工：

每个 repository 必须独立检查。

不要用一个仓库的状态推断另一个。

对于每个仓库分别确认：

* repository root
* branch
* dirty state
* task-owned changes
* local vs remote
* final HEAD
* integration model
* required actions

只处理本轮涉及的仓库。

对本轮未修改的仓库，如果用户要求确认状态，可以只读验证；不要为了“统一收工”制造无意义 commit。

## 8. 完成条件

一个仓库只有在满足其 integration model 对应的终态时，才可以标记为收工。

### 对允许 direct integration 的仓库

通常应达到：

* 本轮成果已 commit
* 已正常 push
* local default 与 remote default 同步
* ahead = 0
* behind = 0
* working tree clean
* 无本轮遗漏 untracked
* 无悬空 task-owned worktree 成果

### 对 PR / protected workflow

应达到仓库允许的最完整安全状态，例如：

* 本轮成果已 commit
* task branch 已正常 push
* 必要的验证已完成
* PR 已存在或已经达到可创建 PR 的状态
* 没有未保存的本轮成果

如果 merge 仍需要 review、用户批准或其他明确授权：

不要绕过。

将其记录为：

```text
Remaining integration step
```

这不等于任务失败，只表示正式仓库流程尚有外部审批步骤。

## 9. 最终验证

完成实际收尾动作后重新检查：

```text
git status --short
git status -sb
git branch -vv
git log -1 --oneline
```

必要时检查：

```text
git rev-list --left-right --count HEAD...<upstream>
```

并确认：

* 没有遗漏 task-owned 修改
* 没有意外 staged 内容
* 没有来源不明内容被混入 commit
* 本轮成果已经安全保存
* 没有通过 destructive cleanup 人为制造 clean 状态

## 10. 最终报告

单仓库简洁报告：

* Repository
* Integration model
* Final branch / HEAD
* Working tree
* Local vs remote
* Commit / push actions
* Checks performed
* Remaining integration step, if any

多仓库使用：

| Repository | Integration Model | Branch | Working Tree | Local vs Remote | Final HEAD | Action Taken |
| ---------- | ----------------- | ------ | ------------ | --------------- | ---------- | ------------ |

如果所有相关 direct-integration 仓库都完成：

```text
All directly integrated repositories touched by this task are clean, committed, pushed, and synchronized with their remotes.
```

如果 PR-based 仓库仍等待正式审批，明确列出，不要声称已经完全进入默认分支。

## 最终原则

不要为了得到漂亮的 `clean` 状态而破坏成果。

不要为了满足统一模板而绕过仓库自己的正式工作流。

收工的本质是：

> **保护成果 → 识别真实工作流 → 完成所有安全且已授权的收尾 → 验证最终状态。**