# Memory Lifecycle 参考

## 使用时机

当项目中的 feature memory 变多、用户准备交接、提交代码、归档 feature，或 Agent 需要恢复上下文时，使用本文控制读取范围和更新成本。

## 核心目标

记忆系统必须会收敛：

- 先读全局首读入口，而不是扫描所有 feature。
- 默认只读取活跃或验证中的记忆。
- 小改动默认不写 memory。
- 单次任务限制更新文件数量。
- 完成的 feature 要归档压缩，不持续扩写。
- 避免 `progress.md`、`handoff.md`、`index.md` 写重复结论。

## AI 首读入口

`feature-memory/ai-handoff.md` 是项目级首读入口。新的 Agent 接手项目时，优先读取它，再按需读取具体 feature。

建议包含：

```md
# AI Handoff

## 当前活跃 Feature

| Feature | 记忆状态 | 工作状态 | 下一步 | 入口 |
| --- | --- | --- | --- | --- |
| payment-callback | ACTIVE | IN_PROGRESS | 实现重试机制 | feature-memory/payment-callback/handoff.md |

## 当前验证中 Bug

| Bug ID | 状态 | 关键词 | 入口 |
| --- | --- | --- | --- |
| BUG-20260609-001-browser-audio-401 | VERIFYING | browser, 401, audio | feature-memory/call-management/debug/BUG-20260609-001-browser-audio-401/conclusion.md |

## 最近重要决策

## 当前风险

## 下一步建议

## 归档提示

- 默认不要读取 ARCHIVED / LEGACY feature，除非用户要求或当前任务命中历史关键词。
```

## 记忆状态

`feature-memory/index.md` 中使用“记忆状态”字段控制读取范围：

| 记忆状态 | 含义 | 默认读取 |
| --- | --- | --- |
| ACTIVE | 正在开发或近期会继续推进 | 是 |
| VERIFYING | 已实现但仍在验证、观察或收敛 | 是 |
| ARCHIVED | 已完成并压缩为摘要 | 否 |
| LEGACY | 历史遗留，仅作参考 | 否 |

工作状态和记忆状态是两个概念：

- 工作状态描述任务进展，如 `TODO`、`IN_PROGRESS`、`BLOCKED`、`VERIFYING`、`DONE`、`ARCHIVED`。
- 记忆状态描述 Agent 是否应默认读取，如 `ACTIVE`、`VERIFYING`、`ARCHIVED`、`LEGACY`。

## 默认读取规则

- 启动或恢复时先读 `feature-memory/ai-handoff.md`。
- 再读 `feature-memory/index.md` 中 `ACTIVE` 和 `VERIFYING` 的 feature。
- 不默认读取 `ARCHIVED` 或 `LEGACY`，除非用户明确要求、关键词命中 `bug-index.md`，或当前任务依赖历史结论。
- 查看 feature 列表时只展示摘要，不展开所有 feature 文档。

## 小改动跳过规则

以下任务默认不创建或更新 memory，除非用户明确要求：

- 纯样式改动。
- 文案调整。
- 格式化。
- 注释修正。
- 单文件低风险修复。
- 很容易从代码读懂的小功能。
- 没有后续交接价值的一次性任务。

以下任务应更新 memory：

- 影响接口、数据库、权限、架构或配置语义。
- 改变 feature 目标、状态、风险或交接信息。
- 产生重要设计决策或取舍。
- 修复复杂 Bug 或更新 debug 结论。
- 线上风险、回滚策略、验证方式发生变化。

## 单次更新限额

默认单次任务最多更新 1-3 个 memory 文件。

优先级：

1. `handoff.md`：当前接手视图变化。
2. `progress.md`：阶段、状态、下一步变化。
3. `decision.md`：重要决策变化。
4. `bug-index.md` / `conclusion.md`：Bug 根因或状态变化。
5. `ai-handoff.md` / `index.md`：全局导航或状态变化。

例外情况：

- 首次初始化 feature 或 Bug。
- 用户明确要求全量同步。
- 归档压缩。
- 重大架构迁移或多 feature 聚合调整。

## 文件职责边界

- `ai-handoff.md`：项目级首读入口，只写当前活跃事项、风险和导航。
- `index.md`：全局索引，只写状态、父子关系、依赖、下一步和入口。
- `handoff.md`：单个 feature 的当前接手视图，只写“现在应该知道什么”。
- `progress.md`：过程流水和状态变化，只追加关键阶段，不复制 handoff 的完整结论。
- `decision.md`：重要决策、原因、备选方案和证据。
- `bug-index.md`：跨 feature 的 Bug 检索入口。
- `conclusion.md`：单个 Bug 的当前最新结论。

## 归档压缩

当 feature 达到 `DONE` 且不再频繁变化时：

1. 将 `handoff.md` 更新为最终摘要。
2. 将 `index.md` 的记忆状态改为 `ARCHIVED`。
3. 将 `ai-handoff.md` 中的活跃列表移除或转入归档提示。
4. 停止持续扩写 `progress.md`，除非后续重新激活。
5. 复杂 Bug 保留 `bug-index.md` 和 `conclusion.md` 入口，避免未来重复排查。

当历史 feature 只剩参考价值时，可标记为 `LEGACY`。
