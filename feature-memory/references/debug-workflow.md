# Debug 流程参考

## 使用时机

复杂问题、线上异常、测试失败、日志排查、偶现 Bug 或多轮未收敛的问题，必须建立独立 Bug 目录并持续更新 debug 记忆。

## 触发判定

出现以下任一信号时，应立即进入 debug 记忆流程：

- 用户明确使用“debug”“排查”“看日志”“分析日志”“测试失败”“线上异常”“偶现”“复现不了”“继续收敛”等表达。
- 用户提供错误堆栈、测试失败输出、报警信息、诊断命令输出或 `output/vN.log`。
- 同一问题需要两轮及以上假设、验证、日志收集或结论更新。
- 当前原因未确认，需要保存“已确认事实”“已排除假设”“剩余假设”和“下一步验证”。
- 需要生成或更新 `debug.sh` 来收集下一轮证据。

不要把上述问题只记录在 feature 级 `progress.md` 中。进入 debug 记忆流程后，必须创建或维护独立 Bug 目录。

## 初始化步骤

首次进入某个 Bug 的 debug 记忆流程时：

1. 确认目标 feature 名称；不确定时先询问用户，或基于问题生成临时 kebab-case 名称。
2. 确保存在 `feature-memory/<feature-name>/debug/`。
3. 创建 Bug 目录：`BUG-YYYYMMDD-XXX-description`。
4. 初始化 `readme.md`、`timeline.md`、`conclusion.md`、`debug.sh` 和 `output/`。
5. 在 `readme.md` 中记录问题定义、影响范围、复现步骤、预期结果和实际结果。
6. 在 `timeline.md` 中追加第 1 轮假设、验证方式、当前结果、证据和下一步。
7. 在 `conclusion.md` 中写入当前发现、已确认事实、已排除假设、剩余假设、下一步验证和待确认问题。
8. 在 `debug.sh` 中写入下一轮一键只读诊断脚本，并让脚本自动输出到 `output/v1.log`。
9. 更新对应 feature 的 `handoff.md` 或 `progress.md`，添加相关 Bug 路径。

如果 Bug 目录已存在，不要新建重复目录，应继续追加 `timeline.md` 并更新 `conclusion.md`、`debug.sh`。

## 目录结构

Bug 目录放在对应 feature 的记忆目录下：

```text
feature-memory/<feature-name>/debug/BUG-YYYYMMDD-XXX-description/
├── readme.md
├── timeline.md
├── conclusion.md
├── debug.sh
└── output/
```

目录名中的描述部分使用 kebab-case，例如：

```text
BUG-20250101-001-login-timeout
```

## 文档说明

`readme.md` 记录问题定义：

```text
问题
影响范围
复现步骤
预期结果
实际结果
```

`timeline.md` 记录每轮 debug 历史，只追加、不覆盖：

```text
第 N 轮
假设
验证方式
结果：已确认 | 已排除 | 暂无结论
证据
下一步
```

`conclusion.md` 只保留当前最新结论：

```text
当前发现
已确认事实
已排除假设
剩余假设
下一步验证
待确认问题
```

`debug.sh` 用于生成下一轮分析所需数据：

- 默认只生成只读诊断命令。
- 必须是一键执行脚本，用户运行 `./debug.sh` 后应自动生成下一个版本化日志文件。
- 日志文件命名为 `output/vN.log`，从 `output/v1.log` 开始递增。
- 每轮更新 `debug.sh` 时，必须根据已有最大版本号写入下一个日志文件，例如已有 `v2.log` 时，下一轮脚本写入 `output/v3.log`。
- 禁止覆盖旧日志；旧日志必须保留用于回溯。
- 涉及写入、删除、迁移、重启服务、修改数据库的命令必须先征得用户确认。
- 不得输出密钥、Token、Cookie 或个人敏感信息。
- 每轮排障后根据新假设更新。

`output/` 保存用户执行 `debug.sh` 后产生的日志，例如 `v1.log`、`v2.log`、`v3.log`。

## 一键脚本循环

多轮排障采用固定闭环：

```text
Agent 生成/更新 debug.sh
用户执行 ./debug.sh
debug.sh 生成 output/vN.log
用户把 output/vN.log 内容原样粘贴回来，或告知日志文件路径
Agent 分析 output/vN.log
Agent 更新 timeline.md、conclusion.md、debug.sh
下一轮 debug.sh 生成 output/vN+1.log
```

Agent 每轮回复必须明确告诉用户：

- 本轮将生成哪个日志文件。
- 执行命令是什么。
- 执行后需要把哪个文件内容发回来。

推荐命令：

```sh
chmod +x debug.sh
./debug.sh
```

## 交互协议

当用户提供新的 `output/vN.log` 时：

1. 分析最新日志，提取证据。
2. 更新 `conclusion.md` 的当前事实、排除项和下一步验证。
3. 追加更新 `timeline.md`，保留历史过程。
4. 更新 `debug.sh`，让下一轮验证更接近 root cause，并写入下一个 `output/vN+1.log`。
5. 向用户说明已确认、已排除、当前怀疑、下一步执行命令和下一轮日志文件名。

当用户只描述一个新 Bug、但没有提供日志时：

1. 建立 Bug 目录和初始文档。
2. 把当前问题拆成初始假设。
3. 生成第一版只读 `debug.sh`，默认写入 `output/v1.log`。
4. 告知用户执行 `./debug.sh`，并把 `output/v1.log` 内容原样粘贴回来。

## 回复模板

```text
我已经分析 output/vN.log。

已确认：
- ...

已排除：
- ...

当前怀疑：
- ...

我已更新：
- timeline.md
- conclusion.md
- debug.sh

下一步请执行：
./debug.sh

本轮会生成：
output/vN+1.log

执行完成后，请将 output/vN+1.log 的内容原样发给我，我将继续收敛。
```
