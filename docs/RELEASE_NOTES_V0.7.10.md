# Release Notes - v0.7.10

## ✨ 新增功能 / New Features

- **群聊白名单拦截 / Group Chat Allowlist Enforcement**
  当 `groupPolicy` 设为 `'allowlist'` 时，连接器现在会在群聊消息处理入口检查发送者的 staffId 是否在 `allowFrom` 白名单中。不在白名单中的用户即使 @ 机器人，消息也会被静默拦截，行为与单聊 `dmPolicy: 'allowlist'` 完全对称。
  When `groupPolicy` is set to `'allowlist'`, the connector now checks the sender's staffId against the `allowFrom` whitelist at the group message processing entry point. Users not on the whitelist are silently dropped even if they @ the bot, mirroring the behavior of `dmPolicy: 'allowlist'` for DMs.

- **`resolveGroupPolicy` 安全接口 / `resolveGroupPolicy` Security Interface**
  在插件描述符的 `security` 配置节新增 `resolveGroupPolicy`，与现有的 `resolveDmPolicy` 并列。OpenClaw 平台可通过此接口统一管理群聊白名单策略，包括通过 `/allow dingtalk-connector:<userId>` 命令动态添加白名单用户。
  Added `resolveGroupPolicy` alongside the existing `resolveDmPolicy` in the plugin descriptor's `security` section. The OpenClaw platform can use this interface to manage the group chat allowlist consistently, including dynamic management via the `/allow dingtalk-connector:<userId>` command.

## ✨ 功能与体验改进 / Features & Improvements

- **WebSocket 心跳重连机制优化 / WebSocket Reconnection**
  WebSocket 会持续尝试重连，保留指数退避策略（1s → 2s → 4s → 8s → 16s → 30s），避免雪崩效应。重连成功后重置计数器，下次失败从 1 秒开始退避。
  Removed maximum reconnection attempt limit, implemented infinite reconnection mechanism. WebSocket will continuously attempt to reconnect without giving up. Exponential backoff strategy retained (1s → 2s → 4s → 8s → 16s → 30s) to avoid avalanche effect. Counter resets on successful reconnection, starting from 1 second on next failure.

- **配置简化 / Configuration Simplification**
  从 `SocketManagerConfig` 中移除 `maxReconnectAttempts` 配置项，简化配置复杂度。
  Removed `maxReconnectAttempts` configuration from `SocketManagerConfig`, simplifying configuration complexity.

- **日志输出优化 / Log Output Optimization**
  更新重连日志格式，移除最大次数显示（从 "尝试 X/5" 改为 "尝试 X"），更清晰地展示重连进度。
  Updated reconnection log format, removed maximum attempt display (from "attempt X/5" to "attempt X"), providing clearer reconnection progress.

## 📋 配置说明 / Configuration

### 启用群聊白名单

```json5
{
  channels: {
    "dingtalk-connector": {
      enabled: true,
      clientId: "dingxxxxxxxxx",
      clientSecret: "your_secret_here",
      groupPolicy: "allowlist", // 启用群聊白名单拦截
      allowFrom: [
        // 允许的用户 staffId 列表
        "user_staff_id_1",
        "user_staff_id_2",
      ],
    },
  },
}
```

### 同时启用单聊和群聊白名单

`dmPolicy` 和 `groupPolicy` 共享同一个 `allowFrom` 列表，可同时配置：

```json5
{
  channels: {
    "dingtalk-connector": {
      dmPolicy: "allowlist",
      groupPolicy: "allowlist",
      allowFrom: ["staff_id_1", "staff_id_2"],
    },
  },
}
```

## ⚠️ 升级注意事项 / Upgrade Notes

- **向下兼容 / Backward Compatible**：`groupPolicy` 默认值仍为 `'open'`，现有未配置白名单的用户不受影响。
- **仅影响 `allowlist` 模式**：只有显式设置 `groupPolicy: 'allowlist'` 且 `allowFrom` 非空时，群聊拦截才会生效。

## 📥 安装升级 / Installation & Upgrade

```bash
# 通过 npm 安装最新版本 / Install latest version via npm
openclaw plugins install @dingtalk-real-ai/dingtalk-connector

# 或升级现有版本 / Or upgrade existing version
openclaw plugins update dingtalk-connector

# 通过 Git 安装 / Install via Git
openclaw plugins install https://github.com/DingTalk-Real-AI/dingtalk-openclaw-connector.git
```

## 🔗 相关链接 / Related Links

- [完整变更日志 / Full Changelog](https://github.com/DingTalk-Real-AI/dingtalk-openclaw-connector/blob/main/CHANGELOG.md)
- [使用文档 / Documentation](https://github.com/DingTalk-Real-AI/dingtalk-openclaw-connector/blob/main/README.md)
- [问题反馈 / Issue Feedback](https://github.com/DingTalk-Real-AI/dingtalk-openclaw-connector/issues)

## 📥 安装升级 / Installation & Upgrade

```bash
# 通过 npm 安装最新版本 / Install latest version via npm
openclaw plugins install @dingtalk-real-ai/dingtalk-connector

# 或升级现有版本 / Or upgrade existing version
openclaw plugins update dingtalk-connector
<<<<<<< HEAD

# 通过 Git 安装 / Install via Git
openclaw plugins install https://github.com/DingTalk-Real-AI/dingtalk-openclaw-connector.git
=======
>>>>>>> origin/main
```

## 🔗 相关链接 / Related Links

- [完整变更日志 / Full Changelog](https://github.com/DingTalk-Real-AI/dingtalk-openclaw-connector/blob/main/CHANGELOG.md)
- [使用文档 / Documentation](https://github.com/DingTalk-Real-AI/dingtalk-openclaw-connector/blob/main/README.md)
<<<<<<< HEAD
=======
- [问题反馈 / Issue Feedback](https://github.com/DingTalk-Real-AI/dingtalk-openclaw-connector/issues)
>>>>>>> origin/main

---

**发布日期 / Release Date**：2026-03-16  
**版本号 / Version**：v0.7.10  
**兼容性 / Compatibility**：OpenClaw Gateway 0.4.0+
