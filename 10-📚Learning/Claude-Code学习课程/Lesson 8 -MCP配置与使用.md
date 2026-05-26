# Lesson 8: MCP 配置与使用

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已学习
> 📌 进阶课程 #2

---

## 一、什么是 MCP

### 1.1 MCP 定义

MCP (Model Context Protocol) 是 Claude Code 的**扩展协议**，让它能连接外部工具和服务。

```
Claude Code ←→ MCP ←→ 外部工具
           ↓
     Obsidian / GitHub / 数据库
```

### 1.2 MCP vs 内置工具

| 对比 | 内置工具 | MCP |
|------|----------|-----|
| **数量** | 有限（Read/Write/Bash等）| 无限（社区贡献）|
| **功能** | 基础文件操作 | 专业工具集成 |
| **配置** | 自动可用 | 需要手动配置 |

---

## 二、MCP 服务器配置

### 2.1 查看已配置的 MCP

```bash
claude mcp list
```

### 2.2 添加 MCP 服务器

```bash
# 语法
claude mcp add <name> <command>

# 示例：添加文件系统 MCP
claude mcp add filesystem npx @modelcontextprotocol/server-filesystem /path/to/dir

# 示例：添加 GitHub MCP
claude mcp add github npx @modelcontextprotocol/server-github
```

### 2.3 移除 MCP 服务器

```bash
claude mcp remove <name>
```

### 2.4 MCP 配置文件

MCP 配置存放在 `~/.claude/settings.json` 或项目 `.claude/mcp.json`：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/path/to/dir"]
    },
    "obsidian": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-obsidian", "--vault-path", "/path/to/vault"]
    }
  }
}
```

---

## 三、常用 MCP 服务器推荐

### 3.1 文件系统

```bash
claude mcp add filesystem npx @modelcontextprotocol/server-filesystem ~/
```

**功能**：增强的文件操作能力

### 3.2 Obsidian

```bash
claude mcp add obsidian npx @modelcontextprotocol/server-obsidian \
  --vault-path ~/Newone_Obsidian-Vault/Newone_Obsidian-Vault
```

**功能**：直接读写 Obsidian 笔记

### 3.3 GitHub

```bash
claude mcp add github npx @modelcontextprotocol/server-github \
  --github-token ghp_xxxxxxxxxxxxx
```

**功能**：GitHub 仓库管理、Issue、PR 操作

### 3.4 SQLite

```bash
claude mcp add sqlite npx @modelcontextprotocol/server-sqlite \
  --db-path ./database.db
```

**功能**：数据库查询操作

### 3.5 更多 MCP 服务器

| 服务器 | 用途 | 命令 |
|--------|------|------|
| `brew` | Homebrew 包管理 | `npx @modelcontextprotocol/server-brew` |
| `git` | Git 操作增强 | `npx @modelcontextprotocol/server-git` |
| `slack` | Slack 消息 | `npx @modelcontextprotocol/server-slack` |
| `sentry` | 错误监控 | `npx @modelcontextprotocol/server-sentry` |

---

## 四、Obsidian MCP 详细配置

### 4.1 安装步骤

```bash
# 1. 安装 MCP 服务器
npm install -g @modelcontextprotocol/server-obsidian

# 2. 添加到 Claude Code
claude mcp add obsidian npx @modelcontextprotocol/server-obsidian \
  --vault-path ~/Newone_Obsidian-Vault/Newone_Obsidian-Vault

# 3. 验证
claude mcp list
# 应该看到 obsidian 在列表中
```

### 4.2 使用示例

```
# 读取笔记
Claude：读取我的 Obsidian 笔记 "学习课程"

# 搜索笔记
Claude：在 Vault 中搜索包含 "Claude Code" 的笔记

# 创建笔记
Claude：在 Obsidian 中创建新笔记 "今日待办"
```

### 4.3 权限设置

Obsidian MCP 需要在 Obsidian 中启用 Community Plugins API：
1. 打开 Obsidian 设置
2. Community Plugins → 关闭安全模式
3. 安装 Local REST API 插件

---

## 五、MCP 故障排除

### 5.1 常见问题

| 问题 | 解决 |
|------|------|
| MCP 不出现在列表 | 重启 Claude Code |
| 连接失败 | 检查命令是否正确 |
| 权限不足 | 确认 vault 路径正确 |

### 5.2 诊断步骤

```bash
# 1. 查看 MCP 列表
claude mcp list

# 2. 测试 MCP 连接
npx @modelcontextprotocol/server-filesystem --help

# 3. 查看错误日志
cat ~/.claude/telemetry/*.log | grep -i mcp
```

### 5.3 调试技巧

```bash
# 手动运行 MCP 服务器查看输出
npx @modelcontextprotocol/server-filesystem ~/
```

---

## 六、MCP 资源

### 6.1 官方 MCP 服务器
- https://github.com/modelcontextprotocol/servers

### 6.2 社区 MCP 服务器
- https://mcpservers.org
- https://smith.dev/mcp

### 6.3 MCP  marketplaces
- https://glama.ai/mcp registry
- https://chats.dev/plugins

---

## 七、推荐 MCP 组合

### 组合 1: Obsidian 知识库
```
filesystem + obsidian
```
用于：笔记读写、搜索、知识管理

### 组合 2: 开发工作流
```
filesystem + github + sqlite
```
用于：代码开发、Git 管理、数据查询

### 组合 3: 生产力工具
```
filesystem + slack + github
```
用于：团队协作、通知推送、代码管理

---

## 💡 今日要点

> 1. **MCP = 扩展连接器**：让 Claude Code 连接更多工具
> 2. **claude mcp add**：添加新的 MCP 服务器
> 3. **claude mcp list**：查看已配置的 MCP
> 4. **Obsidian MCP**：最推荐的配置，用于知识管理

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学会，无疑问

---

## 🔗 关联资源

- [[Lesson 9: 自动化工作流]]
- [MCP 官方文档](https://modelcontextprotocol.org)
- [MCP Servers GitHub](https://github.com/modelcontextprotocol/servers)

---

*Tags: #claude-code #进阶课程 #lesson-8 #MCP #扩展*