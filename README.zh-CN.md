# AI Prompt Protocols

**减少即兴发挥，让 Agent 工作更可验证、可复现。**

这是一套经过真实开发任务持续维护的纯文本提示词与工程协议。每份协议都有明确目标、适用阶段和可以独立复制使用的权威源文件。

[English](README.md) · [简体中文](README.zh-CN.md) · [维护规范](MAINTENANCE.md) · [仓库指南](AGENTS.md)

> 这里不是 Prompt 商城、基准测试榜单，也不是万能 Agent 框架。优先选择**恰好适合当前任务的最小协议**，而不是把全部 Prompt 一起塞进上下文。

## Start with the task

**先确定任务所处的阶段，而不是选最长的提示词。** 常驻行为规则与一次性阶段协议应分开使用。

| 任务 | 推荐协议 | 适用阶段 |
| --- | --- | --- |
| **让 Codex 会话稳定可控** | [Codex Global Rules](agent-rules/codex-global-rules.md) | 长期 Codex 专属约束 |
| **跨不同编程 Agent 保持一致性** | [Universal Coding Agent Global Rules](agent-rules/universal-coding-agent-global-rules.md) | 长期、工具无关的约束 |
| **从截图推导可执行的界面规格** | [UI Screenshot → Implementation Spec](ui-workflows/ui-screenshot-to-implementation-spec.md) | 写代码**之前** |
| **让实际页面更接近参考图** | [UI Visual Fidelity Refinement](ui-workflows/ui-visual-fidelity-refinement.md) | 首版实现**之后** |
| **安全完成和收口工程任务** | [Project Closeout](project-workflows/project-closeout.md) | 验证、Diff 审查与集成决策之后 |
| **执行高信号代码审查** | [Expert Code Review](review-workflows/expert-code-review.md) | 接受变更或合并之前 |
| **建立项目级 Agent 指令** | [Universal Agent Init](init-workflows/universal-agent-init.md) | 新仓库初始化或旧规则核查 |

## Protocols in context

### Codex Global Rules

**适用于：** 需要明确边界、小范围修改与证据驱动验证的 Codex 编码会话。

- 仅在存在会实质影响结果的重要不确定性时追问，否则采用安全并说明的合理假设。
- 禁止大范围破坏性清理和未经授权的外部副作用。
- 检查真实改动、运行与任务相称的测试，不能把未验证内容说成已完成。

**阅读：** [英文原文](agent-rules/codex-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/codex-global-rules.md)

### Universal Coding Agent Global Rules

**适用于：** Codex、Claude Code、Cursor 等不同编码 Agent 之间共享基础行为规则。

保留 Codex 版的主要安全与质量约束，但不绑定某一工具专属调用方式。环境允许时可批量执行独立的只读检查；有副作用的操作仍须遵守明确边界。

**阅读：** [英文原文](agent-rules/universal-coding-agent-global-rules.md) · [中文译文](translations/zh-CN/agent-rules/universal-coding-agent-global-rules.md)

### UI Screenshot → Implementation Spec Protocol

**适用于：** 将截图、设计稿转化为真正可以交给实现 Agent 的技术规格。

将信息区分为 **OBSERVED、ESTIMATED、INFERRED、UNKNOWN**，根据布局、响应式关系和视觉层级建立一致模型，并记录证据无法证明的部分。这份协议输出的是**实现规格，而不是代码**。

**阅读：** [英文原文](ui-workflows/ui-screenshot-to-implementation-spec.md)

### UI Visual Fidelity Refinement Protocol

**适用于：** 真实渲染页面与参考图之间的多轮视觉收敛。

```text
捕获 → 比较 → 排序 → 定位根因
     → 修改一组关联问题 → 重新渲染
     → 接受 / 调整 / 回退
```

实际渲染结果才是证据。如果工具不能截图或对比，协议要求如实说明有限验证模式，不得伪造视觉验收成功。

**阅读：** [英文原文](ui-workflows/ui-visual-fidelity-refinement.md)

### Project Closeout Prompt

**适用于：** 完成一次工程任务、避免丢失工作或推送到错误的分支与远端。

先识别真实 Git 状态与项目的集成模型，再验证本轮修改和 Diff，最后只执行**已经获准**的提交、同步、发行或部署。完成的含义不是“出现了一个 Commit”，而是工作进入预期的安全最终状态。

**阅读：** [中文原文](project-workflows/project-closeout.md)

### Expert Code Review Protocol

**适用于：** 接收变更之前的高信噪比代码审查。

只报告有证据、问题离散、可以行动且真正影响结果的发现。无法确认的重要疑点放在 **Needs Verification**，不要把猜测包装成已验证 Bug，也不要重复列出同一问题。

**阅读：** [英文原文](review-workflows/expert-code-review.md)

### Universal Agent Init

**适用于：** 根据**当前真实项目**初始化或修订 `AGENTS.md` 与 `CLAUDE.md`。

先检查源码、测试、构建命令和现有指令，再写项目规则；保护用户改动，不加入“写出干净代码”之类的空洞模板，也不假定缺少规则文件就需要创建庞大文档。

**阅读：** [英文原文](init-workflows/universal-agent-init.md)

## Putting them together

```text
全局行为规则
      ↓
真实仓库上下文及项目指令
      ↓
当前阶段的专项协议
      ↓
实现 / 审查
      ↓
验证与 Diff 审阅
      ↓
经授权的工程收口
```

**示例：** 截图实现走 **Spec → Implement → Visual Fidelity**；合并前走 **Expert Code Review**；接手新仓库可先使用 **Universal Agent Init**。全局规则不能取代项目特有事实与权限约束。

## How to use a protocol

下方链接的 Markdown 文件才是完整权威提示词；此页只负责导览和选用。

1. **阅读原文。** 每个 Markdown 文件是本协议的权威来源；README 只负责导览。
2. **区分作用域。** 全局规则适合持久指令位置，专项协议只在相应任务阶段调用。
3. **核对 Agent 支持。** 某个工具是否自动读取 `AGENTS.md` 或 `CLAUDE.md` 取决于产品，不能假定全部工具都有相同机制。
4. **在实际项目里验证。** 协议是工作纪律，不是准确率或性能的绝对保证。

## File map

```text
agent-rules/          长期编码 Agent 规则
ui-workflows/         截图规格与视觉保真收敛
project-workflows/    项目收口
review-workflows/     专家代码审查
init-workflows/       Agent 指令初始化
translations/zh-CN/   英文全局规则的中文译本
README.md            英文展示页
README.zh-CN.md      对应中文镜像，保持相同标题与锚点
MAINTENANCE.md       权威文件与衍生内容维护规则
```

## Contributing

欢迎可复现的歧义反馈、经过验证的局部改进、翻译修正以及真实使用中的失败案例。不接受未经实际验证的批量 Prompt 列表和缺乏依据的万能效果宣称。遵守 [MAINTENANCE.md](MAINTENANCE.md)；不要无明确任务就重写已经打磨过的权威 Prompt。

## License

[MIT](LICENSE) · 以小而精的纯文本协议库为长期维护方向。
