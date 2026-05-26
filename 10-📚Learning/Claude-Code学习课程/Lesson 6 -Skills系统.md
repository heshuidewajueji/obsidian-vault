# Lesson 6: Skills 系统

> 📅 学习日期: 2026-05-21
> 📝 学习状态: ✅ 已学习（2026-05-22）

---

## 一、什么是 Skills

Skills 是 Claude Code 的**可复用技能系统**，让你把重复的工作流封装成可复用的技能。

### 与 Commands 的区别

| 对比 | Skills | Commands |
|------|--------|----------|
| **存储位置** | `.claude/skills/` | `.claude/commands/` |
| **触发方式** | `/skill-name` 或自动触发 | `/command-name` 手动调用 |
| **自动调用** | ✅ 可配置自动触发 | ❌ 必须手动 |
| **上下文** | 可与主对话共享 | 独立上下文 |

---

## 二、Skills 目录结构

### 全局 Skills

```
~/.claude/skills/
├── code-review/
│   └── SKILL.md
├── my-custom-skill/
│   └── SKILL.md
└── ...
```

### 项目级 Skills

```
项目/
├── .claude/
│   └── skills/
│       ├── project-specific/
│       │   └── SKILL.md
│       └── ...
```

---

## 三、SKILL.md 文件格式

### 基本结构

```markdown
---
name: skill-name-with-hyphens
description: 技能的描述，Claude 会根据这个判断何时使用
---

# 技能名称

## 描述
详细说明这个技能的作用。

## 触发条件
description 中已说明，当用户提到相关关键词时会触发。

## 执行步骤
1. 第一步操作
2. 第二步操作
3. 第三步操作

## 输出格式
- 问题列表
- 改进建议
```

### Frontmatter 配置

```yaml
---
name: lowercase-name-with-hyphens    # 必须：小写+连字符
description: 描述触发条件和用途      # 必须：自动触发的依据
user-invocable: true                # 可选：是否可用 /skill-name 调用
disable-model-invocation: false    # 可选：是否允许 AI 自动触发
context: shared                      # 可选：shared/fork
---
```

---

## 四、使用示例

### 示例 1: 代码审查 Skill

```
~/.claude/skills/code-review/SKILL.md
```

```markdown
---
name: code-review
description: 当用户要求代码审查、审查代码、检查代码质量时使用
---

# 代码审查技能

## 执行步骤
1. 读取目标文件
2. 检查代码规范：
   - 命名规范
   - 注释完整性
   - 代码结构
3. 查找潜在问题：
   - 空指针风险
   - 安全漏洞
   - 性能问题
4. 输出审查报告

## 输出格式
### 审查结果

| 严重程度 | 问题 | 位置 | 建议 |
|----------|------|------|------|
| 🔴 高 | 问题描述 | 文件:行号 | 建议修复方案 |
| 🟡 中 | 问题描述 | 文件:行号 | 建议修复方案 |
| 🟢 低 | 问题描述 | 文件:行号 | 建议修复方案 |
```

### 示例 2: API 文档生成 Skill

```
~/.claude/skills/api-doc/SKILL.md
```

```markdown
---
name: api-documentation
description: 当用户要求生成 API 文档、生成文档、为函数生成文档时使用
---

# API 文档生成技能

## 执行步骤
1. 读取目标代码文件
2. 分析函数签名和注释
3. 提取参数、返回值、示例
4. 生成 Markdown 格式文档

## 输出格式
```markdown
## 函数名

### 描述
[函数功能说明]

### 参数
| 参数名 | 类型 | 必需 | 说明 |
|--------|------|------|------|

### 返回值
[返回值说明]

### 示例
\`\`\`javascript
[使用示例]
\`\`\`
```
```

---

## 五、创建 Skills 的方法

### 方法 1: 手动创建（不推荐新手）

```bash
mkdir -p ~/.claude/skills/my-skill
touch ~/.claude/skills/my-skill/SKILL.md
# 编辑 SKILL.md 文件
```

### 方法 2: 让 Claude 生成（推荐）

```
帮我创建一个代码审查的 Skill
要求：检查代码规范、安全问题、性能问题
```

Claude 会帮你生成完整的 SKILL.md 文件。

### 方法 3: 从示例复制修改

```
参考 ~/.claude/skills/code-review/SKILL.md
帮我创建一个类似的 Skill，但专注于 [你的需求]
```

---

## 六、Skills 资源库

### 官方资源

- [skillsmp.com](https://skillsmp.com) - 8万+ Skills
- [mcpservers.org/claude-skills](https://mcpservers.org/claude-skills) - 即插即用

### 推荐 Skills

| Skill | 来源 | 用途 |
|-------|------|------|
| code-review | 官方 | 代码审查 |
| simplify | 官方 | 代码优化 |
| batch | 官方 | 批量处理 |
| loop | 官方 | 定时任务 |

---

## 七、实战技巧

### 技巧 1: 渐进式创建

```
第一步：创建最简单的 Skill
第二步：在使用中发现不足
第三步：不断完善 Skill 描述
第四步：添加更多执行步骤
```

### 技巧 2: 命名规范

- 使用小写字母和连字符
- 名称要描述性且简洁
- 避免与其他 Skill 重名

### 技巧 3: description 写法

**好的 description**：
```
"当用户要求代码审查、检查代码质量时使用"
```

**不好的 description**：
```
"帮我看看代码有没有问题"
```

---

## 八、常见问题

| 问题 | 解决 |
|------|------|
| Skill 不触发 | 检查 description 是否包含关键词 |
| 多个 Skill 冲突 | 用更具体的 description 区分 |
| Skill 太复杂 | 拆分成多个简单 Skill |
| Skill 结果不对 | 优化执行步骤描述 |

---

## 💡 今日要点

> 1. **Skills = 封装好的工作流**，可自动触发
> 2. **name + description** = Skill 的核心配置
> 3. **从简单开始**，逐步完善
> 4. **描述越清晰**，触发越准确

---

## 📝 个人笔记区

（在这里写下你的理解和问题）

---

## 🔗 关联资源

- [[Lesson 7: MCP 配置]]
- [[Lesson 5: 项目管理]]
- [Claude Code 官方文档 - Skills](https://docs.anthropic.com/claude-code/skills)

---

*Tags: #claude-code #lesson-6 #skills #可复用技能 #自动化*