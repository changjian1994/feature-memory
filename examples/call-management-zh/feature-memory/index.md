# Feature Memory Index

| Feature | 工作状态 | 记忆状态 | 最近更新 | 负责人 | 父 feature | 依赖 | 下一步动作 | 入口 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| call-management | VERIFYING | VERIFYING | 2026-06-09 | agent/user | - | call-data-platform | 验证真实数据、录音播放和刷新行为。 | `feature-memory/call-management/handoff.md` |

## 状态说明

- `工作状态` 描述研发进度。
- `记忆状态` 控制 Agent 默认读取范围。
- 新 Agent 默认只读取 `ACTIVE` 和 `VERIFYING` 记忆，除非用户要求查看归档历史。

## Feature 边界

- `call-management` 负责通话会话的只读观测能力。
- 挂断、重试、补偿和数据修复不属于当前 feature 范围。
