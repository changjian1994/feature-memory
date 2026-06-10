# Feature Memory Skill

让 AI 辅助开发中的 feature 上下文可追溯、可恢复、可交接。

一个面向 Code Agent 的 feature 级记忆 skill，用于在多轮开发、调试、交接和恢复上下文时，把关键设计、进度、决策和排障过程持久化到项目中，减少重复工作和上下文丢失。

适用于支持 Markdown-based Skill 或目录型 skill 包的 Code Agent / IDE Agent。

## 解决什么问题

AI 辅助开发经常会遇到这些情况：

- 长 feature 做到一半后，Agent 忘记之前的设计决策。
- 新会话接手项目时，需要重新解释背景、进度和阻塞点。
- Bug 排查经过多轮假设和验证后，历史证据散落在对话中。
- 多个 Agent 协作时，无法快速确认“已完成什么、还差什么、为什么这么做”。

`feature-memory` 不是普通文档模板，而是一套给 Code Agent 使用的上下文记忆工作流。它帮助研发团队把长期 feature、复杂 debug 和交接恢复过程沉淀到项目内，让新的 Agent 或同事可以快速理解当前状态，继续推进而不是从头再来。

## 核心能力

- 记忆优先：适用任务开始前读取已有 feature 记忆，避免重复实现和重复排查。
- 事实与假设分离：已验证事实、待确认问题和调试假设分别记录，避免把猜测写成结论。
- 按需同步：当目标、设计、状态、风险、交接信息或 debug 结论变化时更新记忆文档。
- 快速交接：新的 Agent 可以通过 `handoff.md`、`progress.md`、`design.md` 在短时间内恢复状态。
- Debug 收敛：复杂问题使用独立 Bug 目录记录假设、验证、日志和最新结论。

## 仓库结构

```text
.
├── .github/
│   └── ISSUE_TEMPLATE/
├── feature-memory/
│   ├── SKILL.md
│   └── references/
│       ├── debug-workflow.md
│       ├── feature-relationship.md
│       ├── privacy.md
│       ├── readme.md
│       └── templates.md
├── CHANGELOG.md
├── LICENSE
├── PRIVACY.md
├── readme.md
└── readme.en.md
```

- `feature-memory/SKILL.md`：skill 入口文件，采用 Lean Core 路由结构，包含触发边界、硬规则和按需 reference 读取规则。
- `feature-memory/references/templates.md`：项目级、feature 级、Bug 级文档模板。
- `feature-memory/references/debug-workflow.md`：复杂问题排障流程、`debug.sh` 安全规则和日志迭代协议。
- `feature-memory/references/feature-relationship.md`：feature 合并、拆分、父子关系、依赖关系和聚合导航规则。
- `feature-memory/references/privacy.md`：skill 内置隐私、安全边界和脱敏规则。
- `feature-memory/references/readme.md`：参考资料说明和读取时机。
- `.github/ISSUE_TEMPLATE/`：GitHub Issue 模板。
- `CHANGELOG.md`：版本变更记录。
- `LICENSE`：MIT 开源许可证。
- `PRIVACY.md`：隐私、脱敏和目标项目 Git 提交建议。

## 安装方式

这个仓库不包含运行时代码，也不需要安装依赖。将仓库中的 `feature-memory/` 目录作为一个 skill 包导入到你的 Code Agent / IDE Agent 的 skills 目录即可。

常见方式：

```bash
git clone https://github.com/changjian1994/feature-memory.git
```

然后把 `feature-memory/feature-memory` 目录复制或链接到你的 Agent skills 目录。不同 Agent 的 skill 目录位置可能不同，请以你使用的工具文档为准。

## 使用场景

建议按需启用此 skill，不建议对所有普通编码任务无脑全局启用。当 Agent 处理以下任务时，适合触发：

- 实现或继续一个多步骤 feature。
- 恢复之前中断的开发上下文。
- 为 feature 做交接准备。
- 跟踪设计、进度、决策或风险。
- 排查复杂 Bug、测试失败、线上异常或偶现问题。
- 多轮 debug 后需要收敛证据和下一步验证动作。

对于一次性小修改、格式调整、简单重命名或无需保留上下文的低风险任务，通常不需要创建或更新记忆文档。

## 交互入口

用户可以只输入：

```text
/feature-memory
```

Agent 会先给出简短问候，并让用户选择下一步。

如果宿主 Agent 支持原生选择器、Quick Pick 或问题选择组件，会优先使用交互式选择器；如果不支持，则退化为编号文本输入：

```text
你好，我是 feature-memory。
我可以帮助你恢复已有 feature 上下文，或为新的 feature 建立记忆。

请选择：
1. 继续已有 feature
2. 创建新的 feature
3. 查看当前项目 feature 列表
4. 退出
```

如果选择继续已有 feature，Agent 会读取目标项目的 `feature-memory/index.md`，列出已有 feature 供用户选择；用户选定后，再读取对应 feature 的 `handoff.md`、`progress.md`、`design.md` 和 `decision.md` 来恢复上下文。

如果选择创建新的 feature，Agent 会询问 feature 名称、目标和范围，并初始化对应的 `feature-memory/<feature-name>/` 记忆目录。

## 在目标项目中生成的记忆结构

skill 会要求在目标项目根目录维护 `feature-memory/` 目录：

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

注意：目标项目里的 `feature-memory/` 只用于保存开发记忆，不应存放业务代码、测试文件、构建产物、密钥、Token、Cookie、个人数据、内部域名、生产日志原文或未脱敏数据库内容。

## 工作流概览

1. 定位目标项目根目录的 `feature-memory/`。
2. 如果记忆目录不存在，初始化最小项目级文档结构。
3. 读取 `index.md`、`project/*` 和目标 feature 的关键文档。
4. 区分已验证事实、假设和待确认问题。
5. 开始实现、调试、重构或文档更新。
6. 如果 feature 目标、设计、状态、风险、交接信息或 debug 结论发生变化，任务结束前同步更新相关记忆文档。

## Debug 约定

复杂问题会在对应 feature 下建立独立 Bug 目录：

```text
feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/
├── readme.md
├── timeline.md
├── conclusion.md
├── debug.sh
└── output/
```

- 当用户提到 debug、排查、看日志、测试失败、线上异常、偶现问题，或提供 `output/vN.log`、错误堆栈、报警信息时，Agent 应进入 debug 记忆流程。
- 首次进入某个 Bug 的 debug 流程时，Agent 应创建 Bug 目录并初始化 `readme.md`、`timeline.md`、`conclusion.md`、`debug.sh` 和 `output/`。
- 不应把复杂排障只记录在 feature 级 `progress.md` 中。
- `debug.sh` 是一键诊断脚本，每轮应自动生成下一个版本化日志文件，例如 `output/v1.log`、`output/v2.log`、`output/v3.log`。
- 用户只需要执行 `./debug.sh`，然后把生成的 `output/vN.log` 内容原样发回给 Agent。
- `timeline.md` 只追加，不覆盖历史排查过程。
- `conclusion.md` 只保留当前最新结论，并明确已确认事实、已排除假设和剩余假设。
- `debug.sh` 默认只包含只读诊断命令；写入、删除、迁移、重启服务或修改数据库前必须先确认。
- `output/` 保存每轮诊断输出，便于 Agent 基于日志继续收敛；旧日志不得被覆盖。

## Bug 知识库索引（bug-index.md）

每个复杂 Bug 排障结束后，必须在 `feature-memory/bug-index.md` 登记一条记录。它的价值是：AI 下次遇到相同或相似问题时，能先检索 `bug-index.md`，直接复用历史结论和修复路径，避免重复排查。

### 登记字段

- Bug ID：`BUG-YYYYMMDD-XXX-description`
- 标题：一句话概括
- 根因分类：如 `auth/browser-native-request`、`data/tcc-misconfig`
- 关键词：覆盖错误码、组件名、关键行为（如 `browser, 401, audio, X-Jwt-Token`）
- 所属 feature / 状态 / 最近更新 / 入口文档（指向 `conclusion.md`）

### 触发时机

1. 进入 debug 流程时，**必先读** `feature-memory/bug-index.md` 检索同类 Bug
2. 若命中匹配记录，直接告诉用户「已发现类似 Bug」，并以对应 `conclusion.md` 作为起点
3. Bug 结束（RESOLVED / BLOCKED / VERIFYING）后，**必须**更新 `bug-index.md` 登记一条新记录
4. `conclusion.md` 顶部需写入「记忆索引」与「前置知识」区块（见模板）

## Feature 关系管理

当遇到类似、交叉或父子包含关系的 feature 时，skill 会自动检测并建议合适的处理方式：

### 关系类型

- **父子关系**：一个 feature 是另一个 feature 的子任务（如 `user-profile` 包含 `user-profile-avatar-upload`）
- **依赖关系**：一个 feature 依赖另一个 feature 的完成（如 `order-checkout` 依赖 `payment-gateway`）
- **交叉关系**：两个 feature 共享部分实现或数据（如 `cart` 和 `wishlist` 都访问商品数据）
- **重复关系**：两个 feature 目标相同或高度重叠

### 处理策略

遵循「尽可能不增加重复目录，但不让一个 feature 臃肿」原则：

1. **检测重叠**：创建新 feature 前，检查名称、目标、范围是否与现有 feature 重叠超过 60%
2. **优先合并**：如果高度重叠（>60%），优先建议用户合并到已有 feature，而不是新建
3. **独立拆分**：如果只是部分重叠或涉及不同模块，创建独立的小 feature，保持低耦合
4. **建立关系**：如果是父子或依赖关系，在 `index.md` 和 feature `readme.md` 中明确记录
5. **明确边界**：每个 feature 必须定义"包含什么"和"不包含什么"
6. **聚合导航**：当新建一个大 feature 时，如果发现现有多个 feature 可以作为它的子集，创建聚合 feature 目录，通过索引树结构导航到子 feature

### 聚合导航

当需要创建一个大的聚合 feature（如 `user-profile`），而现有 `user-profile-avatar-upload`、`user-profile-settings` 等可以作为其子 feature 时：

- 创建聚合 feature 目录 `feature-memory/user-profile/`
- 在聚合 feature 的 `readme.md` 中列出子 feature 索引树
- 在 `index.md` 中为子 feature 设置父 feature 为聚合 feature
- 子 feature 的实际文档仍保留在各自独立目录中，不重复存储
- 聚合 feature 可以包含跨子 feature 的总体设计和协调信息

### 边界定义

每个 feature 的 `readme.md` 应包含：
- **包含**：此 feature 负责的功能范围、核心交付物、关键接口/模块
- **不包含**：不在此 feature 范围内的功能、由其他 feature 负责的部分、明确排除的边界

## 适用边界

- 适合：长期 feature、复杂调试、多 Agent 交接、上下文恢复、架构决策记录。
- 不适合：普通一次性小任务、替代正式产品文档、替代测试、存放敏感信息、存放业务代码。
- 提交策略：目标项目中的 `feature-memory/` 是否提交到 Git 应由项目决定；公开仓库或包含敏感信息时，建议不提交或先脱敏。
- 隐私规则：写入任何架构、API、数据库或日志信息前，请先阅读 `PRIVACY.md`。

## 贡献

欢迎通过 Issue 或 Pull Request 改进模板、目录规范、debug 协议和不同 Agent 的安装说明。

Bug 或建议也可以发送到：`changjian1994@sina.com`。

## License

本项目使用 MIT License。详见 `LICENSE`。
