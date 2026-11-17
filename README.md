# agent-delegate

**开箱即用的 Agent 跨端 CLI 委派工具，对 Windows 做了适配。** 一句话把任务交给 Claude Code、DeepSeek Harness、WorkBuddy、Codex 等本机 Agent，结果带回当前对话，还能接着追问。基于 [oil-oil/oil-delegate](https://github.com/oil-oil/oil-delegate)。

## 和原版有什么不同

功能和用法与原版一致，主要差别是 Windows 上的兼容性和一些小的增强：

- **Windows 兼容**：任务文字尽量不经过命令行传递，避免 Windows 环境下长任务、多行任务传不完整；中文系统下也能正确读取各 Agent 的输出。
- **单次任务可指定模型**：用 `--model`、`--effort` 给某一次任务换模型或思考强度，不影响各 Agent 自己的默认设置。

每次改了什么见 [CHANGELOG](CHANGELOG.md)。

## 能用来做什么

把不同 Agent 各自擅长的事串起来，你只需要用自然语言说：

```text
让 cc 先规划这个功能的实现方案，列出要改的文件。
```

```text
按刚才的方案，交给 dsh 去写代码，写完跑一遍测试。
```

```text
让 wb 只读审查 dsh 刚才的改动，指出问题和风险。
```

```text
以后代码审查都交给 codex，方案评审交给 cc。
```

- **规划、执行、审查分工**：贵的模型做判断，便宜的模型干活，另一个 Agent 独立挑错。
- **按用途记住**：代码审查、方案挑刺、疑难会诊、功能实现分别交给谁，说一次就记住。
- **连续对话**：拿到结果后可以接着追问或返工，不用重复背景。
- **安全默认**：审查类任务自动只读；不读取、不保存任何密钥。

## 支持的 Agent

全名或简称都可以，不区分大小写：

| Agent | 简称与别名 |
| --- | --- |
| Claude Code | `cc`、`claude`、`claude-code` |
| WorkBuddy | `wb`、`workbuddy`、`codebuddy`、`cbc` |
| Codex | `codex` |
| DeepSeek Harness | `dsh`、`deepseek`、`deepseek-harness` |
| Antigravity | `agy`、`antigravity` |
| OpenCode | `opencode` |
| Grok Build | `grok`、`grok-build` |
| Kimi Code | `kimi`、`kimi-code` |

Windows 上已用 WorkBuddy、Codex、DeepSeek Harness 跑过真实任务；其余 Agent 按官方文档适配并有自动化测试覆盖。遇到问题欢迎反馈。

## 安装

### 准备

- Windows 10/11（macOS、Linux 同样可用）
- Python 3.10 或更高版本。装好后在终端运行 `py -3 --version` 能看到版本号即可
- 至少一个已经安装并登录的上述 Agent

### 安装 Skill

最简单的方式是把下面这句话发给你的 Agent：

```text
请帮我安装这个 Skill：https://github.com/Wexexortexort/agent-delegate
```

也可以用命令安装：

```bash
npx skills add Wexexortexort/agent-delegate
```

安装后重新加载 Skill 即可。Skill 名称是 `agent-delegate`，可以和原版 `oil-delegate` 同时安装，互不影响。

两者共用同一个配置目录（`~/.oil-delegate`），在一边选好的 Agent 和偏好，另一边也能直接用。原作者的配套 Skill（如 `oil-plan`、`oil-code-review`）按名字查找 `oil-delegate`，不会自动调用本工具。

## 第一次使用

第一次委托时，它会自动检查本机装了哪些 Agent，然后只问你两件事：

1. 要启用哪些 Agent？
2. 默认用哪一个，还是每次再选？

你确认后才会保存配置。检查过程不调用模型、不安装软件、不替你登录。某个 Agent 没登录或不可用时，它会如实告诉你。

之后直接用自然语言说就行。没指定 Agent 时用你的默认选择；没有默认值时会先问你，不会擅自更换。

## 注意事项

- **费用**：任务内容会发给你选的 Agent 及其模型服务，可能产生费用。不要在任务里写密钥。
- **权限**：普通任务默认使用各 Agent 的最高权限，避免无人值守时卡在审批上。这意味着 Agent 可以改文件、执行命令。重要项目请先限定范围，事后检查改动并重跑测试。需要只读时直接说“只读”。
- **验收**：Agent 自己说“完成了”不算数，以实际改动和测试结果为准。

关于什么场景适合派发子 Agent、不同角色怎么分工，可以参考 Oil Book 里的[《怎样用好子 Agent》](https://book.oiloil.org/articles/subagent-workflow?utm_source=github&utm_medium=referral&utm_campaign=oil-delegate-readme)。

<details>
<summary>维护、测试与排查</summary>

运行回归测试（不会调用任何外部模型）：

```bash
py -3 -m unittest discover -s tests -p "test_*.py" -v
```

macOS / Linux 把 `py -3` 换成 `python3`。

各 Agent 的调用细节和失败处理见 [Runner 适配参考](references/runners.md)。遇到问题请到 [Issues](https://github.com/Wexexortexort/agent-delegate/issues) 反馈，附上 Agent 名称、版本和错误信息。

</details>

## 致谢与上游

本项目是 [oil-oil/oil-delegate](https://github.com/oil-oil/oil-delegate) 的独立分支，在原版基础上补充了 Windows 适配。核心设计、多 Agent 适配和交互流程都来自原作者 [oil-oil](https://github.com/oil-oil)，感谢原作者的开源工作。

本分支会持续同步上游更新。其中通用的修复也会以 PR 的形式提交给上游，例如 [oil-oil/oil-delegate#9](https://github.com/oil-oil/oil-delegate/pull/9)。

## License

[MIT License](LICENSE)。原作者版权声明保留在 LICENSE 中。图标与配图来源见[素材说明](assets/readme/NOTICE.md)。
