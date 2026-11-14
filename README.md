# dsh-preset-ptc-creator

融合预设：**PTC 程序化工具调用 + 创造模式（cordis）全套能力**，用于开发 DSH 插件。

## 它是什么

一条 `@deepseek-ai/dsh-agent-preset` 声明（id `ptc-creator`），子插件清单 =
出厂 `cordis` 预设的完整清单 + `@deepseek-ai/dsh-tool-executor-ptc` 一行，外加一段
persona suffix 说明 PTC 下的创作流程。

因此会话同时拥有：

| 能力 | 来源 |
|---|---|
| 只有 `run_code` 可直呼，工具结果不进上下文 | `dsh-tool-executor-ptc` |
| 运行时自检 `cordis_inspect_list/query` | `dsh-tool-cordis` |
| 插件生成与安装 `plugin_manager` | `dsh-plugin-manager/tools` |
| 创作技能（plugin dev / composition / packages） | `dsh-skill-filesystem` + `dsh-agent-preset` 的 `skills/` |
| workflow 多子代理编排、计划模式、子代理 | `dsh-tool-workflow`、`dsh-plan-mode`、`dsh-tool-subagent` |

## 安装

```yaml
# plugin_manager 工具
{ action: install_bundle, target: "<本目录绝对路径>" }
```

安装后**新建会话**才能在模式选择器中选到 `PTC 创造模式`（已开始的会话保持原预设）。

## 关键行为约定（写进 persona suffix）

- 技能文本只有 `return await tools.skill({ name })` 才会进入上下文；
- 安装插件要给 `plugin_manager` 传 `timeoutMs: 600000` 或转后台作业；
- 计划模式仍用 `exit_plan_mode` 提交计划。

## 不冲突的边界

- 只 `insert` 新行，不覆盖 `standard` / `ptc` / `cordis` 任何已有预设；
- 执行器按 scope 就近生效，一个 scope 只允许一个执行器；
- cordis inspect 的 Host provider 由 `dsh-web-app` 在宿主层注册一次，本预设只挂工具行。

## 安装

```powershell
# 在 DSH Desktop 的插件/预设管理中安装本地预设包，或：
# 将本目录加入 profile 的 dependencies 并在 dsh.profile.bundles 中登记
```

## 内容

| 文件 | 作用 |
| --- | --- |
| `package.json` | 预设声明（`dsh.bundle.patch`） |
| `cordis.patch.yml` | 组合出厂的 cordis 预设与 PTC 执行器 |
| `README.md` | 本说明 |

## 说明

这是一个 **Agent 预设**（不是可执行插件）：它声明一个预设 id `ptc-creator`，把
「PTC 程序化工具调用（只有 `run_code` 可直呼）」与「Creator 创作流程（运行时自检、插件生成、
技能、workflow）」组合在一起，用于开发 DSH 插件。

## 致谢

- 产品设计 / 测试 / 验收：**BR-PC**
- 实现：**DeepSeek-V4.1-Flash-1017**（AI agent）

## License

MIT
