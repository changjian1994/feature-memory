# 示例：通话管理

这个示例展示 `feature-memory` 如何帮助 Agent 在不重读完整对话历史的情况下，继续推进一个长期 feature。

场景是一个脱敏后的“AI 外呼通话管理后台”。这个 feature 从只读通话列表开始，逐步扩展到通话详情、事件时间线、转写、质检、录音播放、WebSocket 实时刷新，以及一次生产风格的录音播放问题排查。

## 这个示例展示什么价值

- 新 Agent 可以从 `feature-memory/ai-handoff.md` 开始，而不是扫描所有文件。
- feature 边界清晰：第一期只做只读观测，不做挂断、重试、补偿等控制动作。
- `handoff.md` 记录当前接手视图，`progress.md` 记录过程流水。
- `decision.md` 记录关键取舍，方便后续理解为什么这么做。
- `bug-index.md` 和 `debug/.../conclusion.md` 把排障经验沉淀为可复用知识。
- 示例保留架构、决策和排障模式，但移除了域名、密钥、生产日志和用户数据。

## 示例结构

```text
examples/call-management-zh/
└── feature-memory/
    ├── ai-handoff.md
    ├── index.md
    ├── bug-index.md
    ├── project/
    │   └── architecture.md
    └── call-management/
        ├── readme.md
        ├── design.md
        ├── decision.md
        ├── progress.md
        ├── handoff.md
        └── debug/
            └── BUG-20260609-001-recording-playback-auth/
                └── conclusion.md
```

## 推荐阅读顺序

1. 先读 [`feature-memory/ai-handoff.md`](feature-memory/ai-handoff.md)，理解项目级当前状态。
2. 再读 [`feature-memory/index.md`](feature-memory/index.md)，查看 feature 状态和入口。
3. 阅读 [`feature-memory/call-management/handoff.md`](feature-memory/call-management/handoff.md)，恢复单个 feature 的当前接手视图。
4. 阅读 [`feature-memory/call-management/decision.md`](feature-memory/call-management/decision.md)，理解关键技术取舍。
5. 阅读 [`feature-memory/bug-index.md`](feature-memory/bug-index.md) 和 debug 结论，理解一次 Bug 如何变成可复用知识。

## 隐私说明

本示例已经泛化和脱敏。产品名、域名、数据库名、对象存储 key、Token、Cookie、客户数据和生产日志都已移除或替换为中性表达。
