# Project Architecture

## 概览

示例项目是一个用于 AI 外呼通话观测的 Web 管理后台。

## 分层

- 前端：路由、页面、API client、WebSocket 状态和详情抽屉。
- 后端 handler：HTTP 和 WebSocket 路由。
- Service 层：查询编排、响应组装、校验和脱敏。
- 数据源：通话会话表、通话事件表、转写存储、质检结果存储和对象存储。

## 关键边界

- 列表接口只返回轻量字段。
- 详情接口按需加载大 JSON 字段。
- 录音文件必须通过带鉴权的后端代理读取。
- WebSocket 列表刷新不推送转写、质检 JSON、事件 payload 或音频内容。
- 敏感信息在写入 feature memory 前必须脱敏。
