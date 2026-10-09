<h1 align="center">🧭 AI Prompt Protocols</h1>

<p align="center">
  <strong>减少即兴发挥，让 Agent 工作更可复现。</strong>
</p>

<p align="center">
  一套持续维护的 AI 辅助软件开发工程协议手册。<br>
  七份聚焦具体任务的协议，明确的操作边界，以证据支撑结论。
</p>

<p align="center">
  <a href="README.md"><strong>English</strong></a>
  &nbsp;·&nbsp;
  <a href="README.zh-CN.md">简体中文</a>
  &nbsp;·&nbsp;
  <a href="#start-with-the-task">🗂️ 选择协议</a>
  &nbsp;·&nbsp;
  <a href="#putting-them-together">🔁 工作流程</a>
  &nbsp;·&nbsp;
  <a href="MAINTENANCE.md">⚙️ 维护规范</a>
</p>

<p align="center">
  <sub>全局规则 &nbsp;·&nbsp; UI 视觉保真 &nbsp;·&nbsp; 代码审查 &nbsp;·&nbsp; 仓库初始化 &nbsp;·&nbsp; 项目收口</sub>
</p>

---

> **选当前任务真正需要的最小协议，而不是最长的提示词。**
>
> 这里不是 Prompt 商城、不提供未经验证的效果排名，也不是适用于所有 Agent 的万能框架。这些文档来自实际工程任务中的持续打磨；它们不能代替阅读真实仓库、验证代码修改，或在执行有副作用的操作前取得授权。

## Start with the task

每份文档都有明确的职责。**全局规则决定 Agent 如何持续工作；专项协议只指导某一个任务阶段。** 不应把全部协议一次性塞入每个会话。

<table>
  <tr>
    <td width="50%" valign="top">
      <h3>01 / Establish the Rules</h3>
      <p><sub>持久化 AGENT 行为规则</sub></p>
      <p>限制改动范围、保护已有工作，并要求每项完成声明都有真实证据。</p>
      <p><a href="agent-rules/codex-global-rules.md"><strong>Codex Global Rules →</strong></a><br><a href="agent-rules/universal-coding-agent-global-rules.md">Universal Coding Agent Rules →</a></p>
    </td>
    <td width="50%" valign="top">
      <h3>02 / Make the Interface Match</h3>
      <p><sub>设计证据 → 真实渲染结果</sub></p>
      <p>先将参考截图转化为可靠的实现规格，再根据实际渲染结果进行视觉精修。</p>
      <p><a href="ui-workflows/ui-screenshot-to-implementation-spec.md"><strong>Screenshot → Spec →</strong></a><br><a href="ui-workflows/ui-visual-fidelity-refinement.md">Visual Fidelity Refinement →</a></p>
    </td>
  </tr>
  <tr>
    <td width="50%" valign="top">
      <h3>03 / Establish Confidence</h3>
      <p><sub>真实仓库 · 可执行审查结果</sub></p>
      <p>根据实际源码建立项目级 Agent 指令；审查代码时不虚构缺陷。</p>
      <p><a href="init-workflows/universal-agent-init.md"><strong>Universal Agent Init →</strong></a><br><a href="review-workflows/expert-code-review.md">Expert Code Review →</a></p>
    </td>
    <td width="50%" valign="top">
      <h3>04 / Finish Without Losing Work</h3>
      <p><sub>验证 · DIFF · 授权边界</sub></p>
      <p>检查真实工作区、完成与任务相称的验证，并且只执行已获得授权的集成操作。</p>
      <p><a href="project-workflows/project-closeout.md"><strong>Project Closeout →</strong></a></p>
    </td>
  </tr>
</table>

| 当前任务 | 推荐协议 | 适用阶段 |
| --- | --- | --- |
| 让 Codex 会话稳定、可控 | [Codex Global Rules](agent-rules/codex-global-rules.md) | 持久化 Codex 专属规则 |
| 跨不同编程 Agent 保持一致行为 | [Universal Coding Agent Global Rules](agent-rules/universal-coding-agent-global-rules.md) | 持久化、工具无关的行为约束 |
| 从截图推导页面实现方案 | [UI Screenshot → Implementation Spec](ui-workflows/ui-screenshot-to-implementation-spec.md) | 编写页面代码之前 |
| 将实际页面调整得更接近参考图 | [UI Visual Fidelity Refinement](ui-workflows/ui-visual-fidelity-refinement.md) | 首版页面完成并实际渲染之后 |
| 完成一次有明确范围的工程任务 | [Project Closeout](project-workflows/project-closeout.md) | 测试、Diff 检查与集成决策之后 |
| 审查代码或待合并变更 | [Expert Code Review](review-workflows/expert-code-review.md) | 批准或合并变更之前 |
| 建立或修订仓库 Agent 指令 | [Universal Agent Init](init-workflows/universal-agent-init.md) | 接手仓库或审计现有指令时 |

## Protocols in context

下面链接的文件才是**具有权威性的完整提示词**。本 README 只负责导览，不是替代原始协议的精简版。

### Codex Global Rules

**适用场景：** 希望 Codex 在修改范围、验证方式、已有文件保护和外部操作权限方面保持稳定。协议强调最小必要改动、尊重用户现有工作、按风险选择检查，并准确报告完成情况。

**阅读：** [英文原文](agent-rules/codex-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md)

### Universal Coding Agent Global Rules

**适用场景：** 在 Codex、Claude Code、Cursor 等编程 Agent 之间复用同一套工程行为原则。不假定所有工具具有相同接口，同时保留对破坏性操作、Git 状态与执行授权的明确边界。

**阅读：** [英文原文](agent-rules/universal-coding-agent-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md)

### UI Screenshot → Implementation Spec Protocol

**在写代码之前使用。** 将用户提供的截图转化为可执行的界面规格。区分直接可见的信息、合理估计与推断，描述布局关系、视觉层级及参考图无法证明的部分。

**交付物：** 有证据支撑的**实现规格**，而不是直接生成的页面代码。

**阅读：** [权威协议原文](ui-workflows/ui-screenshot-to-implementation-spec.md)

### UI Visual Fidelity Refinement Protocol

**在首版页面真实渲染后使用。** 将当前页面与参考图对比，优先解决有意义的差异，定位根因，每次修改一组相互关联的问题，然后重新渲染并评估。

```text
参考图 + 当前页面截图
         ↓
    比较真实视觉证据
         ↓
     修复关键根因
         ↓
重新渲染 → 保留 / 调整 / 回退
```

如果当前环境无法打开浏览器、截图或进行实际对比，必须**明确说明验证受限**，不能声称视觉验收已经通过。

**阅读：** [权威协议原文](ui-workflows/ui-visual-fidelity-refinement.md)

### Project Closeout Prompt

**只有用户明确触发时才使用。** 这份中文协议区分普通状态检查与已授权的工程收工任务。它要求在执行允许的 Git 收尾操作前，先审查工作区、Diff 和验证结果；涉及破坏性修改或生产环境的操作仍需额外明确授权。

**阅读：** [中文权威原文](project-workflows/project-closeout.md)

### Expert Code Review Protocol

**在接受变更前使用。** 只报告有证据、可定位、可修复、与当前审查范围实质相关且不重复的问题。将确定存在的缺陷与 **Needs Verification（仍需验证）** 分开；没有可确认的问题，也优于列出大量猜测。

**阅读：** [权威协议原文](review-workflows/expert-code-review.md)

### Universal Agent Init

**接手仓库或审计指令文件时使用。** 在更新 `AGENTS.md` 或 `CLAUDE.md` 之前，先阅读真实源码、可用命令、测试和已有说明；保留有效约束及用户工作，不机械粘贴通用模板。

**阅读：** [权威协议原文](init-workflows/universal-agent-init.md)

## Putting them together

```text
全局 Agent 行为规则
         ↓
真实仓库事实与现有指令
         ↓
必要时选用一份专项协议
         ↓
执行实现或检查
         ↓
验证证据并审查 Diff
         ↓
完成已获授权的工程收口
```

**典型路径：** *截图 → 实现规格 → 编码 → 视觉保真精修*；或 *变更 → 专家代码审查 → 验证后的收工*。这些协议是对实际项目规则的补充，不能覆盖真实仓库事实或用户明确的权限限制。

## How to use a protocol

1. **打开对应的权威 Markdown。** 每份阶段协议都可以独立使用；这份目录不能替代完整原文。
2. **放在正确的上下文里。** 全局规则适合持久指令；代码审查或视觉精修协议只在相应任务阶段使用。
3. **确认 Agent 的加载方式。** 是否自动读取 `AGENTS.md`、`CLAUDE.md` 等文件取决于具体工具，不是所有产品共有的标准。
4. **在真实环境中验收。** 再严谨的 Prompt，也不能保证代码一定正确、审查一定准确，或视觉结果一定与参考图一致。

## File map

```text
agent-rules/          Codex 专用与通用全局规则
ui-workflows/         截图规格与视觉保真精修
project-workflows/    经明确触发和授权的项目收口
review-workflows/     证据驱动的代码审查
init-workflows/       AGENTS.md / CLAUDE.md 初始化
translations/zh-CN/   权威英文全局规则的中文译文
README.md            英文协议导览
README.zh-CN.md      对应中文版，保持相同标题与锚点
MAINTENANCE.md       权威源文件、翻译与维护生命周期
```

## Contributing

最有价值的改进是**可复现的真实问题或已验证的修正**。提交反馈时，说明任务背景、原协议的不足，以及能够解决问题的最小修改。权威协议仍由源文件维护，中英文 README 的章节与导航必须保持一致。

请阅读 [MAINTENANCE.md](MAINTENANCE.md)，了解权威内容与派生导览、翻译同步、路径稳定性和修改审查的规则。避免无证据地扩充 Prompt、制造重复的 `-v2` 文件，或宣称某份协议对所有 Agent 都有效。

## License

本仓库采用 **[MIT License](LICENSE)**。这是一套轻量的纯文本工程协议库，**不需要安装应用、运行构建或执行依赖安装步骤**。

---

<p align="center"><sub>减少即兴发挥。以证据说话。让每一次工程工作都有据可查。</sub></p>
