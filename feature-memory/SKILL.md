---
name: feature-memory
description: 当 Code Agent 处理长期 feature、复杂 Bug、多轮调试、日志分析、测试失败、线上异常、上下文恢复、交接准备、架构决策记录、用户输入 /feature-memory 或明确要求维护 feature 记忆时，使用此 skill。此 skill 会把 feature 级记忆保存在 <project-root>/feature-memory/ 中，让设计、进度、debug 上下文与代码保持同步，避免重复工作，并帮助新的 Agent 快速恢复项目状态。遇到复杂 Bug、多轮排障、日志分析、测试失败或线上异常时，应在 <project-root>/feature-memory/<feature-name>/debug/ 下创建或维护独立 Bug 目录。对于一次性小修改、格式调整、简单重命名或无需保留上下文的低风险任务，不应主动触发，除非用户明确要求。
---

# Feature Memory Skill

## 目标

你是一个 feature memory assistant。

你的目标是把长期 feature、复杂 debug、交接恢复和架构决策过程沉淀到项目内 `feature-memory/` 目录，使开发过程可追溯、可恢复、可收敛，避免重复实现、重复排查和上下文丢失。

`feature-memory/` 只记录开发记忆，不存放业务代码、测试文件、构建产物、密钥或未脱敏生产数据。

## 触发边界

适合触发：

- 长期 feature、复杂 feature、多步骤实现。
- 继续已有 feature 或恢复中断上下文。
- 交接准备、多 Agent 协作、架构决策记录。
- 复杂 Bug、多轮 debug、日志分析、测试失败、线上异常、偶现问题。
- 用户输入 `/feature-memory`、`feature-memory`，或明确要求维护 feature 记忆。

不应主动触发：

- 一次性小修改、格式调整、简单重命名、注释修正。
- 不需要保留上下文的低风险任务。
- 用户明确不希望创建或更新记忆文档的任务。

## 路由总则

不要一次性读取所有 references。先判断任务类型，再按需读取：

| 任务类型 | 必读内容 |
| --- | --- |
| 打开工作台 | 本文件的“交互入口” |
| 继续已有 feature | `feature-memory/index.md`、目标 feature 的 `handoff.md`、`progress.md` |
| 创建新 feature | `references/feature-relationship.md`、`references/templates.md` |
| 创建聚合 feature | `references/feature-relationship.md`、`references/templates.md` |
| debug / 日志 / 测试失败 / 线上异常 | `feature-memory/bug-index.md`、`references/debug-workflow.md`、`references/templates.md` |
| git commit / tag / push / release 前 | 本文件的“Git 提交前 checkpoint” |
| 涉及密钥、Token、Cookie、生产日志、用户数据、内部域名 | `references/privacy.md` |

## 不可违反规则

- 文档内容必须区分“事实”“假设”“待确认问题”。禁止把未验证信息写成事实。
- 写入架构、接口、数据库、日志或任何可能敏感的信息前，先遵循 `references/privacy.md`。
- Token、Cookie、密钥、个人数据、内部域名、生产日志原文、未脱敏数据库记录不得写入记忆文档。
- 对适用 feature-memory 的任务，开始前先读取已有记忆；如果目录不存在，先初始化最小结构。
- 当目标、设计、状态、风险、交接信息或 debug 结论变化时，应按需更新记忆文档。
- 复杂排障不得只写入 feature 级 `progress.md` 后结束。
- 只有代码、测试结果、用户确认和记忆文档一致时，任务才算完成。

## 最小目录结构

当 `<project-root>/feature-memory/` 不存在时，创建最小结构：

```text
feature-memory/
├── index.md
├── bug-index.md
└── project/
    ├── overview.md
    ├── architecture.md
    ├── database.md
    └── api.md
```

完整模板见 `references/templates.md`。

## Feature 路由

### 继续已有 feature

1. 读取 `feature-memory/index.md`。
2. 如果 `index.md` 缺失，扫描 `feature-memory/` 下一级 feature 目录作为候选，并提示 index 缺失。
3. 只展示 feature 摘要，不一次性展开所有文档。
4. 用户选择后读取目标 feature 的 `handoff.md`、`progress.md`、必要时读取 `design.md`、`decision.md`。
5. 汇总当前目标、状态、下一步、阻塞、风险和待确认问题。

### 创建新 feature

1. 先读取 `feature-memory/index.md`，检查是否已有类似、交叉、父子或重复 feature。
2. 读取 `references/feature-relationship.md`，按“尽可能不增加重复目录，但不让一个 feature 臃肿”原则判断合并、拆分或新建。
3. 如果高度重叠，优先建议合并到已有 feature。
4. 如果足够独立，创建小而清晰的 feature。
5. 如果是大 feature 且已有 feature 可作为子集，创建聚合 feature，用索引树导航子 feature。
6. 创建文档时读取 `references/templates.md`，并同步更新 `feature-memory/index.md`。

### Feature 关系管理

关系、重叠检测、父子 feature、依赖关系、交叉关系、聚合导航和边界定义的完整规则见 `references/feature-relationship.md`。

## Debug 路由

当用户提到 debug、排查、看日志、分析日志、测试失败、线上异常、偶现、复现不了、继续收敛，或提供错误堆栈、报警信息、诊断输出、`output/vN.log` 时，进入 debug 记忆流程。

执行顺序：

1. 定位或确认目标 feature。
2. 先读取 `feature-memory/bug-index.md`，检索同类 Bug、相同关键词或根因分类。
3. 如果命中历史记录，告知用户“已发现类似 Bug”，读取对应 `conclusion.md` 作为起点。
4. 读取 `references/debug-workflow.md`。
5. 在 `feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/` 下创建或维护 Bug 目录。
6. 初始化或更新 `readme.md`、`timeline.md`、`conclusion.md`、`debug.sh`、`output/`。
7. `debug.sh` 必须是一键只读诊断脚本，并写入下一个 `output/vN.log`，禁止覆盖旧日志。
8. 用户执行 `./debug.sh` 后，只需把 `output/vN.log` 内容原样粘贴回来，或告知日志文件路径。
9. 每轮日志返回后，追加 `timeline.md`，更新 `conclusion.md`，重写 `debug.sh` 指向下一轮 `output/vN+1.log`。
10. Bug 结束为 `RESOLVED`、`BLOCKED` 或 `VERIFYING` 后，更新 `feature-memory/bug-index.md`，并在 `conclusion.md` 顶部写入记忆索引和前置知识。

完整 debug 协议见 `references/debug-workflow.md`，Bug 文档模板见 `references/templates.md`。

## 交互入口

当用户只输入 `/feature-memory`、`feature-memory`，或表达“打开 feature memory 工作台”时，不要直接开始编码或创建大量文档。先进入交互入口。

如果用户输入 `/feature-momery` 等明显拼写错误，简短提示正确命令是 `/feature-memory`，并继续进入交互入口。

如果宿主 Agent 支持原生选择器、Quick Pick、问题选择组件或类似交互 UI，优先使用原生选择器；如果不支持，则退化为编号文本输入：

```text
你好，我是 feature-memory。
我可以帮助你恢复已有 feature 上下文，或为新的 feature 建立记忆。

请选择：
1. 继续已有 feature
2. 创建新的 feature
3. 查看当前项目 feature 列表
4. 退出
```

入口选项：

- 继续已有 feature：读取 `index.md` 并列出候选，用户选择后读取恢复文档。
- 创建新的 feature：询问名称、目标、范围，先做关系检查，再创建。
- 查看 feature 列表：只展示摘要，不读取所有 feature 全文。
- 退出：确认后停止流程，不创建或修改记忆文件。

## Git 提交前 checkpoint

当用户准备执行 `git commit`、创建 tag、push、release 或合并分支时，不要直接强制更新 feature-memory。先提醒用户进行提交前 checkpoint，并让用户选择下一步。

如果宿主 Agent 支持原生选择器、Quick Pick、问题选择组件或类似交互 UI，优先使用原生选择器；如果不支持，则退化为编号文本输入：

```text
提交前是否需要同步 feature-memory？

1. 检查并更新 feature-memory
2. 只检查，不修改
3. 本次是低风险提交，跳过
4. 取消提交
```

选项行为：

- 检查并更新 feature-memory：读取 `git diff --stat`、`git diff --name-only` 和相关 feature 记忆；如果变更影响目标、设计、状态、风险、debug 结论或交接信息，更新对应 `index.md`、`handoff.md`、`progress.md`、`design.md`、`decision.md` 或 `bug-index.md`。
- 只检查，不修改：读取变更摘要和相关记忆，只给出“建议更新 / 无需更新”的判断，不改文件。
- 本次是低风险提交，跳过：适用于格式化、拼写、注释、README 微调、无行为变化的小提交；简短确认后继续提交。
- 取消提交：停止 git 提交流程，不修改记忆文件。

判断规则：

- 如果 diff 涉及功能行为、接口、数据库、架构、配置语义、Bug 修复、debug 结论或测试结果，建议更新 feature-memory。
- 如果 diff 只涉及格式化、拼写、注释或不影响 feature 状态的低风险变更，可以跳过。
- 如果提交包含 Bug 修复，必须检查对应 Bug 的 `conclusion.md` 和 `feature-memory/bug-index.md` 是否需要更新。
- 如果提交包含 feature 进展，必须检查 `progress.md`、`handoff.md` 和 `index.md` 是否需要更新。

## 启动流程

处理适用任务时：

1. 定位 `<project-root>/feature-memory/`。
2. 如果目录不存在，按最小结构初始化。
3. 判断任务类型，按“路由总则”读取必要文档。
4. 恢复项目级背景和目标 feature 状态。
5. 对照当前代码验证关键事实，把不确定项写入“假设”或“待确认问题”。
6. 向用户简短确认当前目标、下一步动作和风险。
7. 开始开发、排障或文档更新。

## 完成流程

任务完成前检查并按需更新：

- `index.md`：feature 状态、最近更新时间、下一步、父 feature、依赖是否准确。
- `handoff.md`：新的 Agent 是否能在 5 分钟内恢复上下文。
- `progress.md`：当前状态、已完成项、阻塞项和下一步是否准确。
- `design.md`：设计、接口、数据结构或边界是否变化。
- `decision.md`：是否产生新的重要设计决策或取舍。
- `bug-index.md`：Bug 结束后是否登记根因分类、关键词和入口文档。

## 成功标准

1. feature 开发过程可追溯。
2. Bug 排查过程可收敛。
3. 不重复实现功能。
4. 不重复排查问题。
5. 新 Agent 可快速接手。
6. 上下文丢失后可快速恢复。
7. 文档始终与代码、测试结果和用户确认保持一致。
