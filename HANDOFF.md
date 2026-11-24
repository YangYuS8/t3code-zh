# 隐私补丁交接

汉化基线为 `markshan00/t3code-zh@92ca2ee`，应用基线为 `pingdotgg/t3code@fe27e8e`（v0.1.1）。更新基线时要复查以下路径。

- 旧版即使设置 `T3CODE_TELEMETRY_ENABLED=false`，服务端构建 AnalyticsService 时仍会解析本机账号。启动心跳、ChatGPT 登录完成、MCP 工具调用和模型轮次结束也各有独立采集点。0025 从生产调用点和发送端同时移除。
- 浏览器、桌面和移动端连接旧服务端时，也可能在 WebSocket URL 中附带归因哈希。验证主连接、Bearer 和 DPoP 三种入口。配对使用的短期票据和设备授权仍有独立用途。
- Web、移动端和本地服务端可各自创建 Axiom 导出器，服务端环境变量无法控制另一个进程。0025 同时移除导出器及其打包配置。用户明确设置的 OTLP 接收端有自己的配置路径。
- 上游汉化版的 Releases 提供原始安装包。分发时核对产物版本；本分支版本为 `0.1.1-zh.2.privacy.1`。

可用 `node --test scripts/lib/t3-connect-config.test.ts` 检查构建规则。测试需要按 README 准备 `upstream/`。macOS 和 Windows 共用的产物检查会扫描服务端、随包网页、桌面主进程及 preload 中已知的遥测标记。
