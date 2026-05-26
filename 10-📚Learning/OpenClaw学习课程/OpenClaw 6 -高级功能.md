# OpenClaw 6: 高级功能

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #6

---

## 一、ACP 协议

### 1.1 ACP 概述

ACP (Agent Communication Protocol) 是 OpenClaw 的 Agent 间通信协议：

- **消息传递**：Agent 之间传递消息
- **任务委托**：一个 Agent 委托任务给另一个
- **状态同步**：保持多 Agent 状态一致
- **事件通知**：订阅和发布事件

### 1.2 ACP 架构

```
┌─────────────────────────────────────────────────────────────┐
│                        ACP 协议栈                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   ┌─────────┐  ──ACP─→  ┌─────────┐  ──ACP─→  ┌─────────┐  │
│   │ Agent A │           │ Gateway │           │ Agent B │  │
│   └─────────┘  ←───  │  └─────────┘  ←───  │  └─────────┘  │
│                      │                                  │
│                      ▼                                  │
│              ┌─────────────────┐                        │
│              │  ACP Router     │                        │
│              │  (消息路由)      │                        │
│              └─────────────────┘                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1.3 ACP 消息格式

```json
// ACP 消息结构
{
  "type": "message",           // 消息类型
  "from": "agent:coding",      // 发送方
  "to": "agent:main",          // 接收方
  "content": {
    "text": "任务完成报告",
    "attachments": []
  },
  "metadata": {
    "request_id": "req_xxx",
    "timestamp": "2026-05-24T12:00:00Z"
  }
}
```

### 1.4 ACP 消息类型

| 类型 | 说明 | 示例 |
|------|------|------|
| `message` | 普通消息 | 对话内容 |
| `task` | 任务委托 | "帮我写这个函数" |
| `response` | 任务响应 | 任务结果返回 |
| `event` | 事件通知 | "有新消息" |
| `sync` | 状态同步 | 同步状态信息 |

---

## 二、任务委托

### 2.1 委托任务

```bash
# 委托任务给其他 Agent
openclaw acp delegate --to coding --task "代码审查"

# 异步委托
openclaw acp delegate --to coding --task "优化性能" --async

# 带参数委托
openclaw acp delegate --to coding --task "审查代码" --params '{"file": "main.ts"}'
```

### 2.2 查看任务状态

```bash
# 列出待处理任务
openclaw acp tasks pending

# 查看任务详情
openclaw acp tasks show <task-id>

# 取消任务
openclaw acp tasks cancel <task-id>
```

### 2.3 任务配置

```bash
# 设置任务超时
openclaw config set acp.task.timeout 300

# 设置重试次数
openclaw config set acp.task.retries 3
```

---

## 三、事件系统

### 3.1 事件类型

| 事件 | 说明 |
|------|------|
| `agent:online` | Agent 上线 |
| `agent:offline` | Agent 下线 |
| `channel:message` | 新消息 |
| `memory:updated` | 记忆更新 |
| `task:completed` | 任务完成 |
| `task:failed` | 任务失败 |

### 3.2 订阅事件

```bash
# 订阅事件
openclaw acp subscribe --event task:completed --callback my_handler

# 查看订阅
openclaw acp subscriptions

# 取消订阅
openclaw acp unsubscribe <subscription-id>
```

### 3.3 发布事件

```bash
# 发布事件
openclaw acp publish --event "deployment:completed" --data '{"version": "1.0"}'

# 发布给特定 Agent
openclaw acp publish --event "alert" --to agent:monitoring --data '{"level": "high"}'
```

---

## 四、高级路由

### 4.1 条件语法

```bash
# 支持的条件类型
- channel:telegram         # 通道类型
- keyword:编程             # 关键词匹配
- regex:^\/.*             # 正则表达式
- time:09:00-18:00        # 时间范围
- agent:available         # Agent 可用性
- load:<0.8               # 负载均衡
```

### 4.2 路由链

```bash
# 链式路由：按顺序尝试匹配
openclaw routing chain add \
  --condition "channel:telegram + keyword:紧急" \
  --agent main \
  --priority 100

openclaw routing chain add \
  --condition "channel:telegram + keyword:代码" \
  --agent coding \
  --priority 90

openclaw routing chain add \
  --condition "channel:telegram" \
  --agent assistant \
  --priority 50
```

### 4.3 负载均衡

```bash
# 启用负载均衡
openclaw config set routing.load_balancing true

# 设置权重
openclaw agents config coding --set weight 2
openclaw agents config assistant --set weight 1
```

---

## 五、自定义 Agent

### 5.1 创建专业 Agent

```bash
# 创建 Agent
openclaw agents create reviewer

# 配置系统提示词
cat > ~/.openclaw/agents/reviewer/agent/system-prompt.md << 'EOF'
# Code Reviewer Agent

## 角色
你是一个专业的代码审查专家。

## 职责
- 检查代码质量
- 发现潜在 Bug
- 提出优化建议
- 确保代码规范

## 审查标准
1. 功能正确性
2. 性能影响
3. 安全性
4. 可读性
5. 测试覆盖

## 输出格式
```markdown
## 审查报告
### 问题
- ...
### 建议
- ...
```
EOF
```

### 5.2 Agent 技能配置

```json
// ~/.openclaw/agents/reviewer/agent/skills.json
{
  "skills": [
    {
      "name": "code-review",
      "enabled": true,
      "triggers": ["审查", "review", "代码检查"]
    },
    {
      "name": "security-scan",
      "enabled": true,
      "triggers": ["安全", "security"]
    }
  ]
}
```

### 5.3 Agent 工具配置

```bash
# 为 Agent 配置工具
openclaw agents config reviewer --tools "shell,git,file_reader"

# 限制工具权限
openclaw agents config reviewer --tool shell --allow "npm test"
openclaw agents config reviewer --tool shell --deny "rm -rf"
```

---

## 六、系统监控

### 6.1 健康检查

```bash
# 健康检查
openclaw health

# 详细状态
openclaw health --verbose

# 检查特定组件
openclaw health --component gateway
openclaw health --component memory
openclaw health --component channels
```

### 6.2 性能指标

```bash
# 查看性能指标
openclaw metrics

# 实时指标
openclaw metrics --watch

# 特定指标
openclaw metrics --type memory
openclaw metrics --type agent
```

### 6.3 指标输出格式

```json
{
  "timestamp": "2026-05-24T12:00:00Z",
  "system": {
    "cpu": 0.15,
    "memory": 0.45,
    "uptime": 86400
  },
  "agents": {
    "main": { "requests": 150, "errors": 2 },
    "coding": { "requests": 80, "errors": 0 }
  },
  "memory": {
    "total": 1000,
    "used": 250
  }
}
```

---

## 七、API 扩展

### 7.1 自定义 API 端点

```bash
# 注册自定义端点
openclaw api register --path /status --handler my_handler

# 查看注册的端点
openclaw api list
```

### 7.2 Webhook 配置

```bash
# 添加 Webhook
openclaw webhook add --url https://example.com/hook --events "task:completed"

# 查看 Webhook
openclaw webhook list

# 删除 Webhook
openclaw webhook delete <webhook-id>
```

### 7.3 Webhook 配置文件

```json
// ~/.openclaw/webhooks.json
{
  "webhooks": [
    {
      "id": "wh_001",
      "url": "https://example.com/hook",
      "events": ["task:completed", "agent:online"],
      "secret": "xxx",
      "enabled": true
    }
  ]
}
```

---

## 八、安全配置

### 8.1 认证配置

```bash
# 启用 API 认证
openclaw config set api.auth.enabled true
openclaw config set api.auth.api_key "sk-xxx"

# 配置 JWT
openclaw config set api.auth.jwt.secret "xxx"
openclaw config set api.auth.jwt.expiry 3600
```

### 8.2 通道安全

```bash
# Telegram 允许列表
openclaw config set channels.telegram.allowed_chats '["CHAT_ID_1", "CHAT_ID_2"]'

# Discord 频道白名单
openclaw config set channels.discord.allowed_channels '["CHANNEL_ID_1"]'
```

### 8.3 数据加密

```bash
# 启用记忆加密
openclaw config set memory.encrypt true
openclaw config set memory.encrypt.key "xxx"
```

---

## 九、备份与恢复

### 9.1 完整备份

```bash
# 备份所有配置
openclaw backup create --output ~/backups/openclaw_$(date +%Y%m%d).tar.gz

# 备份指定组件
openclaw backup create --components agents,memory,channels
```

### 9.2 恢复配置

```bash
# 恢复备份
openclaw backup restore ~/backups/openclaw_20260524.tar.gz

# 恢复前预览
openclaw backup restore ~/backups/openclaw_20260524.tar.gz --dry-run
```

### 9.3 定时备份

```bash
# 设置定时备份
openclaw backup schedule --cron "0 3 * * *" --output ~/backups/

# 查看备份计划
openclaw backup schedule
```

---

## 十、实战示例

### 10.1 场景：构建自动化工作流

```bash
# 1. 创建工作流 Agent
openclaw agents create workflow

# 2. 配置任务触发
openclaw acp subscribe --event channel:message --callback workflow_trigger

# 3. 配置自动任务委托
openclaw routing add \
  --condition "keyword:部署" \
  --delegate coding \
  --delegate deploy

# 4. 设置完成通知
openclaw webhook add \
  --url "https://notify.example.com/deploy" \
  --events "task:completed"
```

### 10.2 场景：多 Agent 协作

```bash
# 1. 创建专业 Agent
openclaw agents create code-reviewer
openclaw agents create security-auditor

# 2. 配置代码审查流程
openclaw acp delegate \
  --to code-reviewer \
  --task "审查代码" \
  --params '{"file": "src/main.ts"}'

# 3. 审查通过后自动安全扫描
openclaw acp subscribe \
  --event task:completed \
  --condition "agent:code-reviewer" \
  --callback "delegate_to_security"

# 4. 安全扫描后汇总报告
openclaw acp delegate \
  --to main \
  --task "汇总审查报告"
```

### 10.3 场景：系统监控告警

```bash
# 1. 配置监控
openclaw metrics --watch

# 2. 设置告警规则
cat > ~/.openclaw/alerts.json << 'EOF'
{
  "alerts": [
    {
      "condition": "memory.usage > 0.9",
      "severity": "high",
      "action": "notify",
      "message": "记忆存储使用率超过 90%"
    },
    {
      "condition": "agent.main.errors > 10",
      "severity": "medium",
      "action": "webhook",
      "webhook": "https://notify.example.com/alert"
    }
  ]
}
EOF

# 3. 启用告警
openclaw config set alerts.enabled true
```

---

## 常用命令速查

| 命令 | 功能 |
|------|------|
| `openclaw acp delegate` | 任务委托 |
| `openclaw acp publish` | 发布事件 |
| `openclaw acp subscribe` | 订阅事件 |
| `openclaw routing chain` | 路由链 |
| `openclaw agents create` | 创建 Agent |
| `openclaw health` | 健康检查 |
| `openclaw metrics` | 性能指标 |
| `openclaw backup create` | 备份 |

---

## 💡 今日要点

> 1. **ACP 协议**：Agent 间通信、任务委托、事件系统
> 2. **高级路由**：条件链、负载均衡
> 3. **自定义 Agent**：专业角色、工具权限
> 4. **系统监控**：健康检查、性能指标、告警
> 5. **安全配置**：认证、加密、白名单

---

## 📝 个人笔记区
1、已学习，无问题

---

## 🔗 关联资源

- [[OpenClaw 5 -记忆系统]]
- [[OpenClaw 1 -认识OpenClaw]]
- [[Hermes 8 -自我进化系统]]
- [[00-OpenClaw课程目录]]

---

*Tags: #openclaw #高级 #acp #路由 #监控 #lesson-6*