# Lesson 12: 团队协作
> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已学习
> 📌 进阶课程 #6

---

## 一、团队协作模式

### 1.1 单人 vs 团队

| 模式 | 特点 | Claude Code 使用 |
|------|------|------------------|
| **单人** | 独立工作 | 直接对话、完整控制 |
| **团队** | 共享代码、风格一致 | CLAUDE.md 规范、模板复用 |

### 1.2 团队协作核心原则

```
1. 规范先行 → CLAUDE.md 约定
2. 文档共享 → 工具配置、流程文档
3. 代码审查 → Claude Code 代码审查
4. 知识积累 → Obsidian 团队知识库
```

---

## 二、共享 CLAUDE.md 规范

### 2.1 项目级 CLAUDE.md

创建团队共享的规范文件：

```markdown
# CLAUDE.md - 项目规范

## 代码风格
- 使用中文注释
- 函数命名：camelCase
- 组件命名：PascalCase
- 缩进：2空格

## Git 提交规范
- feat: 新功能
- fix: 修复bug
- docs: 文档更新
- style: 格式调整
- refactor: 重构
- test: 测试
- chore: 杂项

## 代码审查要求
- 所有 PR 必须通过 Claude Code 审查
- 审查清单见 CODE_REVIEW.md
```

### 2.2 团队模板库

```bash
# 团队共享模板目录
~/.claude/team-templates/
├── CLAUDE.md.template       # 项目规范模板
├── pr-template.md           # PR 描述模板
├── code-review-checklist.md # 审查清单
└── commit-message-guide.md  # 提交指南
```

### 2.3 复制使用模板

```bash
# 新项目时复制模板
cp ~/.claude/team-templates/CLAUDE.md.template ./CLAUDE.md
```

---

## 三、代码审查工作流

### 3.1 Claude Code 审查流程

```bash
# 1. 启动审查模式
claude
请审查这个 PR：feature/user-auth

# 2. Claude 分析代码
- 检查代码风格
- 检查潜在 bug
- 检查安全漏洞
- 检查性能问题

# 3. 输出审查报告
```

### 3.2 审查提示词模板

```
审查 PR：{PR 标题}
分支：{feature/xxx} → main

审查要点：
1. 代码风格是否符合规范？
2. 是否有潜在 bug？
3. 是否有安全漏洞？
4. 是否有性能问题？
5. 测试是否充分？

输出格式：
- 通过/需要修改
- 问题列表
- 改进建议
```

### 3.3 GitHub Actions 集成

创建自动化审查工作流：

```yaml
# .github/workflows/code-review.yml
name: AI Code Review

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        
      - name: Run Claude Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude "审查这个 PR 的代码变更"
```

---

## 四、知识共享策略

### 4.1 团队 Obsidian 知识库

```
团队 Vault 结构：
team-vault/
├── 01-项目文档/       # 各项目文档
├── 02-技术规范/       # 开发规范
├── 03-常见问题/       # FAQ
├── 04-会议记录/       # 会议纪要
└── 05-代码模板/       # 常用代码模板
```

### 4.2 共享笔记模板

```markdown
# 项目文档模板

## 项目概述
- 项目名称：
- 负责人：
- 目标：

## 技术栈
- 前端：
- 后端：
- 数据库：

## 部署信息
- 测试环境：
- 生产环境：

## 联系人
- 开发：
- 运维：
```

### 4.3 定期知识同步

```bash
# 每周五同步知识
CronCreate cron="0 17 * * 5" \
  prompt="整理本周技术分享到团队知识库" \
  recurring=true
```

---

## 五、交接与文档化

### 5.1 项目交接清单

| 项目 | 内容 | 工具 |
|------|------|------|
| 代码交接 | 完整代码、Git 历史 | Git |
| 文档交接 | 项目文档、API 文档 | Obsidian |
| 配置交接 | 环境配置、密钥 | .env.example |
| 流程交接 | 开发流程、部署流程 | CLAUDE.md |

### 5.2 Claude Code 辅助交接

```bash
# 生成项目总结
claude "生成项目交接文档，包括：
1. 项目架构图
2. 核心模块说明
3. 配置清单
4. 常见问题
5. 联系人列表"
```

### 5.3 交接文档模板

```markdown
# 项目交接文档

## 基本信息
- 项目名称：
- 接手人：
- 交接日期：
- 前任负责人：

## 项目状态
- 当前进度：
- 待解决问题：
- 风险提示：

## 技术架构
[Claude Code 生成的架构图]

## 快速上手
[Claude Code 生成的上手指南]
```

---

## 六、团队效率工具

### 6.1 共享 Aliases

在团队 `.zshrc` 中添加：

```bash
# 团队常用命令
alias pr-review="claude '审查当前 PR'"
alias pr-desc="claude '生成 PR 描述'"
alias code-analyze="claude '分析代码质量'"
alias gen-doc="claude '生成技术文档'"
```

### 6.2 Slack/Teams 集成

```bash
# 安装 Slack MCP
claude mcp add slack npx @modelcontextprotocol/server-slack \
  --token xoxb-xxxxx

# 在 Claude 中直接发消息
claude "在 #dev-team 频道发送：代码审查完成，请查看"
```

### 6.3 团队 MCP 配置

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"]
    },
    "slack": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-slack"]
    }
  }
}
```

---

## 七、团队最佳实践

### 7.1 Git 工作流

```
main (生产)
  └── develop (开发)
        ├── feature/xxx
        ├── feature/yyy
        └── hotfix/zzz
```

### 7.2 PR 规范

| 要求 | 说明 |
|------|------|
| 标题 | feat: 功能描述 |
| 描述 | 使用 PR 模板 |
| 审查 | 至少 1 人 review |
| 测试 | 必须通过 CI |

### 7.3 Code Review 检查项

```
□ 代码风格一致
□ 功能逻辑正确
□ 单元测试覆盖
□ 无安全隐患
□ 性能可接受
□ 文档已更新
□ 无敏感信息泄露
```

---

## 八、冲突解决

### 8.1 Git 冲突处理

```bash
# 让 Claude 帮助解决冲突
claude "解决 Git 冲突，保留两边的修改并确保逻辑正确"

# 冲突解决策略
1. 理解冲突原因
2. 与 CLAUDE.md 规范对照
3. 选择最佳方案
4. 测试验证
```

### 8.2 团队决策流程

```
问题提出 → 团队讨论 → Claude 分析 → 决策执行
```

---

## 💡 今日要点

> 1. **CLAUDE.md 规范**：团队代码一致性基础
> 2. **模板复用**：提高效率，减少重复工作
> 3. **自动化审查**：GitHub Actions + Claude Code
> 4. **知识共享**：团队 Obsidian 知识库
> 5. **交接文档化**：Claude 辅助生成交接文档

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学会，无疑问

---

## 🔗 关联资源

- [[Lesson 11 -多Agent协作]]
- [[Lesson 6+ - CC-Switch使用指南]]
- [GitHub Actions 文档](https://docs.github.com/actions)

---

*Tags: #claude-code #进阶课程 #lesson-12 #团队协作 #代码审查*
