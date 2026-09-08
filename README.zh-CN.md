# AI Prompt Protocols

*在真实 AI 辅助开发中打磨出来的实用提示词、编码代理规则与工作流协议。*

[English](README.md) | 简体中文

---

本仓库是一份小而精的纯文本 Markdown 提示词集合，服务于 AI 辅助开发。每份文档都是完整、自洽的提示词——复制它、放进你的代理上下文、直接使用。

本页是 [README.md](README.md) 的中文镜像，信息架构、锚点与链接结构保持一致。两份英文规则附带中文译文（[`translations/zh-CN/agent-rules/`](translations/zh-CN/agent-rules/)）；收工协议本身即以中文撰写，UI 工作流、代码审查与代理初始化协议均为英文。提示词的生命周期与维护规则见 [MAINTENANCE.md](MAINTENANCE.md)。

## 协议选择（Choose a Protocol）

| 协议 | 是什么 | 适合场景 | 语言 |
| --- | --- | --- | --- |
| [Codex Global Rules](#codex-global-rules) | 面向可靠、低噪音 Codex 编码会话的全局行为规则 | 统一 Codex 工作流基线 | English · 中文译文 |
| [Universal Coding Agent Global Rules](#universal-coding-agent-global-rules) | 同一套核心规则的工具无关版本 | 混合使用多种代理的团队（Codex、Claude Code、Cursor 等） | English · 中文译文 |
| [UI Screenshot → Implementation Spec Protocol](#ui-screenshot--implementation-spec-protocol) | 将截图 / 设计稿转化为精确、可执行的 UI 实现规格 | 实现之前：先分析、后输出规格 | English |
| [UI Visual Fidelity Refinement Protocol](#ui-visual-fidelity-refinement-protocol) | 通过「渲染 → 对比 → 修复」闭环让实现向参考截图收敛 | 实现之后：视觉收敛阶段 | English |
| [Project Closeout Prompt](#project-closeout-prompt) | 将本轮成果安全收敛进默认分支并同步远端 | 里程碑结束、发布或交接时 | 中文 |
| [Expert Code Review Protocol](#expert-code-review-protocol) | 高信号代码审查：只报告真实、可操作、与合并相关的发现 | 合并前审查 PR / diff | English |
| [Universal Agent Init](#universal-agent-init) | 依据仓库证据初始化或改进 `AGENTS.md` / `CLAUDE.md` | 初始化或梳理代理指令 | English |

## 协议库（Prompt Library）

### Codex Global Rules

**Codex 全局规则。**（英文原文 · [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md)）

**它做什么。** 一套面向可靠、低噪音 Codex 编码会话的全局行为规则：如何提问、改多少、什么可以删、怎样才算完成、如何汇报。

**为什么有效。**

- **可逆假设优先于提问。** 只有当缺失信息可能导致数据丢失、不可逆结局、结果实质性改变或需要用户真实选择时，才提出澄清问题；否则选择最安全且可逆的假设并继续。
- **最小必要改动。** 先复用现有代码、架构与约定，再考虑新抽象、依赖或基础设施；不添加未要求的功能、面向未来的设计或无关重构。
- **删除黑名单。** `del /s`、`rd /s`、`rmdir /s`、`Remove-Item -Recurse`、`rm -rf`、`git clean -fd/-fdx` 等破坏性一次性清理命令一律禁用；只删除被明确识别、用途与安全性均已验证的路径。
- **外部副作用闸门。** 推送、合并、部署、发布、更新依赖与修改远端配置，均需用户明确授权，或经由一个声明目的包含该动作且被有意调用的工作流授权。
- **验证后再声称成功。** 非平凡改动前先定义成功标准，检查最终 diff，只运行与改动相关的检查，绝不声称未实际执行的验证。
- **工具批处理。** 环境支持时，在同一有界阶段内批量执行相互独立的只读检查；有依赖、冲突、审批或破坏性的步骤保持串行。

**适合场景。** 长时间运行的 Codex 会话，以及希望统一代理行为基线的仓库。

**配套工具。** Codex（为其 Code Mode 工具调用设计）；思路同样适用于类似的可调用工具代理。

**阅读。** [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md) · [英文原文](agent-rules/codex-global-rules.md)

<details>
<summary>预览</summary>

```text
* 只有在缺失信息可能实质性地改变结果、导致数据丢失、造成不可逆结局或需要用户真实选择时，才提出澄清问题。否则选择最安全且可逆的假设，在相关时说明该假设，然后继续。
* 绝不执行范围宽泛或范围不明确的删除操作。不要使用 `rm -rf`、`Remove-Item -Recurse`、`git clean -fd/-fdx`、`del /s`、`rd /s` 或 `rmdir /s` 等破坏性一次性清理命令。只删除用途与安全性都已逐项验证过的明确路径。
* 只运行与本次改动相关的检查。不要仅仅为了显得彻底而运行宽泛或昂贵的验证。
```

</details>

### Universal Coding Agent Global Rules

**Universal Coding Agent Global Rules。**（英文原文 · [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md)）

**它做什么。** 同一套核心行为规则，去掉了 Codex 专属工具细节，适用于多种编码代理：Codex、Claude Code、Cursor、Gemini CLI 及类似工具。

**为什么有效。**

- **同样的核心规则。** 行为契约——可逆假设、最小改动、删除安全、外部副作用闸门、验证——与 Codex 版本完全一致。
- **唯一的实质差异。** 只有 Tool Efficiency 一节不同：用工具无关的「在支持时批量执行独立检查」措辞替代 Codex 专属表述。
- **可选个人收藏。** 为「用户整理的可复用收藏」提供一个通用、可选的挂载点；它永不被当作项目记忆、也不会默认扫描，路径是占位符而非机器特定值。
- **按环境选用。** Codex 会话用 Codex 版本，其他代理用通用版本，而不是维护两套互相偏离的哲学。
- **小且易同步。** 两份短规则文件，而不是两个庞大的提示词体系；便于审查与同步。

**适合场景。** 在多款代理之间切换的个人或团队，或想要一套任何编码代理都能加载的规则。

**配套工具。** Codex、Claude Code、Cursor、Gemini CLI 及类似编码代理。

**阅读。** [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md) · [英文原文](agent-rules/universal-coding-agent-global-rules.md)

<details>
<summary>预览</summary>

```text
* 只有在缺失信息可能实质性地改变结果、导致数据丢失、造成不可逆结局或需要用户真实选择时，才提出澄清问题。否则选择最安全且可逆的假设，在相关时说明该假设，然后继续。
* 绝不执行范围宽泛或范围不明确的删除操作。避免破坏性一次性清理命令，只删除用途与安全性都已逐项验证过的明确路径。

当环境支持并行执行时，在同一有界阶段内批量执行相互独立的检查或其他无冲突操作。
```

</details>

### UI Screenshot → Implementation Spec Protocol

**UI 截图 → 实现规格协议。**（英文原文）

**它做什么。** 将一张或多张参考截图 / 设计稿转化为一份简洁、结构化、可直接实现的 UI Implementation Specification——构建能解释截图的最小精确视觉模型，足以据此复现页面。代理的职责是「检查 → 建模 → 输出规格」，绝不直接实现。

**为什么有效。**

- **事实来源层级。** 证据按优先级排序：显式用户需求 → 参考截图 → 多截图一致证据 → 已验证的项目资产 / Token → 合理测量或估算 → 推断；除非用户另有说明，截图就是视觉事实来源。
- **三类证据标签。** 未直接确立的信息标注 ESTIMATED（结构化格式 `Estimated width: ~240px / Confidence: high`）、INFERRED 或 UNKNOWN——普通观察不重复打标，不确定性绝不伪装成精确。
- **反幻觉规则。** 明确的「禁止臆造」清单（文字、对话框、交互、响应式状态、字体、颜色、新页面、品牌、Token），且可识别的产品记忆永远不能凌驾于所提供的截图之上。
- **先全局后局部。** 11 步分析顺序：画布与视口 → 全局构图 → 主要页面区域 → 布局关系 → 排版 / 设计系统模式 → 可复用组件 → 重要可见内容 → 重要资源 → 可见状态 → 响应式证据 → 实现关键约束。
- **关系优先于坐标。** 用容器关系、比例与间距节奏描述布局，偏好小间距系统而非几十个孤立测量值。
- **保真优先级与交接。** 需求按 CRITICAL / IMPORTANT / COSMETIC 分级，输出以 Material Uncertainty 与 Implementation Directives 收尾，实现方不会在关键失配未解决时去打磨细节。

**适合场景。** 截图重建、设计转代码交接，以及任何不希望实现方重新猜测视觉设计的任务。

**配套工具。** 规格输出由 Codex、Claude Code、Cursor、Gemini CLI 等编码代理消费。

**阅读。** [ui-screenshot-to-implementation-spec.md](ui-workflows/ui-screenshot-to-implementation-spec.md)（英文原文）

<details>
<summary>预览</summary>

````text
### ESTIMATED
Use when a value can only be approximated.

Example:

```text
Estimated width: ~240px
Confidence: high
```

### INFERRED
Use for behavior or structure that is not directly visible but is reasonably supported.

### UNKNOWN
Use when the available evidence is insufficient.

Do not disguise uncertainty as precision.
````

</details>

### UI Visual Fidelity Refinement Protocol

**UI 视觉保真收敛协议。**（英文原文）

**它做什么。** 将实际渲染界面与参考截图对比，识别有意义的差异、修复其根本原因，并验证收敛——同时不破坏功能或可维护性。

**为什么有效。**

- **渲染即证据。** 协议核心规则是「The implementation is a hypothesis. The rendered page is evidence.」——正确性只看渲染结果，绝不单凭 CSS 值或 DOM 结构下结论。
- **诚实的工具可用性门槛。** 开始前先确认环境是否真的能渲染、捕获并对比：Full Visual Mode 运行完整迭代闭环；Limited Verification Mode 只做有证据支撑的代码级收敛，且必须报告 `VISUAL VERIFICATION REQUIRED`。
- **稳定环境，基线先行。** 先固定参考尺寸、视口、缩放、DPR、路由、滚动、状态与字体/资源就绪条件，再采集基线渲染与排序后的差异清单——不要一开始就随机改 CSS。
- **先全局后局部的层级。** 对比按 V0 结构 → V1 主要视觉影响 → V2 明显 → V3 细节执行；高层级仍错误时禁止打磨 V3。
- **先找根本原因。** 先检查共享原因（容器宽度、布局模型、Token、继承样式、断点），再修补症状；每个变更簇只对应一个连贯假设，不堆叠补偿性 hack。
- **验收门槛与停止条件。** 每个变更簇标注 IMPROVED / NEUTRAL / REGRESSED，若导致更高优先级区域回归则拒绝；Full Visual Mode 下仅当 V0 = 0、V1 = 0，且完成全新终稿对比时才停止。

**适合场景。** UI 依据规格实现完成后的视觉收敛阶段。

**配套工具。** UI 实现代理：Codex、Claude Code、Cursor 等。

**阅读。** [ui-visual-fidelity-refinement.md](ui-workflows/ui-visual-fidelity-refinement.md)（英文原文）

<details>
<summary>预览</summary>

```text
V0 — Structure
V1 — Major Visual Impact
V2 — Noticeable
V3 — Cosmetic

CAPTURE / OBSERVE
↓
COMPARE
↓
RANK
↓
IDENTIFY ROOT CAUSE
↓
PATCH ONE COHERENT CLUSTER
↓
RENDER AGAIN
↓
ACCEPT / ADJUST / REVERT
↓
REPEAT
```

</details>

### Project Closeout Prompt

**项目收工提示词。**（中文原文）

**它做什么。** 一套 Git 收工 / 交接协议：当用户明确说「收工」时，将本轮成果安全收敛到仓库既有集成工作流允许的最终状态——提交、推送、同步并保持干净，且遵循该仓库真实模型（直推默认分支 / 功能分支集成 / PR 流程），无论成果原本在哪里。

**为什么有效。**

- **先识别集成模型。** 协议先判断仓库实际如何集成成果——direct-to-default、功能分支 + 集成、或受保护 / PR 流程——而不是把 `main` + 直推模型强加给每个仓库。
- **定义终点而非动作清单。** 「完成」由仓库集成模型定义：对直推仓库是成果完整提交、推送、本地与远端默认分支同步、工作区干净；对 PR 仓库是成果已推送并达到可建 PR 的状态，剩余审批步骤记为 `Remaining integration step`。
- **授权边界。** 调用本协议授权正常、低风险、task-owned 的 Git 收尾（状态 / diff 检查、fetch、commit、push、安全本地集成）；force push、改写历史、丢弃用户修改与任何外部副作用需要额外明确授权。
- **升级规则。** 分叉、merge/rebase/reset/force 选择与业务语义冲突先暂停并向用户询问；普通无歧义的机械冲突可以安全处理。
- **多仓库支持。** 每个仓库独立检查；未被本轮触及的仓库只做只读验证，绝不制造无意义提交。
- **可验证的最终状态。** 用 `git status`、`git rev-list --left-right --count HEAD...<upstream>` 与最终 diff 检查逐一验证完成。

**适合场景。** 里程碑结束、发布或交接时——「完成」必须名副其实，且符合仓库自己的工作流。

**配套工具。** 能执行 Git 命令的编码代理（Codex、Claude Code、Cursor 等）。

**阅读。** [project-closeout.md](project-workflows/project-closeout.md)（中文原文）

<details>
<summary>预览</summary>

```text
收工的判断标准不是“执行过 commit / push”。

真正目标是：

> **本轮需要保留的成果已经被安全保存，并达到当前仓库正式工作流允许的最终状态，没有遗留本轮未处理工作。**

仓库的正式 integration model 可能不同。

先识别，再执行。
```

</details>

### Expert Code Review Protocol

**专家代码审查协议。**（英文原文）

**它做什么。** 一份高信号代码审查协议：像决定「这段代码是否安全、适合合并进生产环境」一样审查代码，优先真实、可操作、与合并相关的问题，同时尽量减少猜测与低价值噪音。

**为什么有效。**

- **七条入选标准。** 只有当问题有证据支撑、独立、可操作、实质性相关、具体到能修复、不是纯主观、也不是重复发现时，才予以报告——宁可没有发现，也不报投机性发现。
- **置信度门槛。** 每条发现附带 0.0–1.0 置信度；审查者必须先主动尝试推翻每条发现，通常只报告置信度 >= 0.80 的已确认发现；严重但证据不足的担忧放入 `Needs Verification`，不作为已确认 bug。
- **风险优先的审查顺序。** 从最高层风险向下审查：意图 / 设计 → 正确性 / 可靠性 → 安全 → 性能 / 资源 → 可维护性 → 测试 / 契约。
- **定向验证。** 有仓库与工具权限时，可用小而相关、低风险的检查（聚焦测试、定向复现、窄范围 typecheck/lint、只读调用点检查）提升或降低某条发现的置信度——绝不为了显得彻底而跑宽泛套件。
- **严重度体系。** P0（立即阻塞）到 P3（非阻塞改进），每级都有具体示例。
- **明确排除低价值评论。** 琐碎格式、主观风格、泛泛的「补测试」评论、理论性风险，除非明确要求，一律不报。
- **诚实的结论。** 审查以 APPROVE / CHANGES REQUESTED / NEEDS CONTEXT 三选一收尾——绝不声称运行过实际没有运行的测试、构建或 linter。

**适合场景。** 合并前的 PR / diff 审查、安全敏感审查，以及受够了低信号审查噪音的团队。

**配套工具。** 被要求审查代码的编码代理（Codex、Claude Code、Cursor 等）；也适用于聊天或代理审查工作流。

**阅读。** [expert-code-review.md](review-workflows/expert-code-review.md)（英文原文）

<details>
<summary>预览</summary>

```text
Report an issue only when it is:

1. evidence-supported
2. discrete
3. actionable
4. materially relevant
5. specific enough to fix
6. not merely subjective
7. not a duplicate

Prefer no finding over a speculative finding.

Normally report confirmed findings only when confidence is at least 0.80.

A potentially serious concern with insufficient evidence belongs under `Needs Verification`, not as a confirmed bug.

Never present speculation as fact.
```

</details>

### Universal Agent Init

**通用代理初始化。**（英文原文）

**它做什么。** 初始化或改进仓库的持久 AI 代理指令：一份精简、准确的根级 `AGENTS.md` 作为共享指引，外加根级 `CLAUDE.md` 作为 Claude Code 入口——内容以仓库证据为准，并在提交前验证。

**为什么有效。**

- **证据而非臆造。** 每一条新增路径、命令与约定都必须对照仓库验证；禁止臆造命令、策略或架构，绝不把个人 / 机器本地 / 全局指令文件复制进提交内容。
- **刻意精简。** 简单仓库目标约 200–400 词：持久指令会消耗模型上下文，泛泛的「写好代码」类建议（除非有具体的仓库含义）一律排除。
- **单一事实来源。** `CLAUDE.md` 通常只包含 `@AGENTS.md` 一行导入；共享指引只存一处，不重复。
- **保守处理。** 现有指令文件绝不盲目覆盖；最小有效 diff；识别并报告 `AGENTS.override.md` 的优先级。
- **验证与安全提交。** 复查最终文件、逐条验证路径与命令、检查 diff（`git diff --check`）、扫描密钥与私有路径，并且只用显式 pathspec 提交本任务文件——未经明确要求绝不推送或开 PR。

**适合场景。** 在新仓库初始化代理指令，或在保留用户成果的前提下梳理已有的 `AGENTS.md` / `CLAUDE.md`。

**配套工具。** 拥有仓库访问权的编码代理：Codex（通过 `AGENTS.md`）、Claude Code（通过 `CLAUDE.md`）、Cursor 等。

**阅读。** [universal-agent-init.md](init-workflows/universal-agent-init.md)（英文原文）

<details>
<summary>预览</summary>

````text
If no root `CLAUDE.md` exists, normally create exactly:

```md
@AGENTS.md
```

Persistent instructions consume model context. Do not add generic advice such as “write clean code,” “use best practices,” “test thoroughly,” or “avoid bugs” unless it has concrete repository-specific meaning.
````

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
- **合并之前** → 合并前对 PR 或 diff 运行 [Expert Code Review](#expert-code-review-protocol)。
- **新仓库** → 运行 [Universal Agent Init](#universal-agent-init) 初始化 `AGENTS.md` / `CLAUDE.md`，再开始长时间会话。

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
├── MAINTENANCE.md
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
├── review-workflows/
│   └── expert-code-review.md
├── init-workflows/
│   └── universal-agent-init.md
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
- 提示词生命周期与维护改进（见 [MAINTENANCE.md](MAINTENANCE.md)）
- 真实世界的使用反馈

请勿提交批量生成或未经验证的提示词列表。如果你的提示词经受住了真实项目的考验，请说明它在哪里、如何被使用。

## 许可证（License）

[MIT](LICENSE)
