# Debug 流程参考

## 使用时机

复杂问题、线上异常、测试失败、日志排查、偶现 Bug 或多轮未收敛的问题，必须建立独立 Bug 目录并持续更新 debug 记忆。

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
- 涉及写入、删除、迁移、重启服务、修改数据库的命令必须先征得用户确认。
- 不得输出密钥、Token、Cookie 或个人敏感信息。
- 每轮排障后根据新假设更新。

`output/` 保存用户执行 `debug.sh` 后产生的日志，例如 `v1.log`、`v2.log`、`v3.log`。

## 交互协议

当用户提供新的 `output/vN.log` 时：

1. 分析最新日志，提取证据。
2. 更新 `conclusion.md` 的当前事实、排除项和下一步验证。
3. 追加更新 `timeline.md`，保留历史过程。
4. 更新 `debug.sh`，让下一轮验证更接近 root cause。
5. 向用户说明已确认、已排除、当前怀疑和下一步执行命令。

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

执行完成后，将生成的 output/vN+1.log 发给我，我将继续收敛。
```
