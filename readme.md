# Feature Memory Skill

一个面向 Code Agent 的 feature 级记忆 skill，用于在多轮开发、调试、交接和恢复上下文时，把关键设计、进度、决策和排障过程持久化到项目中，减少重复工作和上下文丢失。

适用于支持 Markdown-based Skill 或目录型 skill 包的 Code Agent / IDE Agent。

## 解决什么问题

AI 辅助开发经常会遇到这些情况：

- 长 feature 做到一半后，Agent 忘记之前的设计决策。
- 新会话接手项目时，需要重新解释背景、进度和阻塞点。
- Bug 排查经过多轮假设和验证后，历史证据散落在对话中。
- 多个 Agent 协作时，无法快速确认“已完成什么、还差什么、为什么这么做”。

`feature-memory` 的目标是把一个 feature 的开发过程沉淀为可追溯、可恢复、可交接的项目内文档。

## 核心能力

- 记忆优先：开始开发前读取已有 feature 记忆，避免重复实现和重复排查。
- 事实与假设分离：已验证事实、待确认问题和调试假设分别记录，避免把猜测写成结论。
- 持续同步：设计变更、代码实现、Bug 修复、架构调整后同步更新记忆文档。
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
│       ├── readme.md
│       └── templates.md
├── CHANGELOG.md
├── LICENSE
├── readme.md
└── readme.en.md
```

- `feature-memory/SKILL.md`：skill 入口文件，包含触发条件、核心原则、目录规范和主流程。
- `feature-memory/references/templates.md`：项目级、feature 级、Bug 级文档模板。
- `feature-memory/references/debug-workflow.md`：复杂问题排障流程、`debug.sh` 安全规则和日志迭代协议。
- `feature-memory/references/readme.md`：参考资料说明和读取时机。
- `.github/ISSUE_TEMPLATE/`：GitHub Issue 模板。
- `CHANGELOG.md`：版本变更记录。
- `LICENSE`：MIT 开源许可证。

## 安装方式

这个仓库不包含运行时代码，也不需要安装依赖。将仓库中的 `feature-memory/` 目录作为一个 skill 包导入到你的 Code Agent / IDE Agent 的 skills 目录即可。

常见方式：

```bash
git clone https://github.com/changjian1994/feature-memory.git
```

然后把 `feature-memory/feature-memory` 目录复制或链接到你的 Agent skills 目录。不同 Agent 的 skill 目录位置可能不同，请以你使用的工具文档为准。

## 使用场景

当 Agent 处理以下任务时，适合触发此 skill：

- 实现或继续一个多步骤 feature。
- 恢复之前中断的开发上下文。
- 为 feature 做交接准备。
- 跟踪设计、进度、决策或风险。
- 排查复杂 Bug、测试失败、线上异常或偶现问题。
- 多轮 debug 后需要收敛证据和下一步验证动作。

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

注意：目标项目里的 `feature-memory/` 只用于保存开发记忆，不应存放业务代码、测试文件、构建产物或密钥。

## 工作流概览

1. 定位目标项目根目录的 `feature-memory/`。
2. 如果记忆目录不存在，初始化最小项目级文档结构。
3. 读取 `index.md`、`project/*` 和目标 feature 的关键文档。
4. 区分已验证事实、假设和待确认问题。
5. 开始实现、调试、重构或文档更新。
6. 任务结束前同步更新 `design.md`、`progress.md`、`decision.md`、`handoff.md` 和 `index.md`。

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
- 不适合：替代正式产品文档、替代测试、存放敏感信息、存放业务代码。

## 贡献

欢迎通过 Issue 或 Pull Request 改进模板、目录规范、debug 协议和不同 Agent 的安装说明。

Bug 或建议也可以发送到：`changjian1994@sina.com`。

## License

本项目使用 MIT License。详见 `LICENSE`。
