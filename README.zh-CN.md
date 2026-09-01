# AI Prompt Protocols

*在真实 AI 辅助开发中打磨出来的实用提示词、编码代理规则与工作流协议。*

[English](README.md) | 简体中文

---

本仓库是一份小而精的纯文本 Markdown 提示词集合，服务于 AI 辅助开发。每份文档都是完整、自洽的提示词——复制它、放进你的代理上下文、直接使用。

本页是 [README.md](README.md) 的中文镜像，信息架构、锚点与链接结构保持一致。两份英文规则附带中文译文（[`translations/zh-CN/agent-rules/`](translations/zh-CN/agent-rules/)）；收工协议本身即以中文撰写，UI 工作流、代码审查与代理初始化协议均为英文。提示词的生命周期与维护规则见 [MAINTENANCE.md](MAINTENANCE.md)。

## 协议选择（Choose a Protocol）

| 协议 | 是什么 | 适合场景 | 语言 |
| --- | --- | --- | --- |
| [Codex Global Rules](#codex-global-rules) | 面向长时间 Codex 编码会话的全局行为规则 | 统一 Codex 工作流基线 | English · 中文译文 |
| [Universal Coding Agent Global Rules](#universal-coding-agent-global-rules) | 同一套核心规则的工具无关版本 | 混合使用多种代理的团队（Codex、Claude Code、Cursor 等） | English · 中文译文 |
| [UI Screenshot → Implementation Spec Protocol](#ui-screenshot--implementation-spec-protocol) | 将截图 / 设计稿转化为精确、可执行的 UI 实现规格 | 实现之前：先分析、后输出规格 | English |
| [UI Visual Fidelity Refinement Protocol](#ui-visual-fidelity-refinement-protocol) | 通过「渲染 → 对比 → 修复」闭环让实现向参考截图收敛 | 实现之后：视觉收敛阶段 | English |
| [Project Closeout Prompt](#project-closeout-prompt) | 将本轮成果安全收敛进默认分支并同步远端 | 里程碑结束、发布或交接时 | 中文 |
| [Expert Code Review Protocol](#expert-code-review-protocol) | 高信号代码审查：只报告真实、可操作、与合并相关的发现 | 合并前审查 PR / diff | English |
| [Universal Agent Init](#universal-agent-init) | 依据仓库证据初始化或改进 `AGENTS.md` / `CLAUDE.md` | 初始化或梳理代理指令 | English |

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

**UI 截图 → 实现规格协议 v2.0。**（英文原文）

**它做什么。** v2.0：将一张或多张参考截图 / 设计稿转化为一份简洁、结构化、可直接实现的 UI Implementation Specification——构建能解释截图的最小精确视觉模型，足以据此复现页面。代理的职责是「检查 → 建模 → 输出规格」，绝不直接实现。

**为什么有效。**

- **事实来源层级。** 证据按优先级排序：显式用户需求 → 截图 → 多截图一致证据 → 已验证的项目资产 / Token → 规格估算 → 推断；低优先级证据与高优先级冲突时，以高优先级为准。
- **四类证据标签。** 每条非平凡陈述标注 OBSERVED、ESTIMATED（结构化格式 `Estimated: ~240px / Plausible range: 232–248px / Confidence: high`）、INFERRED 或 UNKNOWN——不要把不确定性伪装成精确。
- **反幻觉规则。** 明确的「禁止臆造」清单（文字、菜单、Logo、字体、动画、响应式布局、Token），且可识别的产品记忆永远不能凌驾于所提供的截图之上。
- **先全局后局部。** 11 步分析顺序：来源与画布 → 全局构图 → 主要区域 → 布局关系 → 设计系统 → 组件模式 → 精确内容 → 资源 → 交互线索 → 响应式证据 → 实现关键约束。
- **关系优先于坐标。** 用容器关系、比例与间距节奏描述布局，偏好小间距系统而非几十个孤立测量值。
- **优先级与交接。** 需求按 CRITICAL / IMPORTANT / COSMETIC 分级，输出以 Uncertainty Register 与 Implementation Directives 收尾，实现方不会在关键失配未解决时去打磨细节。

**适合场景。** 截图重建、设计转代码交接，以及任何不希望实现方重新猜测视觉设计的任务。

**配套工具。** 规格输出由 Codex、Claude Code、Cursor、Gemini CLI 等编码代理消费。

**阅读。** [ui-screenshot-to-implementation-spec.md](ui-workflows/ui-screenshot-to-implementation-spec.md)（英文原文）

<details>
<summary>预览</summary>

````text
### OBSERVED
Directly visible or verifiable from the supplied material.

### ESTIMATED
Visually measurable only approximately.

Format important estimates as:

```text
Estimated: ~240px
Plausible range: 232–248px
Confidence: high
```

### INFERRED
Not directly shown, but a reasonable conclusion from the visible structure or existing verified project context.

Format:

```text
Inferred: sidebar likely collapses at narrow widths
Confidence: medium
```

### UNKNOWN
Not supported strongly enough to estimate or infer safely.

Do not disguise uncertainty as precision.
````

</details>

### UI Visual Fidelity Refinement Protocol

**UI 视觉保真收敛协议 v2.0。**（英文原文）

**它做什么。** v2.0：反复将真实浏览器渲染与参考截图对比，定位影响最大的差异并修复其根本原因，直到剩余差异影响很低或已无法合理缩减才停止。

**为什么有效。**

- **渲染即证据。** 协议核心规则是「The implementation is a hypothesis. The browser render is evidence.」——正确性只看渲染结果，绝不单凭 CSS 值或 DOM 结构下结论。
- **确定性环境，基线先行。** 先固定视口、缩放、DPR、路由、滚动、状态与字体/资源就绪条件，再采集基线渲染与排序后的差异清单——不要一开始就随机改 CSS。
- **先全局后局部的层级。** 对比按 Tier 1 结构 → Tier 2 主要几何 → Tier 3 排版 → Tier 4 颜色 / 表面 → Tier 5 组件细节 → Tier 6 微调执行；高层级仍错误时禁止做微调。
- **视觉严重度与验收门槛。** 差异按 V0–V3 分级（视觉专用，区别于 bug 优先级）；每个变更簇标注 IMPROVED / NEUTRAL / REGRESSED，若导致更高优先级区域回归则拒绝。
- **先找根本原因。** 先检查共享原因（容器宽度、Token、继承样式、断点），再修补症状；每个变更簇只对应一个连贯假设，不堆叠补偿性 hack。
- **明确的停止条件。** 仅当 V0 = 0、V1 = 0，完成全新终稿对比并登记 Remaining Difference Register 时才停止。

**适合场景。** UI 依据规格实现完成后的视觉收敛阶段。

**配套工具。** UI 实现代理：Codex、Claude Code、Cursor 等。

**阅读。** [ui-visual-fidelity-refinement.md](ui-workflows/ui-visual-fidelity-refinement.md)（英文原文）

<details>
<summary>预览</summary>

```text
The implementation is a hypothesis.
The browser render is evidence.

CAPTURE / OBSERVE
↓
COMPARE
↓
RANK DIFFERENCES
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

### Expert Code Review Protocol

**专家代码审查协议。**（英文原文）

**它做什么。** 一份高信号代码审查协议：像决定「这段代码是否安全、适合合并进生产环境」一样审查代码，优先真实、可操作的问题，而不是评论数量。

**为什么有效。**

- **六条入选标准。** 只有当问题有证据支撑、独立且可操作、实质性相关、具体到作者能理解并修复、不是主观偏好、也不是重复发现时，才予以报告——宁可没有发现，也不报投机性或低置信度的发现。
- **置信度门槛。** 每条发现附带 0.0–1.0 置信度；审查者必须先尝试推翻每条发现，通常只报告置信度 >= 0.80 的发现；严重但证据不足的担忧放入 `Needs Verification`。
- **风险优先的审查顺序。** 从最高层风险向下审查：意图与设计 → 正确性 → 安全 → 性能 → 可维护性 → 语言 / 框架 → 测试 → 文档。
- **严重度体系。** P0（立即阻塞）到 P3（非阻塞改进），每级都有具体示例。
- **明确排除低价值评论。** 琐碎格式、风格偏好、泛泛的「补测试」评论、理论性的微优化，除非明确要求，一律不报。
- **诚实的结论。** 审查以 APPROVE / CHANGES REQUESTED / NEEDS CONTEXT 三选一收尾——绝不声称运行过实际没有运行的测试、构建或 linter。

**适合场景。** 合并前的 PR / diff 审查、安全敏感审查，以及受够了低信号审查噪音的团队。

**配套工具。** 被要求审查代码的编码代理（Codex、Claude Code、Cursor 等）；也适用于聊天或代理审查工作流。

**阅读。** [expert-code-review.md](review-workflows/expert-code-review.md)（英文原文）

<details>
<summary>预览</summary>

```text
Report an issue only when it is:

1. supported by the available code or context
2. discrete and actionable
3. materially relevant
4. specific enough for the author to understand and fix
5. not merely a subjective preference
6. not a duplicate of another finding

Prefer no finding over a speculative or low-confidence finding.

Normally report only findings with confidence >= 0.80.

A potentially severe issue with insufficient evidence may instead be placed under `Needs Verification`, clearly explaining what missing evidence would confirm or dismiss it.

Never present speculation as a confirmed bug.
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
