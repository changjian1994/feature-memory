# AI Handoff

## 当前活跃 Feature

| Feature | 记忆状态 | 工作状态 | 下一步 | 入口 |
| --- | --- | --- | --- | --- |
| call-management | VERIFYING | VERIFYING | 使用真实通话数据验证列表、详情和录音播放。 | `feature-memory/call-management/handoff.md` |

## 项目背景

- 项目是一个用于观测 AI 外呼通话会话的管理后台。
- 第一期目标是只读观测：通话列表、通话详情、事件时间线、转写、质检、录音和刷新状态。
- Agent 不应在第一期加入挂断、重试、补偿等控制动作，除非权限、审计和幂等设计已经明确。

## 当前风险

- 录音播放必须通过带鉴权的后端代理；浏览器原生资源请求可能不会携带鉴权头。
- 大 JSON、转写、质检和录音元数据应按需加载，不应进入列表查询。
- 不要在 memory 中记录手机号明文、对象存储 key、Token、Cookie 或生产日志原文。

## 最近可复用知识

- 列表和详情基础信息以 `call_session` 为主；`call_events` 只用于事件时间线和录音元数据。
- 转写、质检和录音统一通过按需 `insights` 接口聚合。
- 如果浏览器播放录音出现 `401`，优先检查是否使用了原生 `<audio>` 或 `window.open`，而不是带鉴权拦截器的 HTTP client。
