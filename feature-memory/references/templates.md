# Feature Memory 模板

## 命名规则

- feature 目录名使用 kebab-case，例如 `payment-callback`。
- Bug 目录名使用 `BUG-YYYYMMDD-XXX-description`，例如 `BUG-20250101-001-login-timeout`。
- 日期默认使用 `YYYY-MM-DD`，需要精确时间时再使用时间戳。
- 未确认信息必须写入“待确认问题”或“假设”，不能写成事实。

## feature-memory/index.md

```md
| Feature | 状态 | 最近更新 | 负责人 | 下一步动作 | 相关文档 |
| --- | --- | --- | --- | --- | --- |
| payment-callback | IN_PROGRESS | 2025-01-01 | agent/user | 实现重试机制 | feature-memory/payment-callback/handoff.md |
```

允许的状态值：`TODO`、`IN_PROGRESS`、`BLOCKED`、`VERIFYING`、`DONE`、`ARCHIVED`。

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

## 依赖

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
