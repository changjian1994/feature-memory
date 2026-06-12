# Feature Memory Skill

让 AI 辅助开发中的 feature 上下文可追溯、可恢复、可交接。

`feature-memory` 是一个面向 Code Agent / IDE Agent 的 Markdown-based skill。它把长期 feature、复杂 debug、交接恢复过程中的关键上下文沉淀到项目内，帮助新的 Agent 或同事快速知道：现在做到哪里、为什么这么做、下一步该做什么。

> 适用于支持目录型 skill 包或 Markdown-based Skill 的 Code Agent / IDE Agent。

## 为什么需要它

AI 辅助开发很容易遇到这些问题：

- 长 feature 做到一半后，Agent 忘记之前的设计和决策。
- 新会话接手时，需要反复解释背景、进度和阻塞点。
- 多轮 debug 的假设、日志和结论散落在对话里。
- 多个 Agent 或同事协作时，很难快速判断当前状态。

`feature-memory` 不是普通文档模板，而是一套给 Code Agent 使用的上下文记忆工作流。它让 AI 在合适的任务中先恢复记忆、再继续推进，减少重复实现和重复排查。

## 它能做什么

- **恢复上下文**：继续已有 feature 时，优先读取项目级 `ai-handoff.md` 和目标 feature 的关键文档。
- **记录关键变化**：只在目标、设计、状态、风险、交接信息或 debug 结论变化时更新 memory。
- **支持复杂 debug**：为复杂 Bug 创建独立目录，记录假设、验证、日志和最新结论。
- **沉淀 Bug 知识库**：通过 `bug-index.md` 复用历史排障经验，减少重复定位。
- **管理 feature 关系**：处理父子、依赖、交叉、重复和聚合 feature，避免目录混乱。
- **控制记忆成本**：默认只读活跃记忆，小改动不写 memory，单次任务只更新最关键的 1-3 个文件。

## 安装

这个仓库不包含运行时代码，也不需要安装依赖。

```bash
git clone https://github.com/changjian1994/feature-memory.git
```

然后把仓库中的 `feature-memory/feature-memory` 目录复制或链接到你的 Agent skills 目录。

不同 Agent 的 skills 目录位置可能不同，请以你使用的工具文档为准。

## 快速开始

在支持该 skill 的 Agent 中输入：

```text
/feature-memory
```

Agent 会引导你选择：

```text
1. 继续已有 feature
2. 创建新的 feature
3. 查看当前项目 feature 列表
4. 退出
```

如果宿主 Agent 支持原生选择器、Quick Pick 或问题选择组件，会优先使用交互式选择器；否则退化为编号文本输入。

## 适合场景

建议按需启用，不建议对所有普通编码任务无脑使用。

适合：

- 实现或继续一个多步骤 feature。
- 恢复之前中断的开发上下文。
- 为 feature 做交接准备。
- 跟踪设计、进度、决策或风险。
- 排查复杂 Bug、测试失败、线上异常或偶现问题。
- 多轮 debug 后需要收敛证据和下一步验证动作。

不适合：

- 一次性小修改。
- 简单重命名、格式化、注释或文案调整。
- 替代正式产品文档、测试或项目管理系统。
- 存放业务代码、密钥、Token、Cookie、个人数据或生产日志原文。

## 目标项目中的结构

使用后，Agent 会在目标项目根目录维护 `feature-memory/`：

```text
feature-memory/
├── ai-handoff.md
├── index.md
├── bug-index.md
├── project/
│   ├── overview.md
│   ├── architecture.md
│   ├── database.md
│   └── api.md
└── <feature-name>/
    ├── readme.md
    ├── design.md
    ├── progress.md
    ├── decision.md
    ├── handoff.md
    └── debug/
        └── BUG-YYYYMMDD-XXX-description/
```

其中：

- `ai-handoff.md`：项目级首读入口，帮助新 Agent 快速接手。
- `index.md`：全局 feature 索引，记录状态、关系、依赖和入口。
- `bug-index.md`：复杂 Bug 的全局知识库索引。
- `<feature-name>/handoff.md`：单个 feature 的当前接手视图。
- `<feature-name>/progress.md`：过程流水和状态变化。
- `<feature-name>/debug/`：复杂 Bug 的多轮排障记录。

## 工作方式

一次典型任务中，Agent 会：

1. 定位目标项目根目录的 `feature-memory/`。
2. 优先读取 `ai-handoff.md`，再按需读取 `index.md` 和目标 feature 文档。
3. 默认只读取 `ACTIVE` 和 `VERIFYING` 记忆。
4. 区分已验证事实、假设和待确认问题。
5. 开始实现、调试、重构或文档更新。
6. 仅在关键信息变化时更新 memory。
7. 默认单次任务只更新最关键的 1-3 个 memory 文件。

## 进阶能力

- **Debug 闭环**：复杂问题会生成 `debug.sh`，每轮输出 `output/vN.log`，用户执行后把日志原样发回，Agent 继续分析和收敛。详见 [`debug-workflow.md`](feature-memory/references/debug-workflow.md)。
- **Bug 知识库**：复杂 Bug 结束后登记到 `bug-index.md`，后续遇到相似问题时先复用历史结论。详见 [`templates.md`](feature-memory/references/templates.md)。
- **Feature 关系管理**：创建新 feature 前检查已有 feature，尽量避免重复目录，同时保持 feature 小而独立。详见 [`feature-relationship.md`](feature-memory/references/feature-relationship.md)。
- **记忆生命周期**：通过 `ACTIVE`、`VERIFYING`、`ARCHIVED`、`LEGACY` 控制读取范围和归档节奏。详见 [`memory-lifecycle.md`](feature-memory/references/memory-lifecycle.md)。
- **隐私与脱敏**：禁止记录密钥、Token、Cookie、个人数据、内部域名、生产日志原文或未脱敏数据库内容。详见 [`privacy.md`](feature-memory/references/privacy.md)。

## 示例

如果你想快速理解这个 skill 在真实研发场景中的价值，可以阅读中文示例 [`examples/call-management-zh`](examples/call-management-zh/) 或英文示例 [`examples/call-management`](examples/call-management/)。

这个示例基于一个脱敏后的“通话管理”功能，展示了：

- 新 Agent 如何从 `ai-handoff.md` 快速接手。
- 一个长期 feature 如何拆分 `handoff.md`、`progress.md`、`decision.md`。
- 一次录音播放问题如何通过 `bug-index.md` 和 debug 结论沉淀为可复用经验。
- 如何在记录上下文的同时避免保存密钥、生产日志和敏感数据。

## 仓库结构

```text
.
├── examples/
├── feature-memory/
│   ├── SKILL.md
│   └── references/
├── CHANGELOG.md
├── LICENSE
├── PRIVACY.md
├── readme.md
└── readme.en.md
```

- `examples/`：脱敏后的正式示例，用于展示完整 feature-memory 工作流。
- `feature-memory/SKILL.md`：skill 入口文件，采用 Lean Core 路由结构。
- `feature-memory/references/`：按需读取的详细规则、模板和安全说明。
- `PRIVACY.md`：面向用户的隐私和脱敏说明。
- `CHANGELOG.md`：版本变更记录。

## 文档

- [English README](readme.en.md)
- [中文示例：通话管理](examples/call-management-zh/)
- [Example: Call Management](examples/call-management/)
- [Changelog](CHANGELOG.md)
- [Privacy](PRIVACY.md)
- [Debug workflow](feature-memory/references/debug-workflow.md)
- [Feature relationship](feature-memory/references/feature-relationship.md)
- [Memory lifecycle](feature-memory/references/memory-lifecycle.md)
- [Templates](feature-memory/references/templates.md)

## 贡献

欢迎通过 Issue 或 Pull Request 改进模板、目录规范、debug 协议和不同 Agent 的安装说明。

Bug 或建议也可以发送到：`changjian1994@sina.com`。

## License

本项目使用 MIT License。详见 [`LICENSE`](LICENSE)。
