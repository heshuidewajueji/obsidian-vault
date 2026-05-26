# Lesson 99+: ~/.claude 目录详解

> 📅 学习日期: 2026-05-21
> 📝 学习状态: ✅ 已学习（2026-05-22）
> 📌 关联: [[Lesson 5: 项目管理]]

---

## 一、~/.claude 目录概述

`~/.claude` 是 Claude Code 的**全局配置目录**，相当于 Claude Code 的"家目录"。

### 你的当前目录结构

```
~/.claude/
├── CLAUDE.md              # 全局项目规范
├── config.json            # 全局配置
├── settings.json         # API 和模型配置
├── settings.local.json   # 本地权限配置
├── agents/               # 自定义代理（需创建）
├── commands/             # 自定义命令（需创建）
├── skills/               # 自定义技能（需创建）
├── memory.md             # 全局记忆（需创建）
├── projects/             # 项目配置记录
├── sessions/             # 会话历史
├── history.jsonl         # 对话历史
├── backups/              # 配置备份
├── cache/                # 缓存文件
├── tasks/                # 任务记录
├── telemetry/            # 使用数据
├── ide/                  # IDE 配置
└── file-history/         # 文件操作历史
```

---

## 二、核心文件详解

### 2.1 settings.json ⭐⭐⭐

**作用**：API 和模型配置

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "PROXY_MANAGED",
    "ANTHROPIC_BASE_URL": "http://127.0.0.1:15721",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "claude-haiku-4-5",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "claude-sonnet-4-6",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "claude-opus-4-7"
  },
  "name": "volcengine",
  "provider": "volcengine"
}
```

### 2.2 settings.local.json

**作用**：本地权限配置（你已配置的 allow 列表）

```
包含已授权的命令，如：
- Bash(curl *)
- Bash(claude mcp *)
- WebSearch
等
```

### 2.3 config.json

**作用**：全局配置文件

```json
{
  // Claude Code 全局设置
}
```

### 2.4 CLAUDE.md ⭐⭐⭐

**作用**：全局项目规范，所有项目都会读取

你刚刚创建了一个空的 `CLAUDE.md`。

---

## 三、需要手动创建的目录

这些目录**默认不存在**，需要时手动创建：

### 3.1 memory.md 🧠

**作用**：跨会话记忆，让 Claude 记住你的偏好

**创建方法**：
```bash
touch ~/.claude/memory.md
```

**内容示例**：
```markdown
# 我的记忆

## 基本信息
- 使用中文交流
- 喜欢表格输出

## 偏好设置
- 代码使用中文注释
- 重要操作前确认

## 项目说明
- 主要使用 Newone_Obsidian-Vault 作为知识库
- Claude Code 用于代码任务
```

### 3.2 agents/ 🤖

**作用**：存放自定义代理配置

**创建方法**：
```bash
mkdir -p ~/.claude/agents
```

**文件格式**：`.md` 文件

```markdown
# my-code-review-agent

> 代码审查代理

## 触发条件
description: "当用户要求代码审查时使用"

## 行为规范
- 专注于代码质量和安全性
- 使用表格输出问题
- 列出具体行号
```

### 3.3 commands/ ⌨️

**作用**：存放自定义命令

**创建方法**：
```bash
mkdir -p ~/.claude/commands
```

**文件格式**：`.md` 文件

```markdown
# save-and-commit

> 保存并提交

## 使用方法
/ save-and-commit "提交信息"

## 行为
1. 保存所有文件
2. 执行 git add
3. 执行 git commit -m "提交信息"
```

### 3.4 skills/ 🎯

**作用**：存放可复用技能

**创建方法**：
```bash
mkdir -p ~/.claude/skills
```

**文件格式**：`SKILL.md`

```markdown
# code-review-skill

> 代码审查技能

## 触发条件
description: "当用户请求代码审查时使用"

## 执行步骤
1. 读取目标文件
2. 检查代码规范
3. 查找潜在问题
4. 输出审查报告
```

---

## 四、自动生成的目录

这些目录由 Claude Code **自动生成**，不要手动修改：

### 4.1 projects/

记录你工作过的项目信息

### 4.2 sessions/

会话历史文件夹

### 4.3 history.jsonl

对话历史的 JSONL 格式记录

### 4.4 backups/

配置文件备份

### 4.5 cache/

临时缓存文件

### 4.6 telemetry/

使用数据（匿名）

---

## 五、人工维护指南

### 5.1 建议创建的文件

| 文件 | 命令 | 用途 |
|------|------|------|
| `memory.md` | `touch ~/.claude/memory.md` | 记忆你的偏好 |
| `agents/` | `mkdir ~/.claude/agents` | 自定义代理 |
| `commands/` | `mkdir ~/.claude/commands` | 自定义命令 |
| `skills/` | `mkdir ~/.claude/skills` | 自定义技能 |

### 5.2 CLAUDE.md 维护

建议在 `~/.claude/CLAUDE.md` 中添加：

```markdown
# 全局规范

## 我的偏好
- 使用中文交流
- 代码使用中文注释
- 表格输出结果

## 注意事项
- 删除文件前必须确认
- 执行危险命令前先规划

## 项目架构
- Claude Code → 代码开发
- Hermes → 日常对话
- OpenClaw → 任务执行
- Obsidian → 知识库
```

### 5.3 定期维护

```bash
# 定期备份配置
cp ~/.claude/settings.json ~/.claude/backups/settings.json.$(date +%Y%m%d)

# 清理旧缓存（可选）
rm -rf ~/.claude/cache/*
```

---

## 六、Obsidian 项目 vs 全局配置

| 位置 | 作用范围 | 优先级 |
|------|----------|--------|
| `~/.claude/CLAUDE.md` | 全局 | 低 |
| `~/.claude/memory.md` | 全局 | 中 |
| `项目/CLAUDE.md` | 项目级 | 高 |

**最佳实践**：
- 全局偏好放在 `~/.claude/memory.md`
- 项目规范放在各项目的 `CLAUDE.md`
- 临时需求直接对话说明

---

## 💡 今日要点

> 1. **默认不存在** agents/commands/skills/memory，需要手动创建
> 2. **不要动** 自动生成的目录（sessions/backups/cache）
> 3. **CLAUDE.md** 可放全局规范
> 4. **memory.md** 放你的个人偏好

---

## 📝 个人笔记区（老师已解答）

1、**默认不存在** agents/commands/skills/memory，需要手动创建，有什么规则需要遵循，怎么创建比较科学合理？
   > ✅ 回答：
   >
   > **创建规则汇总**：
   >
   > | 目录/文件 | 创建命令 | 文件格式 | 命名规则 |
   > |-----------|----------|----------|----------|
   > | `memory.md` | `touch ~/.claude/memory.md` | Markdown | 直接文件名 |
   > | `agents/` | `mkdir ~/.claude/agents` | `.md` 文件 | 小写+连字符 |
   > | `commands/` | `mkdir ~/.claude/commands` | `.md` 文件 | 小写+连字符 |
   > | `skills/` | `mkdir ~/.claude/skills` | `SKILL.md` | 子文件夹+SKILL.md |
   >
   > **科学合理的创建步骤**：
   >
   > ### 第一步：创建目录结构
   > ```bash
   > mkdir -p ~/.claude/agents
   > mkdir -p ~/.claude/commands
   > mkdir -p ~/.claude/skills
   > touch ~/.claude/memory.md
   > ```
   >
   > ### 第二步：初始化 memory.md
   > ```markdown
   > # 我的记忆
   >
   > ## 基本信息
   > - 姓名/昵称：
   > - 使用语言：中文
   >
   > ## 偏好设置
   > - 回复语言：中文
   > - 输出格式：表格优先
   >
   > ## AI 工具使用
   > - Claude Code → 代码开发
   > - Hermes → 日常对话
   > - OpenClaw → 任务执行
   > - Obsidian → 知识库
   > ```
   >
   > ### 第三步：创建第一个 Agent（可选）
   > ```bash
   > touch ~/.claude/agents/code-review.md
   > ```
   > 内容格式：
   > ```markdown
   > # code-review-agent
   >
   > > 代码审查代理
   >
   > description: "当用户要求代码审查时使用"
   >
   > ## 行为规范
   > - 专注于代码质量和安全性
   > - 使用表格输出问题列表
   > ```
   >
   > ### 第四步：创建第一个 Command（可选）
   > ```bash
   > touch ~/.claude/commands/save-and-commit.md
   > ```
   > 内容格式：
   > ```markdown
   > # save-and-commit
   >
   > > 保存并提交
   >
   > argument-hint: "提交信息"
   >
   > ## 执行步骤
   > 1. 保存所有修改
   > 2. git add .
   > 3. git commit -m "$ARG"
   > ```
   >
   > ### 第五步：创建第一个 Skill（可选）
   > ```bash
   > mkdir -p ~/.claude/skills/my-first-skill
   > touch ~/.claude/skills/my-first-skill/SKILL.md
   > ```
   > 内容格式：
   > ```markdown
   > ---
   > name: my-first-skill
   > description: "当用户需要 [具体任务] 时使用"
   > ---
   >
   > # 我的第一个技能
   >
   > ## 执行步骤
   > 1. 步骤一
   > 2. 步骤二
   > ```
   >
   > **推荐优先级**：
   > 1. ✅ `memory.md` - 强烈建议创建（最实用）
   > 2. ⬜ `skills/` - 建议创建（自动化工作流）
   > 3. ⬜ `commands/` - 可选（手动触发）
   > 4. ⬜ `agents/` - 可选（高级用户）

2、学习了问题1的回答后，我想知道，如果我在 20-AI与工具/01-Claude Code/My-projects/01-projects_XXX 文件夹底下该怎么科学合理的创建项目，需要有哪些文件？
   > ✅ 回答：
   >
   > **科学合理的项目结构**：
   >
   > ```
   > 20-AI与工具/
   > └── 01-Claude Code/
   >     └── My-projects/
   >         └── 01-projects_XXX/
   >             ├── CLAUDE.md              # 项目规范（必须）
   >             ├── README.md              # 项目说明
   >             ├── .claude/               # Claude 项目配置
   >             │   ├── memory.md
   >             │   ├── agents/
   >             │   ├── skills/
   >             │   └── commands/
   >             ├── 笔记/
   >             ├── 脚本/
   >             └── 输出/
   > ```
   >
   > **快速创建命令**：
   > ```bash
   > cd "20-AI与工具/01-Claude Code/My-projects/01-projects_XXX"
   > mkdir -p .claude/skills .claude/agents .claude/commands
   > touch CLAUDE.md README.md
   > ```
   >
   > **CLAUDE.md 最小内容**：
   > ```markdown
   > # 项目名称
   > ## 项目信息 - 用途/技术栈
   > ## 注意事项 - 项目特殊注意事项
   > ```



---

*Tags: #claude-code #补充课程 #claude目录 #配置维护*