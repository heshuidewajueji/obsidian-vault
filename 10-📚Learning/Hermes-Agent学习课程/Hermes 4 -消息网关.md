# Hermes 4: 消息网关

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已完成
> 📌 基础课程 #4

---

## 一、消息网关概述

### 1.1 什么是消息网关

消息网关让 Hermes Agent 连接到即时通讯平台，实现 **跨平台消息收发**。

```
┌─────────────────────────────────────────┐
│                                         │
│   Hermes Agent                          │
│       │                                 │
│       ├──→ Telegram                    │
│       ├──→ Discord                     │
│       ├──→ Slack                       │
│       ├──→ WhatsApp                    │
│       └──→ 更多...                      │
│                                         │
└─────────────────────────────────────────┘
```

### 1.2 支持的平台

| 平台 | 支持情况 | 用途 |
|------|----------|------|
| **Telegram** | ✅ 完全支持 | 即时聊天 |
| **Discord** | ✅ 完全支持 | 社区/团队 |
| **Slack** | ✅ 完全支持 | 工作协作 |
| **WhatsApp** | ✅ 完全支持 | 日常沟通 |
| **QQ** | ⚠️ 社区支持 | 中文社区 |
| **微信** | ❌ 不支持 | - |

---

## 二、Telegram 接入

### 2.1 创建 Telegram Bot

1. 在 Telegram 搜索 **@BotFather**
2. 发送 `/newbot`
3. 输入 Bot 名称
4. 获取 Bot Token

```
获取 Token 格式：
123456789:ABCdefGHIjklMNOpqrsTUVwxyz
```

### 2.2 配置 Hermes

```bash
hermes gateway setup telegram
```

### 2.3 输入配置

```
┌─────────────────────────────────────────┐
│  Telegram 配置                           │
│  ─────────────────────────────────────  │
│                                         │
│  Bot Token: 123456789:ABCdefGHI...      │
│                                         │
│  允许的用户 ID（可选，留空允许所有人）:   │
│  > _                                    │
│                                         │
└─────────────────────────────────────────┘
```

### 2.4 启动

```bash
# 启动 Hermes 并开启 Telegram
hermes start --gateway telegram

# 或在 Web 面板中启用
# 设置 → 网关 → Telegram → 启用
```

### 2.5 使用 Telegram

```
1. 在 Telegram 找到你的 Bot
2. 发送 /start
3. 开始对话！
```

---

## 三、Discord 接入

### 3.1 创建 Discord 应用

1. 访问 https://discord.com/developers/applications
2. 点击 **New Application**
3. 输入名称
4. 在 **Bot** 设置中获取 Token

### 3.2 配置 Hermes

```bash
hermes gateway setup discord
```

### 3.3 输入配置

```
┌─────────────────────────────────────────┐
│  Discord 配置                            │
│  ─────────────────────────────────────  │
│                                         │
│  Bot Token: xxxxxxxxxxxxxxxxxxxxxx      │
│  Server ID: 123456789012345678          │
│  Channel ID: 987654321098765432         │
│                                         │
└─────────────────────────────────────────┘
```

### 3.4 添加 Bot 到服务器

1. 在 Discord Developer Portal
2. **OAuth2 → URL Generator**
3. 勾选 `bot` 和 `applications.commands`
4. 复制生成的 URL
5. 在浏览器打开并授权

---

## 四、Slack 接入

### 4.1 创建 Slack App

1. 访问 https://api.slack.com/apps
2. 点击 **Create New App**
3. 选择 **From scratch**
4. 输入 App 名称和工作区

### 4.2 获取配置信息

```
┌─────────────────────────────────────────┐
│  Slack App 设置                         │
│  ─────────────────────────────────────  │
│                                         │
│  1. Bot Token (xoxb-xxx)                │
│  2. Signing Secret                      │
│  3. Enable Socket Mode                  │
│  4. Event Subscriptions: bot mentions   │
│                                         │
└─────────────────────────────────────────┘
```

### 4.3 配置 Hermes

```bash
hermes gateway setup slack
```

### 4.4 输入配置

```
┌─────────────────────────────────────────┐
│  Slack 配置                              │
│  ─────────────────────────────────────  │
│                                         │
│  Bot Token: xoxb-xxxxxxxx-xxxx           │
│  Signing Secret: xxxxxxxx               │
│  Team ID: T12345678                     │
│                                         │
└─────────────────────────────────────────┘
```

---

## 五、多网关同时使用

### 5.1 启用多个网关

```bash
# 启动时指定多个网关
hermes start --gateway telegram,discord

# 或在 Web 面板中启用
# 设置 → 网关 → 勾选多个平台
```

### 5.2 统一收件箱

所有平台的消息都会在 Hermes 中统一显示：

```
┌─────────────────────────────────────────┐
│  📥 统一收件箱                           │
│  ─────────────────────────────────────  │
│                                         │
│  📱 Telegram - 小明 (2分钟前)            │
│  💬 Discord - @user (5分钟前)           │
│  💼 Slack - #general (10分钟前)          │
│                                         │
└─────────────────────────────────────────┘
```

---

## 六、网关配置管理

### 6.1 查看已配置的网关

```bash
hermes gateway list
```

### 6.2 启用/禁用网关

```bash
# 禁用 Telegram
hermes gateway disable telegram

# 启用 Telegram
hermes gateway enable telegram
```

### 6.3 删除网关配置

```bash
hermes gateway remove telegram
```

---

## 七、安全设置

### 7.1 允许特定用户

```bash
# 只允许特定 Telegram 用户
hermes gateway config telegram allowed_users 123456789,987654321
```

### 7.2 设置访问密码

```bash
# 设置访问密码
hermes config set gateway_password your_password
```

### 7.3 Webhook 安全

| 设置 | 说明 |
|------|------|
| `verify_token` | 验证 Webhook 来源 |
| `allowed_users` | 白名单用户 |
| `gateway_password` | 全局访问密码 |

---

## 💡 今日要点

> 1. **消息网关**：让 Hermes 连接 IM 平台
> 2. **Telegram**：最简单，推荐首先配置
> 3. **多网关**：可以同时启用多个平台
> 4. **安全**：建议设置 allowed_users 限制访问

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学习，无疑问

---

## 🔗 关联资源

- [[Hermes 3 -核心功能]]
- [[Hermes 5 -进阶用法]]
- [Hermes Gateway 文档](https://github.com/NousResearch/Hermes-Agent#gateways)

---

*Tags: #hermes #基础课程 #lesson-4 #消息网关 #Telegram #Discord*
