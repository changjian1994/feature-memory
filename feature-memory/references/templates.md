# Feature Memory 模板

## 命名规则

- feature 目录名使用 kebab-case，例如 `payment-callback`。
- Bug 目录名使用 `BUG-YYYYMMDD-XXX-description`，例如 `BUG-20250101-001-login-timeout`。
- 日期默认使用 `YYYY-MM-DD`，需要精确时间时再使用时间戳。
- 未确认信息必须写入“待确认问题”或“假设”，不能写成事实。

## feature-memory/index.md

```md
| Feature | 状态 | 最近更新 | 负责人 | 父 feature | 依赖 | 下一步动作 | 相关文档 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| payment-callback | IN_PROGRESS | 2025-01-01 | agent/user | - | - | 实现重试机制 | feature-memory/payment-callback/handoff.md |
| payment-callback-retry | IN_PROGRESS | 2025-01-02 | agent/user | payment-callback | - | 完成指数退避 | feature-memory/payment-callback-retry/handoff.md |
| order-checkout | TODO | 2025-01-01 | agent/user | - | payment-callback | 等待支付模块 | feature-memory/order-checkout/handoff.md |
```

允许的状态值：`TODO`、`IN_PROGRESS`、`BLOCKED`、`VERIFYING`、`DONE`、`ARCHIVED`。

### 关系字段说明

- **父 feature**：如果当前 feature 是某个父 feature 的子任务，填写父 feature 名称；否则填 `-`
- **依赖**：列出当前 feature 依赖的其他 feature，用逗号分隔；否则填 `-`

## feature-memory/bug-index.md（全局 Bug 知识库索引）

> 每个 Bug 解决后，必须在此登记一条记录。遇到相同或相似问题时，AI 先检索此索引，避免重复排查。

```md
| Bug ID | 标题 | 根因分类 | 关键词 | 所属 feature | 状态 | 最近更新 | 入口文档 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| BUG-20260609-001-browser-audio-401 | 浏览器录音无法播放（401） | auth/browser-native-request | browser, 401, audio, X-Jwt-Token, axios-blob, ObjectURL | call-management | RESOLVED | 2026-06-09 | feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/conclusion.md |
```

### 根因分类建议（Tags

- auth/browser-native-request
- auth/jwt-token-missing
- data/tcc-misconfig
- api/frontend-deploy-not-updated
- infra/object-storage
- db/query
- frontend/cors
- perf/timezone

> 根因分类和关键词用于相似问题的语义匹配，标签建议覆盖：浏览器行为 / 鉴权方式 / 组件名 / 错误码 / 关键文件名等信息，便于后续匹配。

### 记录规则

1. 每个 RESOLVED 的 Bug 必须登记一条
2. 关键词覆盖：根因、错误码、核心组件名
3. 「所属 feature 指向对应 `<feature-name>/debug/BUG-XXX/conclusion.md`
4. BLOCKED 或 VERIFYING 的 Bug 也要登记，方便接力

## project/overview.md

```md
# 项目概览

## 项目目标

## 业务领域

## 核心功能

## 技术栈

## 待确认问题
```

## project/architecture.md

```md
# 系统架构

## 模块划分

## 服务关系

## 调用链路

## 核心入口

## 约束条件

## 待确认问题
```

## project/database.md

```md
# 数据库

## 数据表

## 索引

## 关系

## 数据流向

## 待确认问题
```

## project/api.md

```md
# 接口

## 接口列表

## 请求参数

## 响应参数

## 权限要求

## 待确认问题
```

## feature-memory/<feature-name>/readme.md

```md
# <Feature 名称>

## 目标

## 范围

## 包含

- 此 feature 负责的功能范围
- 核心交付物
- 关键接口/模块

## 不包含

- 不在此 feature 范围内的功能
- 由其他 feature 负责的部分
- 明确排除的边界

## 依赖

- 父 feature（如有）
- 前置依赖的其他 feature
- 外部服务/系统依赖

## 约束

## 注意事项

## 待确认问题
```

## feature-memory/<aggregate-feature-name>/readme.md（聚合 feature）

当一个 feature 作为多个子 feature 的聚合容器时，使用此模板：

```md
# <聚合 Feature 名称>

## 目标

## 范围

## 子 Feature 索引树

```text
├── <子 feature 1> - <状态>
│   └── 入口：feature-memory/<子 feature 1>/handoff.md
├── <子 feature 2> - <状态>
│   └── 入口：feature-memory/<子 feature 2>/handoff.md
├── <子 feature 3> - <状态>
│   └── 入口：feature-memory/<子 feature 3>/handoff.md
└── <子 feature 4> - <状态>
    └── 入口：feature-memory/<子 feature 4>/handoff.md
```

## 跨子 Feature 设计

- 总体架构协调
- 子 feature 间的接口约定
- 共享数据和状态管理
- 统一的错误处理策略

## 包含

- 跨子 feature 的协调和总体设计
- 子 feature 索引和导航
- 不重复存储子 feature 的详细文档

## 不包含

- 子 feature 的具体实现细节（由各子 feature 自行维护）
- 子 feature 的进度跟踪（由各子 feature 自行维护）

## 约束

## 注意事项

## 待确认问题
```

## feature-memory/<feature-name>/design.md

```md
# 设计

## Feature 设计

## 架构设计

## 数据结构

## 接口设计

## 状态流转

## 边界条件

## 待确认问题
```

## feature-memory/<feature-name>/progress.md

```md
# 进度

## 状态

IN_PROGRESS

## 当前阶段

## 已完成

- ...

## 进行中

- ...

## 阻塞项

- ...

## 下一步动作

- ...
```

## feature-memory/<feature-name>/decision.md

```md
# 决策记录

## YYYY-MM-DD：<决策标题>

### 决策

### 原因

### 备选方案

### 取舍

### 证据
```

## feature-memory/<feature-name>/handoff.md

```md
# 交接

## 当前目标

## 当前状态

## 已知问题

## 下一步动作

## 相关 Bug

## 最近变更文件

## 待确认问题
```

## Bug 文档

详细 Bug debug 流程和调试脚本规则见 `debug-workflow.md`。

### feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/readme.md

```md
# BUG-YYYYMMDD-XXX-description

## 问题

## 影响范围

## 复现步骤

## 预期结果

## 实际结果

## 待确认问题
```

### feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/timeline.md

```md
# Debug Timeline

## 第 1 轮

### 假设

### 验证方式

### 结果

已确认 | 已排除 | 暂无结论

### 证据

### 下一步
```

### feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/conclusion.md

```md
# Debug Conclusion

## 记忆索引

- 关联记忆索引：`feature-memory/bug-index.md`
- 本 Bug ID：`BUG-YYYYMMDD-XXX-description`
- 根因分类：`auth/browser-native-request` 或其他分类（见 `feature-memory/bug-index.md`）
- 关键词：`browser, 401, audio, X-Jwt-Token, axios-blob, ObjectURL`

## 前置知识 / 参考技能

> 列出理解或复现本问题所需的前置信息，指向 `feature-memory/project/` 或其他 skill 文档；若无则写 `-`。

- 依赖前置知识：`feature-memory/project/api.md`（JWT 鉴权流程）、`feature-memory/project/architecture.md`（TOS 初始化）
- 同类问题参考：检索 `feature-memory/bug-index.md` 中相同 `根因分类` 或 `关键词`

## 当前发现

## 已确认事实

## 已排除假设

## 剩余假设

## 下一步验证

## 待确认问题
```

### feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/debug.sh

```sh
#!/usr/bin/env sh
set -eu

# 只放只读诊断命令。不得输出密钥、Token、Cookie 或个人敏感信息。
mkdir -p output

LOG_FILE="output/v1.log"

{
  echo "Debug log: ${LOG_FILE}"
  echo "Generated at: $(date)"
  echo

  # TODO: 在这里填写只读诊断命令。
  # 示例：
  # echo "Current branch:"
  # git branch --show-current
} > "${LOG_FILE}" 2>&1

echo "Debug output written to ${LOG_FILE}"
```
