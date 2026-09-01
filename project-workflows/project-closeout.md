# 项目收工提示词

> **Purpose:** 项目开发完成后的 Git 收工 / Handoff 协议：确保本轮成果完整提交、进入正式默认分支、同步远端、工作区干净、无悬空工作。
> **Audience:** Coding agents（Codex、Claude Code、Cursor 等）在用户明确表示「收工 / 收尾」时执行。

状态：canonical

## 触发条件

只有当用户明确表示“项目可以收工”“执行收工”“现在收工”“把项目收尾”或等价意思时，执行本提示词。

普通的“检查状态”“总结完成了什么”“看看还有没有问题”不自动触发提交、推送、合并或分支清理。

---

## 核心目标

“收工”不是简单提交当前文件，也不是只把当前分支 push 到远端。

真正的目标是：

> **把本轮已经完成的成果从它实际所在的位置安全收敛到仓库的最终正式状态。**

无论本轮工作发生在：

- 默认主分支；
- 普通 feature/fix 分支；
- 临时分支；
- Git worktree；
- detached HEAD；
- 本地已经提交但尚未推送的位置；

都必须检查这些成果是否已经：

1. 完整提交；
2. 安全保存到远端；
3. 进入仓库正式默认分支；
4. 同步到远端默认分支；
5. 最终让本地默认分支和远端默认分支指向同一最新结果；
6. 默认分支工作区完全干净。

**仅仅把 feature branch / worktree branch push 到远端，不算收工。**

**仅仅创建 PR 但尚未合并，也不算完全收工。**

只要不存在安全阻塞，就继续完成整个 Git 收尾流程，不要停在中间状态。

---

## 基本执行原则

### 1. 以“最终状态”判断是否收工

不要以“执行了 commit / push / merge”等动作作为完成依据。

是否可以收工，只看仓库最后是否真正达到本提示词定义的终态。

### 2. 主动完成低风险、可逆、结论明确的收尾工作

对于正常且明确的 Git 收尾操作，如果风险低、不会覆盖用户成果、不会改变外部生产环境，则应自行继续完成，不需要为了每一步都询问用户。

例如：

- 检查 Git 状态；
- `git fetch`；
- 对本轮明确成果创建正常 commit；
- push 已确认的任务分支；
- 安全 fast-forward；
- 检查 branch / worktree；
- 验证 commit 是否已进入默认分支；
- 最终同步和 clean 检查。

### 3. 遇到真正需要用户决策的情况，可以并且应该询问

“收工”不意味着必须无条件自主做完所有事情。

如果中途出现无法安全自行判断、具有明显风险、不可逆、会影响用户已有成果或需要用户选择的问题，应暂停相关高风险操作并询问用户。

**允许在收工过程中向用户提问。**

询问用户不是失败，也不代表结束收工流程；得到用户决定后，应继续从当前状态完成剩余收尾。

### 4. 不要因为一个问题就放弃整个收尾

如果某个局部问题需要用户确认，但其他独立且安全的检查仍可继续，应先完成能安全完成的部分，再把真正需要用户决定的事项集中说明。

不要遇到一个小问题就提前停止全部工作。

---

## 必须向用户确认的高风险 / 歧义情况

出现以下情况时，不得擅自替用户做决定。

### Git 历史或成果可能被破坏

例如：

- 需要 `git reset --hard`；
- 需要 force push；
- 需要重写已经发布的 commit history；
- 需要删除尚未确认用途的 branch；
- 需要删除尚未确认用途的 worktree；
- 需要丢弃任何未提交修改；
- 需要删除来源不明的 untracked 文件；
- 需要覆盖用户已有 commit；
- 需要恢复、回滚或撤销用户并未明确要求撤销的成果。

### 无法判断修改归属

例如：

- 当前 dirty files 中混有本轮改动和用户之前的改动；
- 无法确认某几个文件是否应进入本轮 commit；
- task branch 上存在额外 commit，但无法判断它们是否属于本轮成果；
- 两个 worktree 都有不同版本的有效修改；
- 存在多个候选成果，不清楚用户希望保留哪一个。

### 合并冲突存在业务语义选择

普通且含义明确的机械冲突可以处理。

如果冲突意味着必须在两个不同业务实现、文案、数据版本、架构方案或用户成果之间做选择，应询问用户。

### 默认分支存在异常分叉

例如：

- 本地默认分支和远端默认分支都存在独有 commit；
- 无法确定哪一侧才是正确版本；
- 正常 fast-forward 无法解决；
- 需要决定 merge / rebase / reset 等策略；
- 可能影响其他人的远端成果。

### 需要执行超出 Git 收尾范围的外部高风险操作

例如：

- 部署到生产环境；
- 发布 Release；
- 数据库迁移或数据修改；
- 删除云端资源；
- 修改线上配置；
- 修改真实 secrets / credentials；
- 开启付费资源；
- 绕过 branch protection；
- 绕过 CI；
- 改变 GitHub 仓库权限、可见性或保护规则。

这些操作除非用户已经针对当前任务明确授权，否则必须询问。

### 仓库规则与当前收尾目标冲突

如果 `AGENTS.md`、CONTRIBUTING、CI、branch protection 或仓库既有流程要求某种操作，但当前状态无法同时满足，先说明冲突，再让用户决定。

---

## 向用户询问时的要求

需要用户决策时，问题必须具体，不要只说“我遇到了问题怎么办”。

应说明：

1. 当前状态；
2. 遇到的具体问题；
3. 为什么不能安全自行决定；
4. 可选方案；
5. 每个方案最主要的影响或风险；
6. 推荐方案（如果证据足够）；
7. 用户只需要回答哪个选择。

例如：

> 当前 `main` 和 `origin/main` 已经分叉：本地有 2 个独有 commit，远端有 1 个独有 commit。
> 直接 push 会被拒绝，reset 会丢失本地 commit，因此我不会擅自处理。
> 可以选择：
> 1. merge 远端到本地，保留双方历史；
> 2. rebase 本地 2 个 commit 到远端最新提交上；
> 3. 先停止，由你检查这 3 个 commit。
> 从当前仓库历史看更符合既有习惯的是方案 2。请告诉我选 1、2 或 3。

不要让用户重新提供已经能够通过 Git 状态、仓库文件或当前上下文自行确认的信息。

---

## 收工完成条件

只有同时满足以下条件，才允许最终报告：

**可以收工**

### 1. 成果已经落地

本轮需要保留的所有修改都已经形成 Git commit。

不存在：

- 本轮遗漏的 unstaged 修改；
- 本轮遗漏的 staged 修改；
- 本轮遗漏的 untracked 文件；
- 只存在于临时 worktree 中但没有 commit 的成果；
- 只存在于某个任务分支、却尚未进入默认分支的成果。

### 2. 默认分支包含最终成果

先识别仓库真实默认分支，不要武断假设一定叫 `main`。

优先依据：

```powershell
git symbolic-ref refs/remotes/origin/HEAD
```

并结合：

```powershell
git remote show origin
git branch -vv
```

判断默认分支。

最终默认分支必须包含本轮正式成果。

如果成果最初位于其他 branch/worktree，必须继续完成必要的 integration。

### 3. 本地默认分支与远端完全一致

最终必须满足：

```text
local default branch HEAD == origin/default branch HEAD
```

且：

```text
ahead = 0
behind = 0
```

本地不能停留在“已经提交但没 push”。

远端也不能停留在旧版本。

### 4. 默认分支工作区完全干净

最终默认分支必须：

- 无 modified；
- 无 deleted；
- 无 staged；
- 无 unstaged；
- 无 untracked；
- 无尚未提交的本轮成果。

### 5. 任务分支 / worktree 没有悬空成果

如果本轮使用了其他 branch 或 worktree：

必须确认其中的成果已经：

```text
commit → push（需要时）→ integrate → default branch
```

不得留下“只有这个 branch/worktree 才有”的本轮正式成果。

任务 worktree 可以保留，但必须不存在本轮未提交成果，并且其中需要进入正式版本的 commit 已经进入默认分支。

已经安全合并且确认无独立价值的临时 worktree / 临时分支，如要删除仍需确保用途明确；如果存在任何疑问，保留即可，不要为了“收工漂亮”而清理。

---

## 第一阶段：仓库盘点

开始时先确认仓库状态：

```powershell
git rev-parse --show-toplevel
git remote -v
git symbolic-ref --short HEAD
git status --branch --short
git branch -vv
git worktree list --porcelain
git log --oneline --decorate -10
```

然后：

```powershell
git fetch origin
```

如果存在多个 remote，识别哪个才是正式 upstream，不要盲目操作。

记录：

- repository root；
- 当前 branch；
- 默认 branch；
- 当前 worktree；
- 其他 worktree；
- upstream；
- 当前 HEAD；
- `origin/<default>`；
- dirty files；
- staged files；
- untracked files；
- 当前 branch 是否 ahead / behind；
- 哪些 branch/worktree 包含本轮成果。

不要只检查当前 shell 所在 worktree。

---

## 第二阶段：识别本轮真正需要保存的成果

检查当前以及本轮使用过的 task branch / worktree。

使用：

```powershell
git status --short
git diff --stat
git diff --name-only
git diff --cached --name-only
```

必要时比较：

```powershell
git log --oneline origin/<default>..<task-branch>
git diff origin/<default>...<task-branch>
```

区分：

1. 本轮产生、应该进入正式结果的修改；
2. 用户在本轮开始前已经存在的修改；
3. 与当前任务无关、来源不确定的修改。

### 原则

本轮明确产生的成果必须收进去。

用户原本存在或来源不明的修改不得擅自覆盖、删除或混入提交。

如果发现无法判断归属的重要 dirty 内容：

- 不得通过 reset、clean、checkout、restore 等方式强行制造 clean；
- 先完成其他安全检查；
- 然后向用户说明文件和风险，请求决策。

---

## 第三阶段：完成遗漏提交

如果本轮成果仍未提交：

先检查 diff：

```powershell
git diff --check
git diff
```

只暂存确认属于本轮成果的文件。

优先：

```powershell
git add -- <明确文件路径>
```

避免无脑：

```powershell
git add .
git add -A
```

除非已经确认当前所有变化全部属于同一个任务。

提交前检查：

```powershell
git diff --cached --check
git diff --cached --stat
git diff --cached
```

确认无误后创建符合仓库已有风格的 commit。

不得为了“工作区干净”把无关修改塞入 commit。

不得创建无意义空 commit。

---

## 第四阶段：保存任务分支成果

如果成果位于非默认分支：

确保该分支本轮需要保存的 commit 已经存在。

必要时先 push：

```powershell
git push
```

没有 upstream 时：

```powershell
git push -u origin <task-branch>
```

这一步的作用是确保成果已经有远端副本。

但：

> **task branch push 成功只是中间状态，不代表已经收工。**

接下来仍然必须确认这些 commit 是否已经进入默认分支。

---

## 第五阶段：把成果真正整合进默认分支

如果本轮工作不在默认分支，必须继续处理。

先确认：

```powershell
git merge-base --is-ancestor <task-branch> <default-branch>
```

如果返回成功，说明任务成果已经包含在默认分支。

如果没有包含，则必须按照仓库现有工作流完成 integration。

### 优先遵循仓库既有方式

检查：

- CONTRIBUTING；
- AGENTS.md；
- README；
- GitHub Actions；
- 最近 merge history；
- 是否存在 branch protection；
- 项目通常使用 merge、squash 还是 rebase；
- 是否要求 Pull Request。

不要擅自改变仓库已有协作模式。

### 如果允许直接集成

在确认默认分支工作区干净后：

先同步远端：

```powershell
git fetch origin
```

让本地默认分支首先追上远端。

优先使用不会产生历史重写的方式。

如果可以 fast-forward，则优先 fast-forward。

需要正常 merge 且符合仓库惯例时，可以创建 merge commit。

如果需要在 merge、rebase、cherry-pick 等多个策略中做会影响历史结构的重要选择，且仓库惯例无法明确答案，应询问用户。

### 如果仓库要求 Pull Request

如果已有 PR：

继续检查其状态，并在权限、检查结果和仓库规则允许的情况下完成 merge。

如果没有 PR，而仓库明确要求 PR：

可以创建 PR，并在检查通过且权限允许时完成 merge。

**不能因为“已经创建 PR”就提前宣布收工。**

如果 CI、review、branch protection 或权限阻止 merge：

- 先处理属于本轮且低风险、结论明确的问题；
- 如果需要用户批准、review、权限操作或存在高风险决策，则询问用户；
- 在阻塞解除前不得称“可以收工”。

### 冲突

如果 integration 出现 Git conflict：

只对本轮明确修改、且语义明确的冲突进行安全解决。

如果冲突涉及：

- 用户未知改动；
- 无法判断业务语义；
- 可能导致成果丢失；
- 两个独立任务互相覆盖；
- 两种实现都合理，需要产品选择；

不得为了强行收工而猜测。

保留当前安全状态并询问用户。

---

## 第六阶段：确保远端默认分支最新

成果进入本地默认分支后，检查：

```powershell
git status --branch --short
git log -1 --oneline
```

然后把正式结果同步到远端。

如果默认分支允许直接 push：

```powershell
git push
```

如果结果已经通过 PR 合并到远端，则重新：

```powershell
git fetch origin
```

然后让本地默认分支安全 fast-forward 到远端最新状态。

不得使用：

```powershell
git push --force
git push --force-with-lease
```

来强行完成收工，除非用户针对当前具体情况明确授权。

---

## 第七阶段：回到本地正式终态

最终必须检查真正的默认分支，而不是停留在 task branch。

如果存在主 worktree，应在主 worktree 中完成最终检查。

执行：

```powershell
git fetch origin
git status --branch --short
git status --short
git diff --name-only
git diff --cached --name-only
git ls-files --others --exclude-standard
git log -1 --oneline
git rev-parse HEAD
git rev-parse origin/<default>
git rev-list --left-right --count HEAD...origin/<default>
```

预期：

```text
git status --short
```

无输出。

```text
git diff --name-only
```

无输出。

```text
git diff --cached --name-only
```

无输出。

```text
git ls-files --others --exclude-standard
```

无输出。

```text
git rev-list --left-right --count HEAD...origin/<default>
```

必须为：

```text
0    0
```

并确认：

```text
git rev-parse HEAD
```

与：

```text
git rev-parse origin/<default>
```

完全一致。

---

## 第八阶段：检查 task branch / worktree 是否还有悬空成果

对本轮实际使用的其他 task branch/worktree 检查：

```powershell
git status --short
```

并确认：

```powershell
git merge-base --is-ancestor <task-branch> <default-branch>
```

本轮最终成果不得只存在于 task branch。

如果 task branch 仍然存在独有 commit：

```powershell
git log <default-branch>..<task-branch>
```

必须检查这些 commit：

- 如果属于本轮正式成果 → 尚未收工，继续 integration；
- 如果明确已经 superseded / abandoned → 记录原因；
- 如果来源不明 → 不擅自删除或忽略，询问用户。

不要因为默认分支 clean 就忽略另一个 worktree 中尚未收回的正式成果。

---

## AGENTS.md / 长期上下文检查

Git 收尾过程中顺便判断，本轮是否产生了未来 Agent 必须知道的长期变化，例如：

- 架构变化；
- 项目定位变化；
- 新的重要开发规则；
- 命令或目录结构变化；
- rename / path change；
- 明确长期用户要求；
- Rejected / Superseded 方向；
- 安全或提交规范变化。

只有确实需要时才更新根目录 `AGENTS.md` 或对应长期知识文档。

普通：

- bug fix；
- 小功能；
- 样式修改；
- 普通文案；
- 单次实验；
- 普通 commit；

不因为“收工”自动写入长期上下文。

如果需要更新，则把它视为本轮成果的一部分：

```text
修改 → 检查 → commit → integrate → push
```

不能让 AGENTS.md 更新反而成为新的未提交状态。

如果长期上下文的更新位置、范围或内容存在重大歧义，不要凭空决定，询问用户。

---

## 禁止事项

不得为了制造“看起来已经收工”的状态而擅自执行：

```powershell
git reset --hard
git clean -fd
git checkout -- .
git restore .
git push --force
git push --force-with-lease
```

以上操作只有在用户针对当前具体情况明确批准后，且已经说明风险，才可以执行。

不得：

- 删除来源不明的用户修改；
- 覆盖用户已有 commit；
- 把无关 dirty files 混入本轮 commit；
- 删除用途未知的 branch；
- 删除用途未知的 worktree；
- 擅自修改真实 credentials；
- 擅自部署；
- 擅自发布 Release；
- 擅自修改数据库；
- 绕过 branch protection；
- 使用 `--no-verify` 绕过仓库已有提交检查；
- 为了得到 clean 状态而隐藏、丢弃或忽略真实问题。

如果正常 Git 收尾能够安全继续完成，就继续执行，不要因为需要切换 worktree、同步 branch、push 或正常 integration 就提前停下。

---

## 特殊情况

### 当前位于 detached HEAD

如果 detached HEAD 上存在本轮需要保留的 commit 或修改：

先创建明确命名的 branch 保存成果，再继续正常：

```text
commit → push → integrate → default branch
```

不得让正式成果留在 detached HEAD。

如果 detached HEAD 上的内容与其他 branch 的关系无法确定，则先保存，不丢弃，并询问用户。

### 默认分支落后远端

在默认分支 clean 且不存在本地独有成果的前提下，安全 fast-forward。

不要产生无意义 merge commit。

### 默认分支领先远端

确认这些 commits 属于正式成果后 push。

### 默认分支与远端发生 divergence

先检查双方 commit。

不要为了追求 `0 0` 使用 hard reset 或 force push。

如果仓库历史能够明确给出正常 integration 方式，则按既有方式处理。

如果存在多种合理策略或可能影响他人成果，询问用户。

### 存在多个 worktree

必须知道本轮成果到底在哪一个 worktree。

最终验证不能只验证当前 worktree。

至少检查：

- 主 worktree；
- 本轮任务 worktree。

如果多个 worktree 都存在本轮相关 dirty 修改且关系不清楚，先保存当前状态，不覆盖，并询问用户。

### 存在用户原本的未提交修改

不得擅自提交、删除、stash、reset 或覆盖。

如果它们不妨碍本轮成果安全保存和远端同步，可以先完成其他独立工作。

如果这些修改导致无法得到干净的正式默认分支，则向用户说明具体文件和原因，请求决定如何处理。

### 需要 stash

默认不要使用 `git stash` 来“暂时藏起来然后宣布 clean”。

只有当：

- stash 确实是完成安全切换/集成的最佳方案；
- stash 内容归属明确；
- 不会造成用户忘记恢复的隐藏状态；

才可以考虑。

如果 stash 的内容属于用户原有修改或来源不明，应先询问用户。

最终收工时不得遗留本轮产生但未处理的 stash。

---

## 最终判定标准

只有以下全部成立才能输出：

# 可以收工

- 本轮成果全部 commit；
- 本轮需要保存的 commit 已有远端副本；
- 本轮正式成果已经进入默认分支；
- 远端默认分支已经包含最终成果；
- 本地默认分支已经同步到远端最新状态；
- 本地默认分支 HEAD = `origin/<default>`；
- ahead / behind = `0 0`；
- 默认分支没有 modified；
- 没有 staged；
- 没有 unstaged；
- 没有 untracked；
- 本轮 task branch / worktree 不存在尚未整合的正式成果；
- 没有已知的“只存在于临时位置”的本轮工作；
- 没有被隐藏在 stash 中的本轮待处理成果；
- 没有尚待用户决定的高风险收尾问题。

以下任何情况存在，都不能称为“可以收工”：

- 只 commit 没 push；
- 只 push task branch；
- 只创建 PR 没 merge；
- worktree 中仍有本轮未提交修改；
- task branch 还有应该进入默认分支的独有 commit；
- 本地默认分支落后远端；
- 本地默认分支领先但未 push；
- 默认分支和远端 divergence；
- 默认分支 dirty；
- 存在来源不明且无法安全处理的修改；
- integration conflict 未解决；
- CI / branch protection / 权限阻止最终合并；
- 存在需要用户决定但尚未确认的高风险操作。

---

## 最终报告

报告保持简短，只说明实际结果。

至少包含：

**成果**
- 本轮最终成果位于哪个 commit；
- 是否已经进入默认分支。

**Git**
- 默认分支名称；
- 最终 HEAD；
- 远端 HEAD；
- ahead / behind。

**远端**
- task branch 是否已 push；
- 最终成果是否已进入远端默认分支。

**工作区**
- 默认分支是否 clean；
- 是否存在 staged / unstaged / untracked；
- 本轮 worktree 是否还有悬空成果；
- 是否存在本轮遗留 stash。

**结论**

只能选择：

- **可以收工**
- **等待用户决策**
- **不能收工**

### 可以收工

全部终态条件均满足。

### 等待用户决策

成果已经尽可能安全保存，但存在高风险、不可逆、归属不明或需要用户选择的问题。

必须说明：

- 当前成果安全保存在哪里；
- 已完成到哪一步；
- 卡在哪一步；
- 用户需要决定什么；
- 决定后下一步会做什么。

### 不能收工

存在无法通过当前权限、仓库规则或正常安全操作继续解决的客观阻塞。

必须说明直接原因以及成果目前安全保存在哪里。

---

## 最重要的原则

> **收工的目标不是“看起来干净”，而是“成果已经安全落地、正式分支已经同步、没有悬空工作，并且没有通过破坏或隐藏用户成果来制造 clean 状态”。**

> **能安全自行完成的就继续完成；真正需要用户承担风险或做选择的，就明确询问用户。**

> **不要把中间状态包装成“收工完成”。**
