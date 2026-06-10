# Feature 关系管理参考

## 使用时机

创建、继续、拆分或聚合 feature 前，先读取 `feature-memory/index.md`，再按本文判断是否应复用、合并、拆分或建立父子关系。

## 核心原则

遵循“尽可能不增加重复目录，但不让一个 feature 臃肿”的原则：

- 一个 feature 可以很小，但必须足够独立。
- 高度重复的 feature 优先合并，避免重复目录。
- 部分重叠但职责不同的 feature 应保持独立，并记录关系。
- 大 feature 可以作为聚合入口，但不要复制子 feature 的详细文档。

## 关系类型

- **父子关系**：一个 feature 是另一个 feature 的子任务，例如 `user-profile` 包含 `user-profile-avatar-upload`。
- **依赖关系**：一个 feature 依赖另一个 feature 完成，例如 `order-checkout` 依赖 `payment-gateway`。
- **交叉关系**：两个 feature 共享部分实现或数据，例如 `cart` 和 `wishlist` 都访问商品数据。
- **重复关系**：两个 feature 目标相同或高度重叠。

## 新建前检查

创建新 feature 或 Bug 前：

1. 读取 `feature-memory/index.md`。
2. 比较现有 feature 的名称、目标、范围、父 feature、依赖和下一步动作。
3. 如果是 debug 任务，同时读取 `feature-memory/bug-index.md` 检索同类 Bug。
4. 如果新任务与已有 feature 重叠超过 60%，优先建议合并到已有 feature。
5. 如果只是部分重叠或涉及不同模块，创建独立的小 feature，并在 `index.md` 中记录关系。
6. 如果新任务是一个更大的聚合目标，创建聚合 feature，并把现有 feature 作为子 feature 导航。

## 决策规则

| 场景 | 处理方式 |
| --- | --- |
| 目标、范围、交付物基本相同 | 合并到已有 feature |
| 名称相似但模块、接口或验收目标不同 | 独立 feature，并记录交叉关系 |
| 新 feature 是已有 feature 的一部分 | 新建子 feature，设置父 feature |
| 新 feature 覆盖多个已有 feature | 新建聚合 feature，使用索引树导航子 feature |
| Bug 与已有 Bug 根因分类和关键词相同 | 优先复用已有 Bug 结论，再决定是否新建 Bug |

## 聚合导航机制

当需要创建一个大的聚合 feature，例如 `user-profile`，而现有 `user-profile-avatar-upload`、`user-profile-settings` 可以作为其子 feature 时：

1. 创建聚合 feature 目录：`feature-memory/user-profile/`。
2. 在聚合 feature 的 `readme.md` 中列出子 feature 索引树。
3. 在 `feature-memory/index.md` 中为子 feature 设置父 feature 为聚合 feature。
4. 子 feature 的实际文档仍保留在各自独立目录中，不重复存储。
5. 聚合 feature 只记录跨子 feature 的总体目标、设计协调、共享约束和统一风险。

## 聚合 readme 示例

聚合 feature 的 `readme.md` 应包含以下章节：

- `# user-profile`
- `## 目标`
- `## 子 Feature 索引树`
- `## 跨子 Feature 设计`
- `## 包含`
- `## 不包含`

子 Feature 索引树示例：

```text
├── user-profile-avatar-upload - IN_PROGRESS
│   └── 入口：feature-memory/user-profile-avatar-upload/handoff.md
├── user-profile-settings - TODO
│   └── 入口：feature-memory/user-profile-settings/handoff.md
└── user-profile-privacy - BLOCKED
    └── 入口：feature-memory/user-profile-privacy/handoff.md
```

跨子 Feature 设计示例：

- 统一用户资料 API 前缀。
- 统一权限校验方式。
- 统一头像、昵称、隐私配置的缓存策略。

包含：

- 跨子 feature 的协调和总体设计。
- 子 feature 索引和导航。

不包含：

- 子 feature 的具体实现细节。
- 子 feature 的逐项进度跟踪。

## index.md 记录要求

`feature-memory/index.md` 至少包含以下关系字段：

```md
| Feature | 状态 | 最近更新 | 负责人 | 父 feature | 依赖 | 下一步动作 | 相关文档 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| user-profile | IN_PROGRESS | 2026-06-10 | agent/user | - | - | 协调子 feature | feature-memory/user-profile/handoff.md |
| user-profile-avatar-upload | IN_PROGRESS | 2026-06-10 | agent/user | user-profile | - | 完成上传验证 | feature-memory/user-profile-avatar-upload/handoff.md |
```

## feature readme 边界要求

每个 feature 的 `readme.md` 必须包含：

- **包含**：此 feature 负责的功能范围、核心交付物、关键接口或模块。
- **不包含**：不在此 feature 范围内的功能、由其他 feature 负责的部分、明确排除的边界。
- **依赖**：父 feature、前置 feature、外部系统依赖。

## 用户确认

以下情况必须向用户确认后再创建目录：

- 是否将新任务合并到已有 feature。
- 是否把现有 feature 挂到新聚合 feature 下。
- 是否拆分一个过大的 feature。
- 是否归档或合并重复 feature。
