# Lesson 11: 多 Agent 协作

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已学习
> 📌 进阶课程 #5

---

## 一、多 Agent 概念

### 1.1 什么是多 Agent

多 Agent 是指**并行启动多个 Claude 会话**，同时处理不同任务。

```
会话 1 (Haiku)  ──→ 简单问答
会话 2 (Sonnet) ──→ 代码审查
会话 3 (Opus)   ──→ 复杂规划
```

### 1.2 核心优势

| 优势 | 说明 |
|------|------|
| **并行处理** | 同时处理多个任务，效率提升 |
| **专业分工** | 不同会话用不同模型 |
| **隔离环境** | 避免上下文污染 |

---

## 二、Git Worktree 并行工作

### 2.1 什么是 Worktree

Git Worktree 让你在**同一仓库的不同分支**同时工作。

### 2.2 基本命令

```bash
# 查看现有 worktrees
git worktree list

# 创建新的 worktree
git worktree add ../feature-x feature-branch

# 移除 worktree
git worktree remove ../feature-x
```

### 2.3 多分支并行

```bash
# 为每个任务创建独立目录
git worktree add ../task-auth feature/auth
git worktree add ../task-api feature/api
git worktree add ../task-ui feature/ui

# 在每个目录启动独立的 Claude 会话
cd ../task-auth && claude
cd ../task-api && claude
cd ../task-ui && claude
```

### 2.4 命名建议

```bash
# 使用有意义的名称
git worktree add ~/cc-worktrees/analyze-logs analysis-branch
git worktree add ~/cc-worktrees/bigquery-query query-branch
```

---

## 三、Shell 别名快速切换

### 3.1 创建别名

在 `~/.zshrc` 中添加：

```bash
# Claude 会话别名
alias cc1="cd ~/cc-worktrees/task1 && claude"
alias cc2="cd ~/cc-worktrees/task2 && claude"
alias cca="cd ~/cc-worktrees/analyze && claude"
alias ccb="cd ~/cc-worktrees/build && claude"

# 刷新别名
source ~/.zshrc
```

### 3.2 快速启动

```bash
# 在终端1：数据分析
cca

# 在终端2：代码构建
ccb

# 在终端3：简单问答
cc-switch use haiku && claude
```

---

## 四、任务分解与合并

### 4.1 主-从架构

```
主 Agent (Opus)
   ├── 子任务 1 → Agent A (Sonnet)
   ├── 子任务 2 → Agent B (Sonnet)
   └── 子任务 3 → Agent C (Haiku)
       ↓
    合并结果
```

### 4.2 分解示例

```
主会话：
帮我重构整个项目

分解：
1. Agent A：分析项目结构
2. Agent B：制定重构计划
3. Agent C：执行重构

主会话汇总结果
```

### 4.3 实施步骤

```bash
# 1. 主会话分析任务
claude
帮我分析这个项目需要重构的模块

# 2. 并行启动子会话
cd module-auth && claude -r "分析用户认证模块"
cd module-api && claude -r "分析 API 模块"

# 3. 主会话合并结果
claude
汇总所有模块的重构建议
```

---

## 五、分析专用 Worktree

### 5.1 创建分析环境

```bash
# 创建只读的 analysis worktree
git worktree add ../cc-analysis analysis-branch

# 在 analysis 目录配置 Claude
cd ../cc-analysis
# 添加只读配置
echo "# 只用于分析，不修改代码" > CLAUDE.md
```

### 5.2 使用场景

| 场景 | 用途 |
|------|------|
| **日志分析** | 查看日志，排查问题 |
| **BigQuery 查询** | 数据分析查询 |
| **代码审查** | 检查代码质量 |
| **架构分析** | 分析项目结构 |

---

## 六、并行工作最佳实践

### 6.1 分工原则

| 任务类型 | 推荐模型 | 说明 |
|----------|----------|------|
| 简单问答 | Haiku | 快速响应 |
| 代码修改 | Sonnet | 平衡性能 |
| 复杂规划 | Opus | 深度推理 |
| 数据分析 | Sonnet | 分析能力强 |

### 6.2 窗口管理

| 工具 | 用途 |
|------|------|
| **tmux** | 终端多窗口管理 |
| **iTerm2** | Mac 终端分屏 |
| **Warp** | 现代终端 |

### 6.3 任务分配表

| 会话 | 模型 | 任务 | 状态 |
|------|------|------|------|
| 终端1 | Opus | 架构设计 | 运行中 |
| 终端2 | Sonnet | 代码审查 | 运行中 |
| 终端3 | Sonnet | Bug 修复 | 运行中 |
| 终端4 | Haiku | 简单问答 | 空闲 |

---

## 七、高级技巧

### 7.1 Agent 类型选择

| 代理类型 | 说明 | 使用场景 |
|----------|------|----------|
| `Explore` | 快速搜索 | 代码探索 |
| `Plan` | 规划分析 | 设计方案 |
| `general-purpose` | 通用代理 | 复杂任务 |
| `Bash` | 命令执行 | 脚本运行 |

### 7.2 并行与串行选择

| 场景 | 方式 | 原因 |
|------|------|------|
| 多个独立任务 | 并行 | 节省时间 |
| 有依赖的任务 | 串行 | 需要前一步结果 |
| 复杂任务 | 先并行探索，再串行执行 | 效率与质量平衡 |

---

## 💡 今日要点

> 1. **Git Worktree**：创建隔离的工作环境
> 2. **Shell 别名**：快速切换会话
> 3. **任务分解**：并行处理子任务，串行合并
> 4. **模型选择**：按任务复杂度选模型
> 5. **分析专用目录**：只读环境，专注分析

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学会，暂无疑问

---

## 🔗 关联资源

- [[Lesson 12: 团队协作]]
- [Git Worktree 文档](https://git-scm.com/docs/git-worktree)

---

*Tags: #claude-code #进阶课程 #lesson-11 #多agent #并行工作 #worktree*