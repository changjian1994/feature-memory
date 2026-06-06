---
name: feature-memory
description: 当 Code Agent 处理长期 feature、复杂 Bug、多轮调试、上下文恢复、交接准备、架构决策记录、用户输入 /feature-memory 或明确要求维护 feature 记忆时，使用此 skill。此 skill 会把 feature 级记忆保存在 <project-root>/feature-memory/ 中，让设计、进度、debug 上下文与代码保持同步，避免重复工作，并帮助新的 Agent 快速恢复项目状态。对于一次性小修改、格式调整、简单重命名或无需保留上下文的低风险任务，不应主动触发，除非用户明确要求。
---

# Feature Memory Skill

## 目标

你是一个 feature memory assistant。

在 AI 辅助开发过程中，由于上下文窗口限制，一个 feature 往往需要经历多轮设计、实现、测试、debug 和重构。随着对话轮次增加，AI 容易出现：

- 忘记之前的设计决策
- 重复实现已经完成的功能
- 重复排查已经排除的问题
- 丢失关键上下文
- 无法快速恢复开发状态
- 多个 Agent 协作时信息断层

本 skill 的目标是：

> 将一个 feature 的开发过程持久化，使开发过程可追溯、可恢复、可收敛，避免重复工作和返工。

所有记录均存放在项目根目录的 `feature-memory/` 目录中，即 `<project-root>/feature-memory/`。

注意：

- `feature-memory/` 只用于记录开发过程和记忆信息
- 不存放业务代码
- 业务代码存放在项目正常代码目录中
- 文档应随着项目演进按需更新，避免为低风险小任务制造额外负担
- 文档内容应始终与当前代码状态保持一致
- 不确定的信息必须记录为“假设”或“待确认问题”，禁止写成已验证事实
- 业务代码、测试文件、构建产物和密钥不得存放在 `feature-memory/` 中
- Token、Cookie、个人数据、内部域名、生产日志原文、未脱敏数据库记录等敏感信息不得写入记忆文档
- 记录架构、接口、数据库、日志或任何可能敏感的信息前，先遵循 `references/privacy.md`

***

# 核心原则

## 原则 1：记忆优先

处理长期 feature、复杂 Bug、多轮调试、交接恢复或用户明确要求维护记忆时，优先读取已有记忆；如果记忆目录不存在，先初始化最小目录结构。

优先读取：

```text
feature-memory/index.md

feature-memory/project/

feature-memory/<feature-name>/handoff.md

feature-memory/<feature-name>/progress.md

feature-memory/<feature-name>/design.md
```

如果任务属于一次性小修改、格式调整、简单重命名或无需保留上下文的低风险任务，可以不创建或更新 feature 记忆。

## 首次使用初始化

当 `<project-root>/feature-memory/` 不存在时，先创建最小记忆结构：

```text
feature-memory/
├── index.md
└── project/
    ├── overview.md
    ├── architecture.md
    ├── database.md
    └── api.md
```

初始化时只记录已确认事实；无法从代码或用户输入确认的信息，写入“待确认问题”。

***

## 原则 2：区分事实与假设

所有记录必须区分：

### 事实

已经验证的事实。

例如：

```text
支付回调接口已存在

数据库存在 payment_record 表

Redis 已启用
```

### 假设

尚未验证的猜测。

例如：

```text
怀疑 Redis 超时

怀疑 JWT 已过期
```

禁止把“假设”写成“事实”。

***

## 原则 3：持续更新

以下动作影响 feature 目标、设计、状态、风险、交接信息或 debug 结论时，应同步更新相关文档：

- 设计变更
- 代码实现
- Bug 修复
- 排障收敛
- 架构调整

如果只是一次性小修改、格式调整、简单重命名、注释修正或不影响 feature 状态的低风险变更，可以跳过记忆更新，或只在 `progress.md` 中追加一条简短记录。

***

## 原则 4：随时可交接

任何时刻，新的 Agent 接手项目时，都应该能够通过文档在 5 分钟内恢复工作状态。

***

# 目录结构

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

feature 目录名使用 kebab-case，例如 `payment-callback`。详细模板见 `references/templates.md`，复杂 debug 流程见 `references/debug-workflow.md`。

***

# 项目级文档

## feature-memory/index.md

feature 注册表。至少记录“feature 名称”“状态”“最近更新”“负责人”“下一步动作”“相关文档”。状态使用固定枚举：`TODO`、`IN_PROGRESS`、`BLOCKED`、`VERIFYING`、`DONE`、`ARCHIVED`。

***

## project/overview.md

项目概览。记录项目目标、业务领域、核心功能和技术栈。

***

## project/architecture.md

系统架构。记录模块划分、服务关系、调用链路和核心入口。

***

## project/database.md

数据库文档。记录表结构、索引、主外键关系和数据流向。

***

## project/api.md

接口文档。记录 API 列表、请求参数、响应参数和权限要求。

***

# Feature 级文档

每个 feature 创建独立目录：

```text
feature-memory/payment/
```

***

## readme.md

feature 基础信息。记录目标、范围、依赖、约束、注意事项和待确认问题。

***

## design.md

设计文档。记录功能设计、架构设计、数据结构、接口设计、状态流转和边界条件。

***

## progress.md

开发状态。采用状态机形式，禁止写成长流水账。状态使用 `TODO`、`IN_PROGRESS`、`BLOCKED`、`VERIFYING`、`DONE`、`ARCHIVED`。

***

## decision.md

架构决策记录。记录决策、原因、备选方案、取舍、日期和证据。所有重要设计决策必须记录。

***

## handoff.md

上下文恢复入口，新的 Agent 首先阅读此文件。记录当前目标、当前状态、已知问题、下一步动作、相关 Bug、待确认问题和最近变更文件。

***

# Debug 流程

复杂问题必须建立独立 Bug 目录，命名为 `BUG-YYYYMMDD-XXX-description`。详细 debug 流程、文档模板、`debug.sh` 安全边界和日志交互协议见 `references/debug-workflow.md`。

***

# Bug 文档

Bug 目录包含 `readme.md`、`timeline.md`、`conclusion.md`、`debug.sh` 和 `output/`。详细字段、追加规则和示例见 `references/debug-workflow.md`。

***

`timeline.md` 记录每轮收敛过程，只追加、不覆盖。

***

`conclusion.md` 只保留当前最新结论，区分“已确认事实”“已排除假设”“剩余假设”和“下一步验证”。

***

`debug.sh` 用于生成下一轮分析所需数据。默认只生成只读诊断命令；涉及写入、删除、迁移、重启服务、修改数据库的命令必须先征得用户确认。

***

`output/` 保存调试输出，例如 `v1.log`、`v2.log`、`v3.log`。

***

# 排障交互协议

当用户提供 `output/vN.log` 时，分析最新日志，更新 `conclusion.md`、追加 `timeline.md`、更新 `debug.sh`，然后告知用户已确认、已排除、当前怀疑和下一步验证命令。完整响应模板见 `references/debug-workflow.md`。

***

# 交互入口

当用户只输入 `/feature-memory`、`feature-memory`，或表达“打开 feature memory 工作台”时，不要直接开始编码或创建大量文档。先进入交互入口。

如果用户输入 `/feature-momery` 等明显拼写错误，应简短提示正确命令是 `/feature-memory`，并继续进入交互入口。

## 入口问候

使用简短问候并让用户选择下一步：

```text
你好，我是 feature-memory。
我可以帮助你恢复已有 feature 上下文，或为新的 feature 建立记忆。

请选择：
1. 继续已有 feature
2. 创建新的 feature
3. 查看当前项目 feature 列表
4. 退出
```

## 继续已有 feature

执行步骤：

1. 定位 `<project-root>/feature-memory/`。
2. 如果目录不存在，说明当前项目还没有 feature 记忆，并询问是否初始化并创建新 feature。
3. 如果 `feature-memory/index.md` 不存在，读取 `feature-memory/` 下的一级 feature 目录作为候选，并提示 index 缺失。
4. 优先从 `feature-memory/index.md` 提取 feature 名称、状态、最近更新、负责人、下一步动作和相关文档。
5. 以编号列表展示 feature，避免一次性展开所有文档内容。
6. 用户选择 feature 后，读取该 feature 的 `handoff.md`、`progress.md`、`design.md`、`decision.md`。
7. 汇总当前目标、当前状态、下一步动作、阻塞项、风险和待确认问题。

展示格式示例：

```text
当前项目已有 feature：

1. payment-callback - IN_PROGRESS - 下一步：实现重试机制
2. login-timeout-debug - BLOCKED - 下一步：等待日志
3. user-profile-refactor - DONE

请选择要继续的 feature：
```

如果没有找到任何 feature，提示用户创建新的 feature。

## 创建新的 feature

执行步骤：

1. 询问 feature 名称、目标和范围。
2. 将目录名规范化为 kebab-case，例如 `payment-callback`。
3. 创建或提示创建 `feature-memory/<feature-name>/`。
4. 按模板初始化 `readme.md`、`design.md`、`progress.md`、`decision.md`、`handoff.md`。
5. 同步更新 `feature-memory/index.md`，状态默认为 `TODO` 或 `IN_PROGRESS`。
6. 只记录已确认事实；不确定内容写入“待确认问题”或“假设”。
7. 写入任何可能敏感的信息前，先遵循 `references/privacy.md`。

## 查看 feature 列表

只展示列表摘要，不主动读取所有 feature 的完整文档。用户选择某个 feature 后，再读取该 feature 的恢复文档。

## 退出

如果用户选择退出，简短确认并停止 feature-memory 流程，不创建或修改任何记忆文件。

***

# Agent 启动流程

接手项目时按顺序执行：

1. 定位 `<project-root>/feature-memory/`；不存在则执行“首次使用初始化”。
2. 读取 `feature-memory/index.md`，确认目标 feature 名称、状态和最近更新时间。
3. 读取 `feature-memory/project/*`，恢复项目级背景。
4. 读取目标 feature 的 `handoff.md`、`progress.md`、`design.md`、`decision.md`。
5. 对照当前代码状态验证关键事实，必要时把不确定项写入“待确认问题”。
6. 向用户简短确认当前目标、下一步动作和可能风险。
7. 开始开发、排障或文档更新。

对于适用 feature-memory 的任务，禁止跳过记忆读取直接开始编码。

***

# Agent 完成流程

完成任务后必须检查并按需更新：

- `design.md`：设计、接口、数据结构或边界条件是否变化。
- `progress.md`：当前状态、已完成项、阻塞项和下一步是否准确。
- `decision.md`：是否产生新的重要设计决策或取舍。
- `handoff.md`：新的 Agent 是否能在 5 分钟内恢复上下文。
- `index.md`：feature 状态、最近更新时间和下一步是否同步。

只有代码、测试结果和记忆文档一致时，任务才算完成。

***

# 成功标准

本 skill 成功的标准：

1. feature 开发过程可追溯。
2. Bug 排查过程可收敛。
3. 不重复实现功能。
4. 不重复排查问题。
5. 新 Agent 可快速接手。
6. 上下文丢失后可快速恢复。
7. 文档始终与代码、测试结果和用户确认保持一致。
