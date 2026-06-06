# Feature Memory Skill

让 AI 辅助开发中的 feature 上下文可追溯、可恢复、可交接。

一个面向 Code Agent 的 feature 级记忆 skill，用于在多轮开发、调试、交接和恢复上下文时，把关键设计、进度、决策和排障过程持久化到项目中，减少重复工作和上下文丢失。

适用于支持 Markdown-based Skill 或目录型 skill 包的 Code Agent / IDE Agent。

## 解决什么问题

AI 辅助开发经常会遇到这些情况：

- 长 feature 做到一半后，Agent 忘记之前的设计决策。
- 新会话接手项目时，需要重新解释背景、进度和阻塞点。
- Bug 排查经过多轮假设和验证后，历史证据散落在对话中。
- 多个 Agent 协作时，无法快速确认“已完成什么、还差什么、为什么这么做”。

`feature-memory` 不是普通文档模板，而是一套给 Code Agent 使用的上下文记忆工作流。它帮助研发团队把长期 feature、复杂 debug 和交接恢复过程沉淀到项目内，让新的 Agent 或同事可以快速理解当前状态，继续推进而不是从头再来。

## 核心能力

- 记忆优先：适用任务开始前读取已有 feature 记忆，避免重复实现和重复排查。
- 事实与假设分离：已验证事实、待确认问题和调试假设分别记录，避免把猜测写成结论。
- 按需同步：当目标、设计、状态、风险、交接信息或 debug 结论变化时更新记忆文档。
- 快速交接：新的 Agent 可以通过 `handoff.md`、`progress.md`、`design.md` 在短时间内恢复状态。
- Debug 收敛：复杂问题使用独立 Bug 目录记录假设、验证、日志和最新结论。

## 仓库结构

```text
.
├── .github/
│   └── ISSUE_TEMPLATE/
├── feature-memory/
│   ├── SKILL.md
│   └── references/
│       ├── debug-workflow.md
│       ├── privacy.md
│       ├── readme.md
│       └── templates.md
├── CHANGELOG.md
├── LICENSE
├── PRIVACY.md
├── readme.md
└── readme.en.md
```

- `feature-memory/SKILL.md`：skill 入口文件，包含触发条件、核心原则、目录规范和主流程。
- `feature-memory/references/templates.md`：项目级、feature 级、Bug 级文档模板。
- `feature-memory/references/debug-workflow.md`：复杂问题排障流程、`debug.sh` 安全规则和日志迭代协议。
- `feature-memory/references/privacy.md`：skill 内置隐私、安全边界和脱敏规则。
- `feature-memory/references/readme.md`：参考资料说明和读取时机。
- `.github/ISSUE_TEMPLATE/`：GitHub Issue 模板。
- `CHANGELOG.md`：版本变更记录。
- `LICENSE`：MIT 开源许可证。
- `PRIVACY.md`：隐私、脱敏和目标项目 Git 提交建议。

## 安装方式

这个仓库不包含运行时代码，也不需要安装依赖。将仓库中的 `feature-memory/` 目录作为一个 skill 包导入到你的 Code Agent / IDE Agent 的 skills 目录即可。

常见方式：

```bash
git clone https://github.com/changjian1994/feature-memory.git
```

然后把 `feature-memory/feature-memory` 目录复制或链接到你的 Agent skills 目录。不同 Agent 的 skill 目录位置可能不同，请以你使用的工具文档为准。

## 使用场景

建议按需启用此 skill，不建议对所有普通编码任务无脑全局启用。当 Agent 处理以下任务时，适合触发：

- 实现或继续一个多步骤 feature。
- 恢复之前中断的开发上下文。
- 为 feature 做交接准备。
- 跟踪设计、进度、决策或风险。
- 排查复杂 Bug、测试失败、线上异常或偶现问题。
- 多轮 debug 后需要收敛证据和下一步验证动作。

对于一次性小修改、格式调整、简单重命名或无需保留上下文的低风险任务，通常不需要创建或更新记忆文档。

## 在目标项目中生成的记忆结构

skill 会要求在目标项目根目录维护 `feature-memory/` 目录：

```text
feature-memory/
├── index.md
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

注意：目标项目里的 `feature-memory/` 只用于保存开发记忆，不应存放业务代码、测试文件、构建产物、密钥、Token、Cookie、个人数据、内部域名、生产日志原文或未脱敏数据库内容。

## 工作流概览

1. 定位目标项目根目录的 `feature-memory/`。
2. 如果记忆目录不存在，初始化最小项目级文档结构。
3. 读取 `index.md`、`project/*` 和目标 feature 的关键文档。
4. 区分已验证事实、假设和待确认问题。
5. 开始实现、调试、重构或文档更新。
6. 如果 feature 目标、设计、状态、风险、交接信息或 debug 结论发生变化，任务结束前同步更新相关记忆文档。

## Debug 约定

复杂问题会在对应 feature 下建立独立 Bug 目录：

```text
feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/
├── readme.md
├── timeline.md
├── conclusion.md
├── debug.sh
└── output/
```

- `timeline.md` 只追加，不覆盖历史排查过程。
- `conclusion.md` 只保留当前最新结论，并明确已确认事实、已排除假设和剩余假设。
- `debug.sh` 默认只包含只读诊断命令；写入、删除、迁移、重启服务或修改数据库前必须先确认。
- `output/` 保存每轮诊断输出，便于 Agent 基于日志继续收敛。

## 适用边界

- 适合：长期 feature、复杂调试、多 Agent 交接、上下文恢复、架构决策记录。
- 不适合：普通一次性小任务、替代正式产品文档、替代测试、存放敏感信息、存放业务代码。
- 提交策略：目标项目中的 `feature-memory/` 是否提交到 Git 应由项目决定；公开仓库或包含敏感信息时，建议不提交或先脱敏。
- 隐私规则：写入任何架构、API、数据库或日志信息前，请先阅读 `PRIVACY.md`。

## 贡献

欢迎通过 Issue 或 Pull Request 改进模板、目录规范、debug 协议和不同 Agent 的安装说明。

Bug 或建议也可以发送到：`changjian1994@sina.com`。

## License

本项目使用 MIT License。详见 `LICENSE`。
