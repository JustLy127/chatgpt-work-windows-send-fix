# Windows 桌面端 ChatGPT Work 新对话发送按钮灰色

如果 Windows 桌面端 Work 的新对话里已经输入文字，发送按钮仍是灰色，但旧对话可以继续发送，可以检查 ChatGPT 是否在使用这台电脑已有的 Windows 系统代理。

## 适用情况

这台电脑已有可用的 Windows 系统代理。我们的实例使用 v2rayN，直连 ChatGPT 超时，通过同一个已启用的系统代理可以访问 ChatGPT。当前电脑的系统代理由本人或管理员管理时，请先取得同意再更改代理相关配置。

## 操作步骤

1. 在文件资源管理器的地址栏输入 `%USERPROFILE%\.codex`，打开 `config.toml`。编辑前先备份文件。
2. 如果文件已经有 `[features]`，就在这一节下面添加 `respect_system_proxy = true`。如果同名设置已存在，把它的值改为 `true`。只有在 `[features]` 缺失时才新增这一节。

   ```toml
   [features]
   respect_system_proxy = true
   ```

3. 保存后，等待正在运行的本地任务结束，再完整退出并重新打开 ChatGPT 桌面端。Windows 的退出快捷键是 `Ctrl+Q`。
4. 切到 Work，按原来出问题时的运行位置新建对话，输入 `你好` 并发送。能收到回复就完成了首条消息验证。

如需恢复，把 `respect_system_proxy` 改回原值。如果之前没有这一行，就移除它，然后重启 ChatGPT。若本机没有可用的系统代理，或者云端和本地模式遇到不同的故障，先确认具体症状再决定是否使用这项设置。

## 验证记录

Windows 桌面端 `26.1117.41037`，Microsoft Store 包 `26.1117.4957.0`。本地 Work 组件此前反复连接超时。启用 `respect_system_proxy` 后，单独重启本地 Work 组件，它在第一次重连时接入服务。随后手动新建 Work 对话，验证可以发送首条消息并收到回复。完整退出并重开桌面端是官方文档提供的配置生效方式，适合在其他电脑上照做。

GitHub 上也有改动后仍出现同类症状的报告。 [初始故障报告](https://github.com/openai/codex/issues/54564) · [代理相关案例](https://github.com/openai/codex/issues/49608) · [后续复现](https://github.com/openai/codex/issues/62393)

## English summary

On one Windows installation, new ChatGPT Work chats had a disabled Send button while existing chats still worked. Direct HTTPS to ChatGPT timed out, while the existing Windows system proxy could reach ChatGPT. Setting `respect_system_proxy = true` under `[features]` in the user-level `%USERPROFILE%\.codex\config.toml` and restarting the local Work executor restored connectivity. A fresh Work chat then sent its first message and received a reply. On another computer with a working system proxy, fully quit and reopen the desktop app after editing the config, then test the first message in a fresh Work chat. Each machine needs its own verification.

[OpenAI desktop system proxy configuration](https://learn.chatgpt.com/docs/enterprise/managed-configuration#use-the-system-proxy-for-native-client-requests) · [OpenAI desktop shortcuts](https://learn.chatgpt.com/docs/app/commands)
