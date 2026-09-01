# AI Prompt Protocols

*在真实 AI 辅助开发中打磨出来的实用提示词、编码代理规则与工作流协议。*

[English](README.md) | 简体中文

---

本仓库是一份小而精的纯文本 Markdown 提示词集合，服务于 AI 辅助开发。每份文档都是完整、自洽的提示词——复制它、放进你的代理上下文、直接使用。

本页是 [README.md](README.md) 的中文镜像，信息架构、锚点与链接结构保持一致。两份英文规则附带中文译文（[`translations/zh-CN/agent-rules/`](translations/zh-CN/agent-rules/)）；三份工作流协议本身即以中文撰写。

## 协议选择（Choose a Protocol）

| 协议 | 是什么 | 适合场景 | 语言 |
| --- | --- | --- | --- |
| [Codex Global Rules](#codex-global-rules) | 面向长时间 Codex 编码会话的全局行为规则 | 统一 Codex 工作流基线 | English · 中文译文 |
| [Universal Coding Agent Global Rules](#universal-coding-agent-global-rules) | 同一套核心规则的工具无关版本 | 混合使用多种代理的团队（Codex、Claude Code、Cursor 等） | English · 中文译文 |
| [UI Screenshot → Implementation Spec Protocol](#ui-screenshot--implementation-spec-protocol) | 将截图 / 设计稿转化为精确、可执行的 UI 实现规格 | 实现之前：先分析、后输出规格 | 中文 |
| [UI Visual Fidelity Refinement Protocol](#ui-visual-fidelity-refinement-protocol) | 通过「渲染 → 对比 → 修复」闭环让实现向参考截图收敛 | 实现之后：视觉收敛阶段 | 中文 |
| [Project Closeout Prompt](#project-closeout-prompt) | 将本轮成果安全收敛进默认分支并同步远端 | 里程碑结束、发布或交接时 | 中文 |

## 协议库（Prompt Library）

### Codex Global Rules

**Codex 全局规则。**（英文原文 · [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md)）

**它做什么。** 一套面向长时间 Codex 编码会话的全局行为规则：如何提问、改多少、什么可以删、怎样才算完成、如何汇报。

**为什么有效。**

- **可逆假设优先于提问。** 只有当缺失信息可能导致数据丢失、不可逆决策或结果实质性改变时，才提出澄清问题；否则选择最安全且可逆的假设并继续。
- **最小必要改动。** 先复用现有代码与模式，再考虑新抽象、依赖或基础设施；不添加未要求的功能，不做无关重构。
- **删除黑名单。** `del /s`、`rd /s`、`rmdir /s`、`Remove-Item -Recurse`、`rm -rf`、`git clean -fd/-fdx` 一律禁用；只删除被明确授权、逐项验证过的路径。
- **验证后再声称成功。** 非平凡改动前先定义成功标准，检查最终 diff，只运行与改动相关的检查。
- **工具批处理。** Code Mode 下，把相互独立、可经由 functions.exec 执行的调用合并到同一次调用中，按 `Promise.allSettled` / `Promise.all` 语义处理。

**适合场景。** 长时间运行的 Codex 会话，以及希望统一代理行为基线的仓库。

**配套工具。** Codex（为其 Code Mode 工具调用设计）；思路同样适用于类似的可调用工具代理。

**阅读。** [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md) · [英文原文](agent-rules/codex-global-rules.md)

<details>
<summary>预览</summary>

```text
* 只有在缺失信息可能实质性地改变结果、导致数据丢失或造成不可逆决策时，才提出澄清问题。否则选择最安全且可逆的假设，在相关时说明该假设，然后继续。
* 绝不执行范围宽泛或范围不明确的删除操作。不要使用 `del /s`、`rd /s`、`rmdir /s`、`Remove-Item -Recurse`、`rm -rf` 或 `git clean -fd/-fdx`。只删除被明确授权、且已逐项验证过的路径。
* 只运行与本次改动相关的检查。在声称成功之前，检查最终 diff 并核实结果。
```

</details>

### Universal Coding Agent Global Rules

**Universal Coding Agent Global Rules。**（英文原文 · [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md)）

**它做什么。** 同一套核心行为规则，去掉了 Codex 专属工具细节，适用于多种编码代理：Codex、Claude Code、Cursor 及类似工具。

**为什么有效。**

- **同样的 12 条核心规则。** 行为契约——可逆假设、最小改动、删除安全、验证——与 Codex 版本完全一致。
- **唯一的实质差异。** 只有 Tool Efficiency 一节不同：用通用的「在支持时并发运行独立检查」替代 Codex 的 functions.exec 细节。
- **按环境选用。** Codex 会话用 Codex 版本，其他代理用通用版本，而不是维护两套互相偏离的哲学。
- **小且易同步。** 两份短规则文件，而不是两个庞大的提示词体系；便于审查与同步。

**适合场景。** 在多款代理之间切换的个人或团队，或想要一套任何编码代理都能加载的规则。

**配套工具。** Codex、Claude Code、Cursor 及类似编码代理。

**阅读。** [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md) · [英文原文](agent-rules/universal-coding-agent-global-rules.md)

<details>
<summary>预览</summary>

```text
* 只有在缺失信息可能实质性地改变结果、导致数据丢失或造成不可逆决策时，才提出澄清问题。否则选择最安全且可逆的假设，在相关时说明该假设，然后继续。
* 绝不执行范围宽泛或范围不明确的删除操作。不要使用 `del /s`、`rd /s`、`rmdir /s`、`Remove-Item -Recurse`、`rm -rf` 或 `git clean -fd/-fdx`。只删除被明确授权、且已逐项验证过的路径。

在每个有界的阶段内，若工具支持，并发运行相互独立的检查或工具调用。将不依赖中间结果的工作合并执行，并检查所有返回的结果。
```

</details>

### UI Screenshot → Implementation Spec Protocol

**UI 截图 → 实现规格协议。**（中文原文）

**它做什么。** 将 UI 截图、设计稿或页面截图，转化为一份结构化、可执行的 UI 实现规格，让另一个编码代理无需再看截图即可据此实现页面。

**为什么有效。**

- **三层确定性标注。** 规格中的每一项都标注为 OBSERVED（可直接确认的事实）、ESTIMATED（近似值，用 `~` 标记）或 INFERRED（合理推断），读者始终清楚哪些是事实、哪些是推测。
- **反幻觉规则。** 无法确定时必须写 `Unknown` 或 `Inferred — confidence: low`——绝不根据记忆或对其他产品的印象补全。
- **先全局后局部。** 分析顺序为：画布与视口 → 空间地图 → 区块层级 → 布局架构 → 设计 Token → 排版 → 组件，避免遗漏、层级清晰。
- **关系优先于坐标。** 用元素与容器、相邻元素之间的关系来描述，而不是假装像素值精确。
- **明确的交接产物。** 输出格式以 Uncertainty Register（不确定项登记表）和 Implementation Directives（实现指令）收尾，实现方清楚哪些不确定、哪些必须做。

**适合场景。** 截图重建、设计转代码交接，以及任何不希望实现方重新猜测视觉设计的任务。

**配套工具。** 规格输出由 Codex、Claude Code、Cursor、Gemini CLI 等编码代理消费。

**阅读。** [ui-screenshot-to-implementation-spec.md](ui-workflows/ui-screenshot-to-implementation-spec.md)（中文原文）

<details>
<summary>预览</summary>

```text
必须明确区分以下三类信息：

### OBSERVED

可以直接从截图确认的事实。

### ESTIMATED

可以从截图合理估计，但无法直接获得精确值。

使用 `~` 表示估值。

例如：

`~24px`

而不是伪装成：

`24px`

### INFERRED

截图没有直接展示，但根据 UI 结构进行的合理推断。

所有 INFERRED 内容必须明确标记。

绝不能把推测写成截图事实。
```

</details>

### UI Visual Fidelity Refinement Protocol

**UI 视觉保真收敛协议。**（中文原文）

**它做什么。** 页面实现完成后，通过「渲染 → 截图 → 对比 → 修复」的闭环，让实现向参考截图持续收敛，直到剩余差异足够小、继续修改的收益明显降低。

**为什么有效。**

- **渲染即证据。** 协议禁止凭代码判断（如「CSS 已设为 24px，间距肯定正确」）——一切判断以真实浏览器渲染为准：**Implementation is hypothesis. Rendered screenshot is evidence.（实现是假设，渲染截图是证据。）**
- **固定对比环境。** 字体、加载与渲染条件被固定（例如截图前先等待字体加载完成），保证对比在同等条件下进行。
- **差异分类。** 每次发现的差异在改动代码前，先标注为 STRUCTURE / GEOMETRY / TYPOGRAPHY / COLOR / SURFACE / ASSET。
- **保留现有功能。** 这是视觉修整任务，不是架构重写：重写页面、大规模重构、修改业务逻辑 / API 一律禁止。
- **可验证的收敛。** 只有用真实差异清单证明收敛、并以真实截图为依据时，循环才会停止。

**适合场景。** UI 依据规格实现完成后的视觉收敛阶段。

**配套工具。** UI 实现代理：Codex、Claude Code、Cursor 等。

**阅读。** [ui-visual-fidelity-refinement.md](ui-workflows/ui-visual-fidelity-refinement.md)（中文原文）

<details>
<summary>预览</summary>

```text
任何视觉判断都必须以真实 Browser Render 为依据。

禁止：

> “CSS 已经设置为 24px，所以间距应该正确。”

必须：

> 截图确认真实渲染中的视觉间距是否与参考图一致。

**Implementation is hypothesis.  
Rendered screenshot is evidence.**
```

</details>

### Project Closeout Prompt

**项目收工提示词。**（中文原文）

**它做什么。** 一套 Git 收工 / 交接协议：当用户说「收工」时，无论本轮成果在哪里（主分支、功能分支、worktree、detached HEAD 或尚未推送的本地提交），都安全地将其收敛进仓库默认分支——完成提交、合并、推送、同步并保持工作区干净。

**为什么有效。**

- **定义终点而非动作清单。** 「完成」的定义是：成果完整提交、进入默认分支、已推送、本地与远端默认分支指向同一结果、工作区干净——而不只是「我提交并推送了」。
- **中间状态明确不算完成。** 把 feature branch / worktree branch 推到远端不算收工；创建了 PR 但未合并也不算完全收工。
- **升级规则。** 低风险步骤自动完成；真正有风险或不可逆的步骤——`git reset --hard`、force push——先暂停并向用户询问。
- **worktree / detached HEAD 感知。** 协议明确处理成果遗留在其他 worktree 或 detached HEAD 上的情况，禁止把本轮成果留在那里。
- **可验证的最终状态。** 具体完成检查是 `git rev-list --left-right --count HEAD...origin/<default>` 返回 `0 0`。

**适合场景。** 里程碑结束、发布或交接时——「完成」必须名副其实。

**配套工具。** 能执行 Git 命令的编码代理（Codex、Claude Code、Cursor 等）。

**阅读。** [project-closeout.md](project-workflows/project-closeout.md)（中文原文）

<details>
<summary>预览</summary>

```text
“收工”不是简单提交当前文件，也不是只把当前分支 push 到远端。

真正的目标是：

> **把本轮已经完成的成果从它实际所在的位置安全收敛到仓库的最终正式状态。**

**仅仅把 feature branch / worktree branch push 到远端，不算收工。**

**仅仅创建 PR 但尚未合并，也不算完全收工。**
```

</details>

## 推荐工作流（Recommended Workflow）

整个仓库围绕一条流水线组织：先规则、再项目上下文、然后是任务协议、验证、最后收工。

```text
Global Rules
     ↓
Project Context
     ↓
Task Protocol
     ↓
Verification
     ↓
Closeout
```

典型场景：

- **编码会话** → 将 [Global Rules](#codex-global-rules) 作为持久指令加载，然后直接工作。
- **截图重建** → 对截图运行 [Spec 协议](#ui-screenshot--implementation-spec-protocol) → 依据规格实现 → 运行 [视觉保真收敛](#ui-visual-fidelity-refinement-protocol) 让渲染向参考图收敛。
- **项目完成** → 运行 [收工协议](#project-closeout-prompt)，安全落地并同步成果。

## 理念（Philosophy）

很多提示词在纸面上很漂亮，一到真实工作就失效。本仓库只收录在真实项目中使用过、并随时间打磨过的文档：显式的行为约束，用真实渲染与真实 Git 历史验证，目标是减少代理漂移与擅自发挥。

> 验证过的工作流 > 巧妙的措辞

本仓库不做任何 benchmark 声明，也不承诺任何保证结果。唯一的证据是：这些文档在真实工作中使用过、并随时间迭代过——请亲自尝试，留下经得起你自己项目考验的部分。

这是一个小而精、高信噪比的纯文本知识仓库——不是提示词堆砌、市场、教程站或 SaaS 产品。

## 使用方法（Usage）

这些文档是纯文本提示词，有三种用法：

1. **作为上下文复制。** 把整份文档粘贴到聊天或代理会话中，作为初始指令。
2. **作为持久指令。** 如果产品原生支持全局规则文件，把规则放进去——Codex 读取 `AGENTS.md`，Claude Code 读取 `CLAUDE.md`。其他工具请查阅你的工具文档；除非产品文档明确支持原生机制，否则请把这些文档当作你提供的上下文。
3. **作为阶段协议。** Spec、Fidelity、Closeout 三份文档是阶段提示词：在正确的时机运行（实现前、视觉收敛时、项目收尾时），而不是作为常驻规则。

规则文档按「全局规则位」的用途编写，但工具是否原生读取此类文件是产品特性——使用前请查阅你的工具文档。

## 仓库结构（Repository Structure）

```text
.
├── README.md
├── README.zh-CN.md
├── LICENSE
├── .gitignore
├── agent-rules/
│   ├── codex-global-rules.md
│   └── universal-coding-agent-global-rules.md
├── ui-workflows/
│   ├── ui-screenshot-to-implementation-spec.md
│   └── ui-visual-fidelity-refinement.md
├── project-workflows/
│   └── project-closeout.md
└── translations/
    └── zh-CN/
        └── agent-rules/
            ├── codex-global-rules.md
            └── universal-coding-agent-global-rules.md
```

## 参与贡献（Contributing）

本仓库中的每份文档都在真实工作中使用过、并随时间迭代。

欢迎：

- bug 报告与歧义报告
- 对现有提示词的具体改进
- 中文文档的翻译修正
- 真实世界的使用反馈

请勿提交批量生成或未经验证的提示词列表。如果你的提示词经受住了真实项目的考验，请说明它在哪里、如何被使用。

## 许可证（License）

[MIT](LICENSE)
