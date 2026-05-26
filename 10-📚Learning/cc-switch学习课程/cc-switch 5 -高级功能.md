# cc-switch 5: 高级功能

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #5

---

## 一、MCP 深度集成

### 1.1 MCP 相关 Skills

| Skill | 功能 |
|-------|------|
| `mcp-builder` | MCP Server 构建器 |
| `mcp-*` | MCP 相关工具 |

### 1.2 mcp-builder Skill

MCP Server 构建：

```bash
# 查看 Skill
cat ~/.cc-switch/skills/mcp-builder/SKILL.md
```

功能：
- 创建 MCP Server 模板
- 配置 Server
- 测试连接

### 1.3 MCP 架构

```
cc-switch
    │
    ├── Claude Code → ~/.claude/mcp.json
    │
    ├── Hermes → ~/.hermes/config.yaml
    │
    └── 其他工具 → 各自的 MCP 配置
         │
         ▼
    MCP Memory Server ←── 共享 SQLite
         │
         ▼
    SQLite Database
```

---

## 二、Provider 高级配置

### 2.1 Provider 类型

| Provider | 支持的模型 |
|----------|-----------|
| Claude | claude-sonnet-4-20250514, claude-3-5-sonnet, claude-3-opus |
| Claude Desktop | default |
| Gemini | gemini-2.0-flash, gemini-pro |
| OpenAI | gpt-4, gpt-3.5-turbo |

### 2.2 故障转移配置

```json
// settings.json
{
  "enableFailoverToggle": true,
  "failoverConfirmed": true
}
```

### 2.3 故障转移策略

| 场景 | 策略 |
|------|------|
| Provider 超时 | 等待 + 重试 |
| API 限流 | 指数退避 |
| Provider 不可用 | 切换到备选 |

---

## 三、本地代理

### 3.1 代理配置

```json
// settings.json
{
  "enableLocalProxy": true,
  "proxyConfirmed": true
}
```

### 3.2 代理功能

- **请求转发**：转发 API 请求
- **流量监控**：监控使用情况
- **缓存**：减少重复请求

### 3.3 代理端口

```bash
# 查看代理配置
# 通常在 localhost:PORT 运行
```

---

## 四、Skills 注册系统

### 4.1 skill-registry-management

技能注册管理：

```bash
# 查看 Skill
cat ~/.cc-switch/skills/skill-registry-management/SKILL.md
```

功能：
- 注册新 Skill
- 更新 Skill
- 删除 Skill

### 4.2 skill-manage

技能管理：

```bash
# 查看 Skill
cat ~/.cc-switch/skills/skill-manage/SKILL.md
```

功能：
- 启用/禁用 Skill
- 查看 Skill 状态
- 批量操作

### 4.3 book-to-skill-distiller

书籍转 Skill：

```bash
# 查看 Skill
cat ~/.cc-switch/skills/book-to-skill-distiller/SKILL.md
```

功能：
- 从书籍提取知识
- 生成 Skill 模板
- 自动化创建

---

## 五、系统健康监控

### 5.1 architecture-health-check

架构健康检查：

```bash
# 查看 Skill
cat ~/.cc-switch/skills/architecture-health-check/SKILL.md
```

功能：
- 检查配置文件
- 验证连接
- 报告状态

### 5.2 hermes-system-health-monitoring

Hermes 系统监控：

```bash
# 查看 Skill
cat ~/.cc-switch/skills/hermes-system-health-monitoring/SKILL.md
```

功能：
- 监控 Hermes 状态
- 检查健康指标
- 告警通知

---

## 六、高级配置

### 6.1 数据库配置

```bash
# cc-switch.db
sqlite3 ~/.cc-switch/cc-switch.db ".tables"
sqlite3 ~/.cc-switch/cc-switch.db "SELECT * FROM tools;"

# providers.db
sqlite3 ~/.cc-switch/providers.db ".tables"
```

### 6.2 备份策略

```bash
# 备份所有配置
~/.cc-switch/backups/
├── cc-switch_YYYYMMDD.db
├── providers_YYYYMMDD.db
└── skills_YYYYMMDD/
```

### 6.3 日志配置

```
~/.cc-switch/logs/
```

日志级别：
- debug：详细日志
- info：信息日志
- warn：警告日志
- error：错误日志

---

## 七、集成其他工具

### 7.1 与 Claude Code 集成

```json
// settings.json
{
  "enableClaudePluginIntegration": true,
  "skipClaudeOnboarding": true
}
```

### 7.2 与 Hermes 集成

通过 Skills 共享：

```
~/.cc-switch/skills/ ←── symlink/copy ──→ ~/.king-ai/skills/
```

Hermes 自动加载：
- 启动时自动发现 Skills
- 通过 Skill 名称调用

### 7.3 与 OpenClaw 集成

```bash
# OpenClaw 已配置为可见应用
{
  "visibleApps": {
    "openclaw": true
  }
}
```

---

## 八、性能优化

### 8.1 Skills 加载优化

```json
// 只加载需要的 Skills
{
  "skillSyncMethod": "symlink",
  "skillStorageLocation": "cc_switch"
}
```

### 8.2 数据库优化

```bash
# 定期备份数据库
cp ~/.cc-switch/cc-switch.db ~/.cc-switch/backups/

# 清理旧日志
rm ~/.cc-switch/logs/*.log.old
```

### 8.3 内存优化

```bash
# 减少同时运行的工具
# 只启用需要的 visibleApps
```

---

## 九、常用命令速查

| 命令 | 功能 |
|------|------|
| `cc-switch set-provider` | 设置 Provider |
| `cc-switch failover enable/disable` | 故障转移控制 |
| `cc-switch sync-skills` | 同步 Skills |
| `sqlite3 ~/.cc-switch/*.db` | 数据库操作 |
| `cc-switch health` | 健康检查 |

---

## 💡 今日要点

> 1. **MCP 集成**：通过 Skills 管理 MCP Server
> 2. **故障转移**：自动切换 Provider
> 3. **本地代理**：请求转发和监控
> 4. **Skills 注册**：注册、更新、删除 Skill
> 5. **健康监控**：architecture-health-check

---

## 📝 个人笔记区

---

## 🔗 关联资源

- [[cc-switch 4 -Skills 管理]]
- [[cc-switch 6 -实战应用]]
- [[00-cc-switch课程目录]]
- [[Hermes 9 -实战应用与最佳实践]]

---

*Tags: #cc-switch #高级 #MCP #故障转移 #lesson-5*