# OpenClaw 5: 记忆系统

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #5

---

## 一、Vestige 记忆系统

### 1.1 什么是 Vestige

Vestige 是 OpenClaw 的向量记忆存储系统：

- **语义搜索**：基于向量相似度
- **跨 Agent 共享**：多 Agent 共享同一记忆池
- **自动学习**：从对话中提取关键信息
- **持久存储**：长期保存重要记忆

### 1.2 与 Hermes 记忆对比

| 维度 | OpenClaw (Vestige) | Hermes (文本) |
|------|-------------------|--------------|
| 存储方式 | 向量嵌入 | Markdown 文本 |
| 搜索方式 | 语义相似度 | 关键词匹配 |
| 跨 Agent | ✅ 原生支持 | ❌ 需要 MCP |
| 检索精度 | 高 | 中 |
| 实现复杂度 | 中 | 低 |

### 1.3 记忆架构

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   用户对话                                                    │
│       │                                                      │
│       ▼                                                      │
│   ┌─────────┐                                               │
│   │ Vestige │ ← 向量化引擎                                   │
│   │ Engine  │                                               │
│   └────┬────┘                                               │
│        │                                                      │
│        ▼                                                      │
│   ┌─────────────┐                                           │
│   │  向量数据库  │ ← 记忆存储                                 │
│   │  (内存/SQL) │                                           │
│   └─────────────┘                                           │
│                                                             │
│   ┌─────────────┐                                           │
│   │  Agent A    │ ← 共享访问                                 │
│   │  Agent B    │ ← 共享访问                                 │
│   │  Agent C    │ ← 共享访问                                 │
│   └─────────────┘                                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 二、记忆操作

### 2.1 查看记忆

```bash
# 查看记忆统计
openclaw memory stats

# 查看记忆状态
openclaw memory status

# 列出所有记忆
openclaw memory list
```

### 2.2 搜索记忆

```bash
# 语义搜索
openclaw memory search "项目经验"

# 带结果数限制
openclaw memory search "项目经验" --limit 5

# 搜索特定 Agent
openclaw memory search "项目经验" --agent coding
```

### 2.3 添加记忆

```bash
# 添加记忆
openclaw memory add --content "用户偏好使用 TypeScript" --tags typescript,preference

# 指定 Agent
openclaw memory add --content "项目代号 Alpha" --agent main --tags project

# 指定重要性
openclaw memory add --content "关键信息" --importance high
```

### 2.4 删除记忆

```bash
# 删除记忆
openclaw memory delete <memory-id>

# 按标签删除
openclaw memory delete --tag temporary

# 清空所有记忆
openclaw memory purge
```

---

## 三、记忆导入导出

### 3.1 导出记忆

```bash
# 导出所有记忆
openclaw memory export > backup.json

# 导出特定 Agent
openclaw memory export --agent coding > coding_memory.json

# 导出带时间范围
openclaw memory export --before 2026-06-01 > backup.json
```

### 3.2 导入记忆

```bash
# 导入记忆
openclaw memory import backup.json

# 导入并指定 Agent
openclaw memory import backup.json --agent main

# 合并模式（不去重）
openclaw memory import backup.json --merge
```

### 3.3 格式说明

```json
// backup.json 格式
{
  "version": "1.0",
  "exported_at": "2026-05-24T12:00:00Z",
  "memories": [
    {
      "id": "mem_xxx",
      "content": "用户偏好使用 TypeScript",
      "tags": ["typescript", "preference"],
      "agent": "main",
      "created_at": "2026-05-20T10:00:00Z",
      "importance": "medium"
    }
  ]
}
```

---

## 四、标签系统

### 4.1 标签管理

```bash
# 查看所有标签
openclaw memory tags

# 添加标签到记忆
openclaw memory tag add <memory-id> --tags new-tag

# 移除标签
openclaw memory tag remove <memory-id> --tags old-tag
```

### 4.2 常用标签建议

| 标签 | 用途 | 示例 |
|------|------|------|
| `preference` | 用户偏好 | 喜欢 TypeScript |
| `project` | 项目信息 | 项目代号 Alpha |
| `learned` | 学到的知识 | Python 技巧 |
| `error` | 错误记录 | Bug 修复记录 |
| `todo` | 待办事项 | 需要完成的任务 |

### 4.3 按标签筛选

```bash
# 按标签搜索
openclaw memory search --tag preference "编码风格"

# 列出特定标签
openclaw memory list --tag project
```

---

## 五、跨 Agent 共享

### 5.1 共享机制

```bash
# 标记为共享
openclaw memory add --content "团队编码规范" --shared --tags规范

# 查看共享记忆
openclaw memory shared

# 取消共享
openclaw memory shared disable <memory-id>
```

### 5.2 Agent 间共享示例

```bash
# 1. 在 main agent 中添加共享记忆
openclaw agents switch main
openclaw memory add --content "用户工作时间是 UTC+8" --shared --tags timezone

# 2. 在 coding agent 中使用
openclaw agents switch coding
openclaw memory search "工作时间是"

# 输出：用户工作时间是 UTC+8
```

### 5.3 共享权限

```bash
# 设置 Agent 共享权限
openclaw agents config coding --set memory.shared.read true
openclaw agents config coding --set memory.shared.write false
```

---

## 六、自动学习

### 6.1 自动提取

OpenClaw 可以从对话中自动学习：

- **对话摘要**：自动提取关键信息
- **模式识别**：识别用户习惯
- **知识沉淀**：将新学到的知识存入记忆

### 6.2 学习配置

```bash
# 查看学习配置
openclaw memory learning config

# 启用自动学习
openclaw memory learning enable

# 禁用自动学习
openclaw memory learning disable
```

### 6.3 学习配置文件

```json
// ~/.openclaw/memory/learning.json
{
  "enabled": true,
  "auto_summary": true,
  "auto_tag": true,
  "min_confidence": 0.7,
  "excluded_tags": ["temp", "test"]
}
```

---

## 七、记忆清理

### 7.1 清理策略

```bash
# 查看过期记忆
openclaw memory cleanup --dry-run

# 执行清理（删除 90 天前的记忆）
openclaw memory cleanup --days 90

# 按重要性清理
openclaw memory cleanup --importance low --days 30
```

### 7.2 定时清理

```bash
# 设置自动清理
openclaw memory cleanup --schedule "0 2 * * 0"  # 每周日 02:00

# 查看清理计划
openclaw memory cleanup --schedule
```

### 7.3 清理配置文件

```json
// ~/.openclaw/memory/cleanup.json
{
  "schedule": "0 2 * * 0",
  "enabled": true,
  "retention_days": 90,
  "min_importance": "low"
}
```

---

## 八、与 Hermes 记忆对比

### 8.1 架构对比

```
┌─────────────────────────────────────────────────────────────┐
│                   OpenClaw vs Hermes 记忆                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   OpenClaw                                                  │
│   ┌─────────┐     ┌─────────┐     ┌─────────┐              │
│   │ Agent A │ ←→ │  Vestige │ ←→ │ Agent B │              │
│   └─────────┘     └────┬────┘     └─────────┘              │
│                        │                                    │
│                        ▼                                    │
│                  ┌───────────┐                              │
│                  │ 向量数据库 │                              │
│                  └───────────┘                              │
│                                                             │
│   Hermes                                                     │
│   ┌─────────┐     ┌─────────┐                              │
│   │  Hermes │ ←→ │ MEMORY.md│  ← 文本文件                    │
│   │ Agent   │     │  (本地)  │                              │
│   └─────────┘     └─────────┘                              │
│                                                             │
│   共享方案：通过 MCP 协议连接 mcp-memory-sqlite              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 8.2 选择建议

| 场景 | 推荐方案 |
|------|----------|
| 仅使用 OpenClaw | Vestige 原生记忆 |
| 仅使用 Hermes | MEMORY.md 文本记忆 |
| OpenClaw + Hermes | MCP 共享 SQLite |
| 需要向量搜索 | Vestige 或 SQLite |

---

## 九、实战示例

### 9.1 场景：构建项目知识库

```bash
# 1. 添加项目信息
openclaw memory add --content "项目代号：Phoenix" --tags project,phoenix
openclaw memory add --content "技术栈：React + Node.js + PostgreSQL" --tags tech-stack
openclaw memory add --content "团队会议：每周二 10:00" --tags meeting

# 2. 查询
openclaw memory search "项目代号"
# 输出：项目代号：Phoenix

# 3. 共享给团队 Agent
openclaw memory add --content "代码规范：TypeScript strict mode" --shared --tags规范
```

### 9.2 场景：备份记忆

```bash
# 1. 导出
openclaw memory export > ~/backup/openclaw_memory_$(date +%Y%m%d).json

# 2. 清理旧记忆
openclaw memory cleanup --days 180 --importance low

# 3. 验证
openclaw memory stats
```

### 9.3 场景：与 Hermes 共享记忆

```bash
# OpenClaw 端：导出记忆
openclaw memory export > ~/shared/openclaw_memory.json

# Hermes 端：导入到共享数据库
# 使用 MCP Memory Server 或手动合并

# 或设置 OpenClaw 直接使用 SQLite
openclaw config set memory.provider "sqlite"
openclaw config set memory.db_path "/Users/king-macbookair/mcp-memory-sqlite/sqlite-memory.db"
```

---

## 十、常用命令速查

| 命令 | 功能 |
|------|------|
| `openclaw memory stats` | 记忆统计 |
| `openclaw memory search` | 搜索记忆 |
| `openclaw memory add` | 添加记忆 |
| `openclaw memory list` | 列出记忆 |
| `openclaw memory export` | 导出记忆 |
| `openclaw memory import` | 导入记忆 |
| `openclaw memory cleanup` | 清理记忆 |
| `openclaw memory shared` | 共享记忆 |

---

## 💡 今日要点

> 1. **Vestige 向量**：语义搜索能力，跨 Agent 共享
> 2. **记忆操作**：add/search/list/export/import
> 3. **标签系统**：组织和管理记忆
> 4. **自动学习**：从对话中自动提取知识
> 5. **与 Hermes 对比**：Vestige vs 文本记忆 + MCP

---

## 📝 个人笔记区
1、已学习，无疑问

---

## 🔗 关联资源

- [[OpenClaw 4 -通道集成]]
- [[OpenClaw 6 -高级功能]]
- [[Hermes 3 -核心功能]]
- [[Hermes 6 -与Claude Code协作]]
- [[00-OpenClaw课程目录]]

---

*Tags: #openclaw #记忆 #vestige #向量 #lesson-5*