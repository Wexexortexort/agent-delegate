# Changelog

本文件记录 agent-delegate 相对上游 [oil-oil/oil-delegate](https://github.com/oil-oil/oil-delegate) 的改动。上游自身的更新会定期合并进来，不在这里重复列出。

## 2026-11-17

### 修复

- 任务文字尽量不放在命令行参数里：claude、workbuddy、codex、dsh、opencode 从标准输入接收任务，grok 用 `--prompt-file` 从文件读取。在 Windows 上，CLI 通常以 `.cmd` 包装形式安装，经过命令行传递的多行任务可能在换行处被截断。
- antigravity 和 kimi 只能通过参数接收任务，启动前会解析 `.cmd` 包装，直接运行它背后的真实程序；解析不了时按原样启动。
- 读取各 Agent 的版本和能力信息时统一按 UTF-8 解码，避免中文 Windows 默认编码下识别失败。
- 兼容 CodeBuddy 以 JSON 数组形式输出的结果。

### 新增

- `run` 支持 `--model` 和 `--effort`，只对当次任务生效，不保存为默认值。DeepSeek Harness 通过 `--patch` 覆盖默认模型，`provider` 默认 `deepseek-official`，可用 `--dsh-provider` 或环境变量 `OIL_DELEGATE_DSH_PROVIDER` 修改。
