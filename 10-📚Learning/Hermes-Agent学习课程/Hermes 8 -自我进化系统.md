# Hermes 8: 自我进化系统

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已完成
> 📌 进阶课程 #2

---

## 一、自我进化核心概念

### 1.1 什么是内建学习闭环

Hermes Agent 是**唯一**拥有内置学习闭环的 AI Agent 框架：

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   用户对话 ──► 经验积累 ──► 技能创建 ──► 技能优化          │
│       ▲                                      │              │
│       │                                      ▼              │
│       ◄────────── 记忆召回 ◄────────────────┘              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**核心特性**：
- 智能记忆管理，定期自我提醒
- 复杂任务后自主创建技能
- 使用过程中技能自我改进
- FTS5 会话搜索 + LLM 摘要跨会话记忆

### 1.2 与传统 AI Agent 的区别

| 维度 | 传统 AI Agent | Hermes |
|------|---------------|--------|
| 记忆 | 每次会话重置 | 持久化 + 智能召回 |
| 技能 | 固定技能集 | 自动从经验创建 |
| 优化 | 无 | 持续自我改进 |
| 学习 | 需要手动 | 自主驱动 |

---

## 二、记忆系统

### 2.1 记忆类型

```
Hermes 记忆
├── 短期记忆（会话内）
│   └── 当前对话上下文
├── 中期记忆（跨会话）
│   └── MEMORY.md（用户偏好、项目约定）
└── 长期记忆（持久化）
    └── 技能系统（.skills/）
```

### 2.2 记忆管理工具

| 工具 | 功能 |
|------|------|
| `memory` | add/replace/remove 持久记忆 |
| `session_search` | FTS5 全文搜索历史会话 |
| `skills_list` | 列出所有可用技能 |
| `skills_update` | 更新技能内容 |

### 2.3 记忆触发机制

Hermes 在以下场景会自动管理记忆：

1. **复杂任务完成** → 提示保存关键信息
2. **用户偏好表达** → 自动记录到 MEMORY.md
3. **项目约定** → 持久化到项目记忆
4. **定期回顾** → 定期检查记忆有效性

---

## 三、技能系统

### 3.1 技能结构

```
~/.hermes/
├── .skills/
│   ├── builtin/           # 内置技能
│   │   ├── hermes-agent/
│   │   └── hermes-cli/
│   ├── mcp/              # MCP 相关技能
│   │   └── native-mcp/
│   ├── custom/           # 用户自定义技能
│   │   └── memory-mcp/
│   └── optional-skills/   # 可选技能
│       └── research/
│           └── darwinian-evolver/
└── skills/
    └── mcp/
```

### 3.2 技能文件格式

```markdown
---
name: skill-name
description: "技能描述"
version: 1.0.0
author: 作者
platforms: [linux, macos, windows]
---

# 技能标题

## When to Use
何时使用此技能

## Usage
使用说明

## Examples
示例
```

### 3.3 技能自动创建

Hermes 可以从对话经验自动创建技能：

```
用户：帮我分析这个 API 性能问题

Hermes：
[分析完成] 这是一个典型的 N+1 查询问题。
我注意到你经常需要分析 API 性能，要不要我创建一个专门的技能？

用户：好

Hermes：
[创建 skills/custom/api-analyzer/SKILL.md]
以后只需要说 "分析 API 性能" 就可以快速诊断。
```

---

## 四、用户建模 (Honcho Dialectic)

### 4.1 概念

"Honcho dialectic user modeling" 是 Hermes 的用户画像系统：

- 跨会话深化用户理解
- 记住用户偏好、工作风格、沟通习惯
- 自动适应用户的表达方式

### 4.2 实现方式

```
~/.hermes/
├── memories/
│   ├── MEMORY.md      # 用户偏好和约定
│   └── USER.md        # 用户画像
└── .skills/
    └── preferences/    # 偏好相关技能
```

### 4.3 用户建模示例

```
第一次：用户说 "这个代码有问题"
Hermes：记录用户的调试风格

第二次：用户说 "出错了"
Hermes：理解这是 "有问题" 的同义表达

第三次：用户说 "bug"
Hermes：自动关联到 "有问题" 和 "出错"
```

---

## 五、进化机制

### 5.1 技能自我优化

Hermes 会在使用过程中优化技能：

```markdown
# 原始技能
- 当用户询问天气时，使用 web_search 工具

# 优化后
- 当用户询问天气时，首先检查用户时区（从 MEMORY.md 读取）
- 如果用户在中国，使用中文天气源
- 如果用户在国外，使用当地天气源
```

### 5.2 达尔文进化器 (Darwinian Evolver)

> ⏸️ **状态**：待定 - 当前使用场景优先级低

可选的进化工具，用于优化 prompts/regex/SQL/code：

**价值评估**：
- ✅ 适合：明确评估标准的任务（regex 通过率、SQL 性能）
- ❌ 不适合：主观创意、结构优化、学习记录
- 结论：对于 skill 优化，hermes-self-evolution + Claude Code 更实用

**安装命令**（如果后续需要）：
```bash
mkdir -p ~/.hermes/hermes-agent/optional-skills/research/
cd ~/.hermes/hermes-agent/optional-skills/research/
git clone --depth 1 https://github.com/imbue-ai/darwinian_evolver.git
cd darwinian_evolver && uv sync
```

### 5.3 进化工作流

```
1. 定义问题（Problem）
   - organism: 初始候选
   - evaluator: 评估函数
   - mutator: 变异策略

2. 启动进化循环
   - 50-500 次 LLM 调用
   - 自然选择最优个体

3. 获取优化结果
   - 最优 prompt/regex/SQL/code
   - 进化历史记录
```

---

## 六、实践应用

### 6.1 启用自我进化

```bash
# 确保记忆功能开启
hermes config set memory.enabled true

# 检查技能目录
ls ~/.hermes/.skills/

# 查看当前记忆
cat ~/.hermes/memories/MEMORY.md
```

### 6.2 手动触发学习

```
你：帮我记住我的项目目录结构
Hermes：好的，请描述你的项目结构

你：我的项目在 ~/projects/，主要分为 api、web、mobile 三个子项目

Hermes：
[已记住] 项目结构：~/projects/{api,web,mobile}
以后你说 "项目" 我就知道是指 ~/projects/

你：以后分析代码直接去 api 目录

Hermes：
[已更新] 代码分析默认目录：~/projects/api
```

### 6.3 技能创建示例

```markdown
---
name: project-api-analyzer
description: "分析 API 项目性能和结构"
version: 1.0.0
author: User
---

# API 项目分析

分析 ~/projects/api 目录下的代码：

## 检测项目
- N+1 查询问题
- 缺失索引
- 性能瓶颈

## 常用命令
- `npx @anthropic-ai/claude-code -p "分析 API"` → 代码审查
- `sqlite3 db.sqlite` → 数据库检查
```

---

## 七、配置与维护

### 7.1 记忆配置

```yaml
# ~/.hermes/config.yaml
memory:
  memory_enabled: true
  user_profile_enabled: true
  memory_char_limit: 2200
  user_char_limit: 1375
```

### 7.2 技能目录

```bash
# 技能目录结构
~/.hermes/.skills/          # 主要技能目录
~/.hermes/skills/           # 用户技能目录
~/.hermes/hermes-agent/skills/  # Hermes 内置技能

# 查看所有技能
hermes skills list

# 查看技能详情
hermes skills inspect <skill-name>
```

### 7.3 记忆清理

```bash
# 执行清理脚本
bash ~/scripts/clean-shared-memory.sh

# 手动清理过期记忆
hermes memory remove <key>
```

---

## 八、关联文件

| 文件 | 路径 | 说明 |
|------|------|------|
| 用户记忆 | `~/.hermes/memories/MEMORY.md` | 核心记忆存储 |
| 用户画像 | `~/.hermes/memories/USER.md` | 用户偏好 |
| 技能目录 | `~/.hermes/.skills/` | 所有技能 |
| MCP 技能 | `~/.hermes/skills/mcp/native-mcp/` | MCP 工具 |
| 达尔文进化器 | `~/.hermes/cache/darwinian-evolver/` | 进化工具 |

---

## 💡 今日要点

> 1. **内建学习闭环**：唯一具有内置学习能力的 AI Agent
> 2. **三层记忆**：短期 → 中期 → 长期
> 3. **技能自动创建**：从经验生成可复用技能
> 4. **用户建模**：跨会话深化用户理解
> 5. **进化机制**：技能持续自我优化

---

## 📝 个人笔记区

**Q1：我电脑现在装有 cc-switch，由它统一管理 skill，不同 AI Agent 需要时创建软连接，这个情况怎么配置达尔文进化器？**

### 你的 cc-switch 架构

```
~/.cc-switch/
├── skills/              # cc-switch 统一管理（67 个 skills）
│   ├── hermes-self-evolution/   ← 自进化 skill
│   └── ...其他 skills...
├── config.yaml           # cc-switch 配置
└── ...

~/.hermes/
├── skills/              # Hermes 用户 skills
│   ├── mcp/
│   └── custom/
└── hermes-agent/optional-skills/research/
    └── darwinian-evolver/      ← 达尔文进化器
```

**确认**：已存在 `hermes-self-evolution` skill（cc-switch 管理）

### 推荐方案

#### 方案 A：保持现状（推荐）

```
达尔文进化器位置：~/.hermes/hermes-agent/optional-skills/research/darwinian-evolver/
                                    ↑
                                    直接安装，不通过 cc-switch

cc-switch 管理的 skills：~/.cc-switch/skills/hermes-self-evolution/ ✅ 已存在
```

**理由**：
- 达尔文进化器是 Hermes 专用工具，放在 Hermes 目录更合理
- `optional-skills/research/` 就是为这类可选工具设计的
- 如果其他 Agent 需要，通过软链接共享

#### 方案 B：统一到 cc-switch（可选）

如果你想让 cc-switch 管理达尔文进化器：

```bash
# 1. 在 cc-switch 中创建目录（如果不存在）
mkdir -p ~/.cc-switch/skills/darwinian-evolver/

# 2. 移动或克隆到达文进化器
cd ~/.cc-switch/skills/darwinian-evolver/
git clone --depth 1 https://github.com/imbue-ai/darwinian_evolver.git

# 3. 创建软链接到 Hermes
ln -sf ~/.cc-switch/skills/darwinian-evolver \
  ~/.hermes/hermes-agent/optional-skills/research/darwinian-evolver

# 4. 如果 Claude Code 也需要
mkdir -p ~/.claude/skills/
ln -sf ~/.cc-switch/skills/darwinian-evolver \
  ~/.claude/skills/darwinian-evolver
```

#### 架构图（方案 A）

```
~/.cc-switch/skills/                    # cc-switch 管理
├── hermes-self-evolution/    ✅ 已存在
├── ...其他 66 个 skills...
└──

~/.hermes/hermes-agent/optional-skills/research/
└── darwinian-evolver/                  # 直接安装（Hermes 专用）
    ├── darwinian_evolver/               # 克隆的仓库
    ├── cache/                           # 进化缓存
    └── SKILL.md                          # 技能说明
```

#### 验证安装

```bash
# 检查 cc-switch 自进化 skill
ls ~/.cc-switch/skills/hermes-self-evolution/

# 检查达尔文进化器
ls ~/.hermes/hermes-agent/optional-skills/research/darwinian-evolver/

# 测试达尔文进化器
cd ~/.hermes/hermes-agent/optional-skills/research/darwinian-evolver/darwinian_evolver
uv run darwinian_evolver --help
```

---

## 🔗 关联资源

- [[Hermes 1 -认识Hermes]]
- [[Hermes 3 -核心功能]]
- [[Hermes 5 -进阶用法]]
- [[Hermes 7 -配置维护]]
- [[Hermes 10 -工程控制论与自监控体系]]

---

*Tags: #hermes #self-evolution #learning-loop #skill-system #memory*