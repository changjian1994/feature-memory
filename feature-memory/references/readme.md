# Feature Memory 参考文档

本目录存放 feature memory skill 的详细参考资料。`SKILL.md` 只保留触发边界、硬规则和任务路由；详细模板、debug 规范、feature 关系管理和隐私规则放在这里，按需读取。

## 文件说明

- `templates.md`：项目级、feature 级、Bug 级文档模板。
- `debug-workflow.md`：复杂问题 debug 目录、日志迭代、`debug.sh` 安全规则和交互协议。
- `feature-relationship.md`：feature 合并、拆分、父子关系、依赖关系、交叉关系和聚合导航规则。
- `privacy.md`：隐私、安全边界、脱敏规则和 Git 提交策略。

## 读取时机

- 创建或更新 feature 文档时，读取 `templates.md`。
- 创建、拆分、合并、聚合 feature 前，读取 `feature-relationship.md`。
- 处理复杂 Bug、日志分析或多轮 debug 时，读取 `debug-workflow.md`。
- 记录架构、接口、数据库、日志或任何可能敏感的信息前，读取 `privacy.md`。
- 只需要恢复上下文时，优先读取 `feature-memory/<feature-name>/handoff.md`、`progress.md`、`design.md`，不要主动加载全部参考文档。

## 维护规则

- 参考文档只描述通用规范，不记录具体业务事实。
- 具体业务事实必须写入项目根目录下的 `feature-memory/` 记忆目录。
- 敏感信息必须按 `privacy.md` 脱敏或避免记录。
- 如果模板变化，应同步检查 `SKILL.md` 中的目录结构和工作流描述。
