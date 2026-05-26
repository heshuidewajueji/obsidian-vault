# OpenClaw 4: 通道集成

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #4

---

## 一、通道概述

### 1.1 支持的平台

| 平台 | 状态 | 说明 |
|------|------|------|
| Telegram | ✅ | Bot API 集成 |
| Discord | ✅ | Bot Gateway |
| Slack | ✅ | Slack API |
| 飞书 | ✅ | 企微/ Lark |
| Matrix | ✅ | 去中心化协议 |
| WhatsApp | 🔜 | 计划中 |
| Signal | 🔜 | 计划中 |

### 1.2 通道架构

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   OpenClaw Gateway                                          │
│   ┌─────────────────────────────────────────────────────┐  │
│   │                 Channel Manager                      │  │
│   ├──────────┬──────────┬──────────┬──────────┬───────┤  │
│   │ Telegram │  Discord │  Slack   │  Feishu  │  ...  │  │
│   └────┬─────┴────┬─────┴────┬─────┴────┬─────┴───┬───┘  │
│        │          │          │          │         │        │
│        ▼          ▼          ▼          ▼         ▼        │
│   ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐  │
│   │ Telegram│ │ Discord│ │  Slack │ │ 飞书   │ │ Matrix │  │
│   │  Bot   │ │  Bot   │ │  App   │ │  Bot   │ │ Bridge │  │
│   └────────┘ └────────┘ └────────┘ └────────┘ └────────┘  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 二、Telegram 配置

### 2.1 获取 Bot Token

1. 在 Telegram 中搜索 **@BotFather**
2. 发送 `/newbot`
3. 输入 Bot 名称
4. 获取 Bot Token: `123456789:ABCdefGHIjklMNOpqrSTUvwxyz`

### 2.2 配置 Telegram

```bash
# 交互式配置
openclaw channels add telegram

# 或手动配置
openclaw config set channels.telegram.token "YOUR_BOT_TOKEN"
openclaw config set channels.telegram.chat_id "default"
```

### 2.3 Telegram 配置文件

```json
// ~/.openclaw/channels/telegram.json
{
  "enabled": true,
  "token": "123456789:ABCdefGHIjklMNOpqrSTUvwxyz",
  "allowed_chats": [],
  "admin_chats": [],
  "proxy": null
}
```

### 2.4 Telegram 命令

| 命令 | 功能 |
|------|------|
| `/start` | 开始对话 |
| `/agents` | 切换 Agent |
| `/memory` | 搜索记忆 |
| `/status` | 查看状态 |
| `/help` | 帮助信息 |

---

## 三、Discord 配置

### 3.1 创建 Discord 应用

1. 访问 [Discord Developer Portal](https://discord.com/developers/applications)
2. 创建 New Application
3. 在 Bot 页面获取 Token
4. 启用 Message Content Intent

### 3.2 添加 Bot 到服务器

```bash
# 获取邀请链接
openclaw channels discord invite

# 或手动构建 URL
https://discord.com/api/oauth2/authorize?client_id=CLIENT_ID&permissions=CONTAINER_PERMISSIONS&scope=bot
```

### 3.3 配置 Discord

```bash
# 交互式配置
openclaw channels add discord

# 手动配置
openclaw config set channels.discord.token "YOUR_BOT_TOKEN"
openclaw config set channels.discord.guild_id "SERVER_ID"
```

### 3.4 Discord 配置文件

```json
// ~/.openclaw/channels/discord.json
{
  "enabled": true,
  "token": "YOUR_BOT_TOKEN",
  "guild_id": "123456789",
  "channels": {
    "general": "CHANNEL_ID_1",
    "coding": "CHANNEL_ID_2"
  },
  "prefix": "!",
  "admins": ["USER_ID_1"]
}
```

### 3.5 Discord 斜杠命令

| 命令 | 功能 |
|------|------|
| `/chat <message>` | 与 Agent 对话 |
| `/switch <agent>` | 切换 Agent |
| `/memory <query>` | 搜索记忆 |
| `/agents` | 列出 Agent |

---

## 四、飞书配置

### 4.1 创建飞书应用

1. 访问 [飞书开放平台](https://open.feishu.cn/)
2. 创建企业自建应用
3. 获取 App ID 和 App Secret
4. 配置消息事件订阅

### 4.2 配置 Webhook

```bash
# 配置飞书
openclaw channels add feishu

# 设置凭证
openclaw config set channels.feishu.app_id "cli_xxx"
openclaw config set channels.feishu.app_secret "xxx"
openclaw config set channels.feishu.verification_token "xxx"
openclaw config set channels.feishu.encrypt_key "xxx"
```

### 4.3 飞书配置文件

```json
// ~/.openclaw/channels/feishu.json
{
  "enabled": true,
  "app_id": "cli_xxx",
  "app_secret": "xxx",
  "verification_token": "xxx",
  "encrypt_key": "xxx",
  "proxy": null
}
```

---

## 五、Slack 配置

### 5.1 创建 Slack 应用

1. 访问 [Slack API](https://api.slack.com/)
2. 创建 New App
3. 启用 Bot 功能
4. 获取 Bot User OAuth Token

### 5.2 配置 Slack

```bash
# 配置 Slack
openclaw channels add slack

openclaw config set channels.slack.bot_token "xoxb-xxx"
openclaw config set channels.slack.app_token "xapp-xxx"
openclaw config set channels.slack.signing_secret "xxx"
```

### 5.3 Slack 配置文件

```json
// ~/.openclaw/channels/slack.json
{
  "enabled": true,
  "bot_token": "xoxb-xxx",
  "app_token": "xapp-xxx",
  "signing_secret": "xxx",
  "socket_mode": true
}
```

---

## 六、通道管理

### 6.1 查看通道状态

```bash
# 查看所有通道
openclaw channels status

# 探测所有通道
openclaw channels status --probe

# 单独探测
openclaw channels probe telegram
openclaw channels probe discord
```

### 6.2 启用/禁用通道

```bash
# 禁用通道
openclaw channels disable telegram

# 启用通道
openclaw channels enable telegram

# 重启通道
openclaw channels restart telegram
```

### 6.3 通道日志

```bash
# 查看通道日志
openclaw channels logs telegram

# 实时日志
openclaw channels logs telegram --follow
```

---

## 七、代理配置

### 7.1 全局代理

如果网络受限，配置代理：

```bash
# 设置代理
openclaw config set proxy.http "http://proxy:8080"
openclaw config set proxy.https "http://proxy:8080"

# 特定通道代理
openclaw config set channels.telegram.proxy "socks5://proxy:1080"
```

### 7.2 代理配置文件

```json
// ~/.openclaw/proxy.json
{
  "http": "http://proxy:8080",
  "https": "http://proxy:8080",
  "no_proxy": "localhost,127.0.0.1"
}
```

---

## 八、实战示例

### 8.1 场景：同时配置 Telegram + Discord

```bash
# 1. 配置 Telegram
openclaw channels add telegram
openclaw config set channels.telegram.token "TG_BOT_TOKEN"

# 2. 配置 Discord
openclaw channels add discord
openclaw config set channels.discord.token "DISCORD_BOT_TOKEN"

# 3. 验证两个通道
openclaw channels status --probe

# 4. 配置跨通道路由
openclaw routing add --condition "channel:telegram" --agent main
openclaw routing add --condition "channel:discord" --agent main
```

### 8.2 场景：设置管理员

```bash
# Telegram 管理员
openclaw config set channels.telegram.admins '["USER_ID_1", "USER_ID_2"]'

# Discord 管理员
openclaw config set channels.discord.admins '["USER_ID_1"]'
```

### 8.3 场景：调试通道连接

```bash
# 1. 检查配置
openclaw config show | grep -A5 channels

# 2. 测试连接
openclaw channels probe telegram --verbose

# 3. 查看详细日志
openclaw --log-level debug channels start telegram

# 4. 检查网络
curl -x http://proxy:8080 https://api.telegram.org
```

---

## 九、常用命令速查

| 命令 | 功能 |
|------|------|
| `openclaw channels add <name>` | 添加通道 |
| `openclaw channels status` | 通道状态 |
| `openclaw channels probe` | 探测连接 |
| `openclaw channels disable` | 禁用通道 |
| `openclaw channels enable` | 启用通道 |
| `openclaw channels logs` | 查看日志 |

---

## 💡 今日要点

> 1. **Telegram**：Bot API，最简单的配置
> 2. **Discord**：Gateway，需要 Message Content Intent
> 3. **飞书**：企业自建应用，适合团队使用
> 4. **多通道**：同时配置多个平台，统一管理
> 5. **代理**：网络受限时配置代理

---

## 📝 个人笔记区
1、已学习，无疑问

---

## 🔗 关联资源

- [[OpenClaw 3 -核心功能]]
- [[OpenClaw 5 -记忆系统]]
- [[Hermes 4 -多平台网关]]
- [[00-OpenClaw课程目录]]

---

*Tags: #openclaw #通道 #telegram #discord #feishu #lesson-4*