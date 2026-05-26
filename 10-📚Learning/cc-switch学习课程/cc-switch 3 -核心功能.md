# cc-switch 3: 核心功能

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #3

---

## 一、工具切换

### 1.1 列出可用工具

```bash
# 查看所有可用工具
cc-switch list-apps

# 或直接查看配置
cat ~/.cc-switch/settings.json | jq '.visibleApps'
```

### 1.2 切换工具

```bash
# 切换到指定工具
cc-switch switch claude
cc-switch switch hermes
cc-switch switch openclaw

# 启动工具
cc-switch run claude
cc-switch run hermes
```

### 1.3 工具状态

| 工具 | 显示名 | 状态 |
|------|--------|------|
| claude | Claude Code | ✅ |
| claude-desktop | Claude Desktop | ✅ |
| codex | Codex | ✅ |
| gemini | Gemini | ✅ |
| opencode | OpenCode | ✅ |
| openclaw | OpenClaw | ✅ |
| hermes | Hermes | ✅ |

---

## 二、Provider 管理

### 2.1 查看 Provider

```bash
# 查看当前 Provider 配置
cat ~/.cc-switch/settings.json | grep currentProvider
```

### 2.2 Provider 类型

| 工具 | Provider 选项 |
|------|--------------|
| Claude | default, claude-3-5-sonnet, claude-3-opus |
| Claude Desktop | default |
| Gemini | default, gemini-pro |
| OpenAI | default |

### 2.3 切换 Provider

```bash
# 如果支持命令行切换
cc-switch set-provider claude default
cc-switch set-provider claude claude-3-5-sonnet
```

---

## 三、Skills 同步

### 3.1 同步方式

```json
// settings.json
{
  "skillSyncMethod": "symlink",
  "skillStorageLocation": "cc_switch"
}
```

两种模式：
- **symlink**：符号链接，修改同步
- **copy**：复制文件，独立修改

### 3.2 同步命令

```bash
# 同步 Skills（如果支持）
cc-switch sync-skills

# 或手动同步
cd ~/.cc-switch/skills/
git pull  # 如果有 Git 仓库
```

### 3.3 Skills 备份

```
~/.cc-switch/skill-backups/
```

---

## 四、MCP 集成

### 4.1 MCP 相关 Skills

| Skill | 功能 |
|-------|------|
| `mcp-builder` | MCP Server 构建 |
| `mcp-*` | MCP 相关工具 |

### 4.2 MCP 配置

```bash
# 查看 MCP 配置位置
ls ~/.claude/mcp.json
ls ~/.hermes/config.yaml  # 包含 MCP 配置
```

### 4.3 与 cc-switch 的集成

cc-switch 管理的工具可以通过 MCP 协议连接：

```
cc-switch (Claude Code) ←→ MCP Memory Server ←→ Hermes
```

---

## 五、系统托盘

### 5.1 托盘功能

```json
// settings.json
{
  "showInTray": true,
  "minimizeToTrayOnClose": true
}
```

托盘图标提供：
- 快速切换工具
- 查看状态
- 打开设置窗口
- 退出应用

### 5.2 托盘菜单

| 菜单项 | 功能 |
|--------|------|
| 打开主窗口 | 显示 App Window |
| 切换工具 | 子菜单选择工具 |
| 设置 | 打开设置 |
| 退出 | 关闭应用 |

---

## 六、本地代理

### 6.1 代理配置

```json
// settings.json
{
  "enableLocalProxy": true,
  "proxyConfirmed": true
}
```

### 6.2 代理用途

- API 请求转发
- 故障转移
- 流量监控

### 6.3 代理端口

```bash
# 查看代理配置
# 通常在 localhost 上运行
```

---

## 七、故障转移

### 7.1 故障转移配置

```json
// settings.json
{
  "enableFailoverToggle": true,
  "failoverConfirmed": true
}
```

### 7.2 故障转移场景

| 场景 | 处理方式 |
|------|----------|
| Provider 不可用 | 自动切换到备选 Provider |
| API 超时 | 重试或切换 |
| 限流 | 等待后重试 |

### 7.3 手动切换

```bash
# 禁用故障转移
cc-switch failover disable

# 启用故障转移
cc-switch failover enable
```

---

## 八、日志与监控

### 8.1 日志目录

```
~/.cc-switch/logs/
```

### 8.2 日志查看

```bash
# 查看日志目录
ls -la ~/.cc-switch/logs/

# 查看日志文件
cat ~/.cc-switch/logs/cc-switch.log
```

### 8.3 日志配置

```json
// 如果支持日志级别配置
{
  "logLevel": "info"  // debug, info, warn, error
}
```

---

## 九、常用命令速查

| 命令 | 功能 |
|------|------|
| `cc-switch list-apps` | 列出可用工具 |
| `cc-switch switch <tool>` | 切换工具 |
| `cc-switch run <tool>` | 运行工具 |
| `cc-switch set-provider` | 设置 Provider |
| `cc-switch sync-skills` | 同步 Skills |
| `cc-switch failover` | 故障转移控制 |

---

## 💡 今日要点

> 1. **工具切换**：`list-apps` / `switch` / `run`
> 2. **Provider 管理**：统一管理 API 配置
> 3. **Skills 同步**：`symlink` 或 `copy` 模式
> 4. **系统托盘**：快速切换和状态查看
> 5. **故障转移**：自动切换 Provider

---

## 📝 个人笔记区

---

## 🔗 关联资源

- [[cc-switch 2 -安装与配置]]
- [[cc-switch 4 -Skills 管理]]
- [[cc-switch 5 -高级功能]]
- [[00-cc-switch课程目录]]

---

*Tags: #cc-switch #核心功能 #工具切换 #lesson-3*