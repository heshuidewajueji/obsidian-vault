# Hermes 9: 实战应用与最佳实践

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已完成
> 📌 实战课程 #1

---

## 一、实战工作流

### 1.1 晨间启动流程

```
┌─────────────────────────────────────────────────────────────┐
│                    每日 Hermes 晨间流程                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  06:00 - Hermes (飞书) 自动唤醒                              │
│     │                                                      │
│     ├── 推送今日任务（from Cron）                           │
│     ├── 天气 + 日程摘要                                     │
│     └── 昨日未完成任务提醒                                  │
│                                                             │
│  08:00 - 你到达办公室                                       │
│     │                                                      │
│     └── "今天继续项目 X"                                   │
│         │                                                  │
│         ├── Hermes 读取记忆：项目上下文                     │
│         ├── Hermes 读取记忆：上次进展                      │
│         └── 继续工作                                       │
│                                                             │
│  10:00 - 遇到编程问题                                       │
│     │                                                      │
│     ├── "让 Claude Code 帮我分析"                          │
│     ├── Hermes 记录 CC 分析结果                             │
│     └── 继续执行                                            │
│                                                             │
│  18:00 - 收工                                               │
│     │                                                      │
│     └── "收工" → Hermes 自动生成今日总结                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 典型使用场景

| 场景 | 命令/触发 | Hermes 响应 |
|------|----------|-------------|
| 查询记忆 | "上次项目 X 讨论了什么" | 搜索会话历史，返回摘要 |
| 创建技能 | "把这个流程记下来" | 生成 SKILL.md |
| 跨工具协作 | "让 Claude Code 分析" | 记录结果到记忆 |
| 定时任务 | Cron 自动触发 | 推送报告/提醒 |
| 消息转发 | Telegram/飞书 | 多平台同步 |

---

## 二、日常命令速查

### 2.1 核心命令

```bash
# 对话交互
hermes chat -q "你的问题"          # 单次查询
hermes chat                          # 交互模式
hermes chat --tui                    # TUI 界面

# 记忆管理
hermes memory add <key> <value>      # 添加记忆
hermes memory list                   # 列出记忆
hermes memory remove <key>           # 删除记忆

# 技能管理
hermes skills list                   # 列出所有技能
hermes skills inspect <name>        # 查看技能详情
hermes skills install <name>         # 安装技能

# MCP 服务器
hermes mcp list                      # 列出 MCP 服务器
hermes mcp test <name>               # 测试连接
hermes mcp add <name> ...            # 添加服务器

# 网关管理
hermes gateway run                   # 启动网关
hermes gateway status                # 查看状态

# 系统
hermes doctor                       # 健康检查
hermes config show                   # 查看配置
```

### 2.2 快捷命令（别名）

```bash
# ~/.zshrc 或 ~/.bashrc 中添加

# Hermes 快捷命令
alias hm='hermes'
alias hmc='hermes chat'
alias hml='hermes memory list'
alias hms='hermes skills list'
alias hmg='hermes gateway'

# 快速查询记忆
alias hm-search='hermes chat -q "搜索记忆："'
```

### 2.3 Telegram 快捷命令

```
/chat <问题>          # CLI 等效
/memory add <key>     # 添加记忆
/memory list          # 列出记忆
/skills               # 列出技能
/gateway             # 网关状态
```

---

## 三、最佳实践

### 3.1 记忆管理

**Do ✅**：
- 重要决策后主动保存到记忆
- 使用统一的记忆命名规范：`project-name/xxx`
- 定期用 `hermes-self-evolution` 复盘

**Don't ❌**：
- 不要什么都记（保持记忆精简）
- 不要用模糊的命名（如 "test"、"note"）
- 不要忘记清理过期记忆

**记忆命名规范**：
```
# 推荐格式
project/api-v2/status          # 项目/模块/类型
user/preferences/editor         # 用户/类型/具体
work/routine/daily-standup     # 工作/类型/场景

# 不推荐
test
note
important
```

### 3.2 技能创建

**什么时候创建技能**：
- 重复执行的任务（超过 3 次）
- 复杂的命令序列
- 团队共享的操作流程

**技能命名规范**：
```
# 推荐格式
<category>-<action>
project-api-analyzer
devops-deploy-script
data-clean-pipeline
```

**技能文件结构**：
```markdown
---
name: skill-name
description: "简短描述"
triggers:
  - conversation_keywords
  - session_end
---

# 技能标题

## When to Use
何时使用此技能

## Usage
基本用法

## Examples
示例

## Troubleshooting
常见问题
```

### 3.3 配置管理

**配置优先级**（从高到低）：
1. 命令行参数 `--ignore-user-config`
2. 环境变量 `HERMES_XXX`
3. `~/.hermes/config.yaml`
4. 内置默认值

**安全建议**：
- API Keys 放在 `~/.hermes/.env`，不要提交到 git
- 敏感配置使用环境变量
- 定期备份：`hermes backup`

---

## 四、故障排查

### 4.1 常见问题

| 问题 | 症状 | 解决方案 |
|------|------|----------|
| 网关无法启动 | "Port already in use" | `kill $(lsof -i:xxx)` |
| API Key 无效 | "Authentication failed" | 检查 `~/.hermes/.env` |
| 技能不加载 | `skills list` 为空 | 检查 `~/.hermes/skills/` 权限 |
| MCP 连接失败 | `mcp test` 超时 | 检查命令路径和超时设置 |
| 飞书连接断开 | WebSocket error | `hermes doctor` 检查 |

### 4.2 诊断命令

```bash
# 1. 完整诊断
hermes doctor

# 2. 检查配置
hermes config show

# 3. 检查日志
tail -100 ~/.hermes/logs/gateway.log
tail -100 ~/.hermes/logs/mcp-stderr.log

# 4. 检查进程
ps aux | grep hermes

# 5. 检查端口占用
lsof -i :<port>
```

### 4.3 恢复操作

```bash
# 恢复配置
git clone https://github.com/heshuidewajueji/hermes-config.git ~/.hermes
cd ~/.hermes && ./restore.sh

# 重置网关
pkill -f "hermes.*gateway"
launchctl unload ~/Library/LaunchAgents/ai.hermes.gateway.plist
launchctl load ~/Library/LaunchAgents/ai.hermes.gateway.plist
```

---

## 五、效率技巧

### 5.1 会话管理

```
# 继续上次会话
hermes chat --continue

# 查看历史会话
hermes sessions list

# 恢复指定会话
hermes chat --resume <session-id>
```

### 5.2 批量操作

```bash
# 批量安装技能
for skill in skill1 skill2 skill3; do
  hermes skills install $skill
done

# 批量清理记忆
hermes memory list | grep "old-" | xargs -I {} hermes memory remove {}

# 批量测试 MCP
for server in memory minimax; do
  hermes mcp test $server
done
```

### 5.3 自定义 Prompt

在 `~/.hermes/config.yaml` 中添加：

```yaml
agent:
  system_prompt_additions: |
    你是一个专业的开发者助手。
    当用户提到 "性能" 时，主动检查代码效率。
    当用户提到 "安全" 时，提醒潜在风险。
```

---

## 六、与其他工具集成

### 6.1 Claude Code 协作

```bash
# 通过 Hermes 调用 Claude Code
hermes chat -q "让 Claude Code 审查 ~/project/code"

# 通过 Claude Code 记录到 Hermes
claude -p "记录：审查完成，发现 3 个问题"

# MCP 共享记忆（参考 Lesson 6）
python3 ~/scripts/mcp_memory_cli.py read_graph
```

### 6.2 cc-switch 协作

```bash
# cc-switch 管理的 skills 在 ~/.cc-switch/skills/
# Hermes 通过软链接访问

# 查看共享 skills
ls ~/.cc-switch/skills/
ls ~/.hermes/skills/

# 同步新 skills
ln -sf ~/.cc-switch/skills/<skill-name> ~/.hermes/skills/
```

### 6.3 Obsidian 协作

```bash
# 将重要记录备份到 Obsidian
hermes chat -q "将今日学习总结写入 Obsidian"

# 读取 Obsidian 笔记
hermes chat -q "读取 20-AI与工具/00-AI-Shared/memory.md"
```

---

## 七、自定义配置示例

### 7.1 最小配置

```yaml
# ~/.hermes/config.yaml（最小化）
model:
  provider: minimax
  default: MiniMax-M2.7
  base_url: https://api.minimaxi.com/v1
mcp_servers:
  memory:
    command: node
    args:
    - /path/to/mcp-memory-sqlite/dist/index.js
```

### 7.2 完整配置

```yaml
# ~/.hermes/config.yaml（完整）
model:
  provider: minimax
  default: MiniMax-M2.7
  base_url: https://api.minimaxi.com/v1
  api_key: ${MINIMAX_API_KEY}

toolsets:
  - hermes-cli
  - mcp

memory:
  memory_enabled: true
  user_profile_enabled: true
  memory_char_limit: 2200

mcp_servers:
  memory:
    command: node
    args:
    - /Users/king-macbookair/mcp-memory-sqlite/dist/index.js
    env:
      SQLITE_DB_PATH: /Users/king-macbookair/mcp-memory-sqlite/sqlite-memory.db
    enabled: true

cronjob:
  daily_review:
    schedule: "0 22 * * *"
    prompt: "生成今日工作总结"
    deliver: "origin"
```

---

## 八、检查清单

### 部署前检查

- [ ] `hermes doctor` 无红色警告
- [ ] API Keys 配置正确
- [ ] MCP 服务器连接正常
- [ ] 消息平台（飞书/Telegram）连接正常
- [ ] 记忆目录存在且可写

### 日常检查

- [ ] 晨间：检查 Hermes 推送
- [ ] 工作中：主动保存重要决策
- [ ] 收工：执行 `收工` 命令生成总结

---

## 💡 今日要点

> 1. **命令速查**：高频命令优先记忆
> 2. **记忆精简**：只记重要决策和偏好
> 3. **技能复用**：重复任务超过 3 次就创建技能
> 4. **故障排查**：先用 `hermes doctor`
> 5. **效率工具**：善用别名和批量操作

---

## 📝 个人笔记区
1、已学习，无问题

---

## 🔗 关联资源

- [[Hermes 1 -认识Hermes]]
- [[Hermes 3 -核心功能]]
- [[Hermes 5 -进阶用法]]
- [[Hermes 6 -与Claude Code协作]]
- [[Hermes 7 -配置维护]]
- [[Hermes 8 -自我进化系统]]

---

*Tags: #hermes #实战 #最佳实践 #troubleshooting #效率*