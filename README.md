# t3code-zh：T3 Code 简体中文版（非官方）

[T3 Code](https://github.com/pingdotgg/t3code) 桌面版的简体中文界面，可在「设置 → 常规 → 语言」里切换中文 / 英文。

> 非官方项目，与 T3 Tools Inc. 无关。
>
> 基于 T3 Code **官方正式版**翻译（当前 `v0.1.1`，汉化基线 `0.1.1-zh.2`，本分支版本 `0.1.1-zh.2.privacy.1`），只跟进正式 Release，不跟 nightly。应用名沿用上游的 `T3 Code (Beta)`。

<a id="privacy"></a>

## 隐私与外部连接

本分支移除了 T3 Code 产品使用统计和内置的 Axiom 中继追踪。桌面、CLI、随包网页和从同一份源码构建的移动客户端均使用补丁，无需设置环境变量。

- 服务端已移除 PostHog 批量发送、启动统计和模型用量上报。用量页面仍从本地记录读取 Token 数。
- 客户端连接时只附加会话所需的信息。服务端已移除读取 Codex、Claude 本机账号并计算遥测标识的代码。WebSocket 连接不再附带账号哈希、跟踪设备 ID 或设备型号。
- 使用模型、登录或启用 T3 Connect 时，客户端会访问所选服务。手动配置 OpenTelemetry 后，诊断数据会发往你指定的接收端。

这些改动适用于本分支构建的程序。连接到官方托管网页、商店版手机应用或独立安装的远程服务端时，对端遵循其自身的遥测策略。

## 安装

macOS 安装包可按下方「自己构建」生成。Windows x64 可从[本分支构建工作流](https://github.com/YangYuS8/t3code-zh/actions/workflows/build-windows.yml)的成功运行下载产物。请核对文件名包含 `0.1.1-zh.2.privacy.1`。安装前先退出正在运行的 T3 Code。

### macOS

1. 打开 `.dmg`，把 `T3 Code (Beta)` 拖进「应用程序」，替换旧版。
2. 第一次打开会提示无法验证（安装包没有经过苹果公证）。点「完成」，到「系统设置 → 隐私与安全性」，在页面底部点「仍要打开」。

如果提示「已损坏，无法打开」，在终端运行：

```sh
xattr -dr com.apple.quarantine "/Applications/T3 Code (Beta).app"
```

### Windows

运行 `.exe` 安装。安装包没有发布者签名，SmartScreen 拦截时点「更多信息 → 仍要运行」。

### 注意

- 会替换官方的 T3 Code (Beta)，并且不会自动更新，升级时请使用本分支的构建产物。
- 不要和官方版或旧的汉化版同时打开，两者共用设置目录，会互相覆盖。
- 从旧版本升级前，建议先备份 `~/.t3/userdata`。

## 已知限制

少量文字保持英文，主要是账号弹层（由 Clerk 生成）、部分服务端下发的说明和系统权限弹窗。完整清单见 [docs/KNOWN-LIMITATIONS.md](docs/KNOWN-LIMITATIONS.md)。

## 自己构建

macOS 安装包需要 macOS（Apple Silicon）、Node 24、Xcode Command Line Tools、Rust、[Vite+](https://vite.plus)。

```sh
git clone --filter=blob:none https://github.com/pingdotgg/t3code.git ~/Projects/t3code
git clone https://github.com/YangYuS8/t3code-zh.git ~/Projects/t3code-zh
cd ~/Projects/t3code-zh && npm ci

TAG=v0.1.1
git -C ~/Projects/t3code fetch origin tag "$TAG" --no-tags
git -C ~/Projects/t3code worktree add --detach ~/Projects/t3code-zh/upstream   "$TAG"
git -C ~/Projects/t3code worktree add --detach ~/Projects/t3code-zh/.build/src "$TAG"

scripts/build-zh.sh 0.1.1-zh.2.privacy.1
```

产物在 `release/<版本号>/`。Windows 安装包由 GitHub Actions 工作流 [`build-windows.yml`](.github/workflows/build-windows.yml) 构建（Actions 页面手动运行，产物在 artifacts 里）。

详细步骤和常见问题见 [docs/BUILD.md](docs/BUILD.md)，跟进上游新版本见 [docs/UPGRADE.md](docs/UPGRADE.md)，原理和约定见 [CONVENTIONS.md](CONVENTIONS.md)。

## 致谢与许可证

本分支基于 [markshan00/t3code-zh](https://github.com/markshan00/t3code-zh) 的简体中文词库、插件和构建流程。

初版词库来自 [ZhiweiXiao98/t3code](https://github.com/ZhiweiXiao98/t3code)（MIT），感谢原作者。

开发过程中使用了 [Claude Code](https://claude.com/claude-code) 和 [Codex](https://openai.com/codex)。

本仓库使用 [MIT](LICENSE) 许可证，上游许可证见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
