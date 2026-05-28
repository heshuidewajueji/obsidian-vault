# CLAUDE.md

> 本文件为 Claude Code 项目规范文件
> Claude 启动时会自动读取此文件

---

## 项目信息

- **项目名称**: Newone_Obsidian-Vault
- **类型**: Obsidian 知识库（King的第二大脑）
- **最后更新**: 2026-05-28

---

## 项目结构

```
Newone_Obsidian-Vault/
├── 00-🤖AI共享层/      ← AI共享记忆区（所有AI读/写）
│   ├── MEMORY.md       ← King核心偏好+角色分工
│   ├── WORKING.md      ← 当前进行中任务
│   └── 02-框架与原则/   ← 工程控制论监控框架
│
├── 10-📚Learning/      ← 理论学习笔记
│   ├── SQE职业成长课程/ ← 核心课程
│   ├── Claude-Code学习课程/
│   ├── Hermes-Agent学习课程/
│   ├── OpenClaw学习课程/
│   └── cc-switch学习课程/
│
├── 20-🔧AI与工具/      ← AI工具配置（已迁移至00-🤖AI共享层）
│
├── 30-📂项目管理/      ← 工作中产生的文档、案例、检查表
│   ├── 知识沉淀/        ← 新学到的知识（按主题分类）
│   ├── PPAP审核/
│   ├── VDA审核/
│   ├── 8D案例库/
│   └── 供应商档案/
│
├── 98-模板/            ← 笔记模板
├── 99-归档/            ← 已完成项目归档
└── 02-👩🏻‍🎓个人档案/    ← 个人资料
```

---

## 核心约定

### 文件命名
- 使用中文或英文，避免拼音
- 笔记文件使用 `.md` 格式
- Wiki-link 使用 `[[笔记名]]` 语法
- frontmatter: 所有笔记需包含YAML frontmatter（title/date/tags）

### AI共享层（00-🤖AI共享层）
- MEMORY.md: King核心偏好，所有AI共享读
- WORKING.md: 当前跨天任务状态，所有AI共享读
- 新知识 → 30-📂项目管理/知识沉淀/，不写入MEMORY.md

---

## 用户偏好

| 偏好 | 说明 |
|------|------|
| 回复语言 | 中文为主 |
| 决策风格 | AI做调研，King做关键决定 |
| 输出风格 | 简洁、直接、无废话 |

---

## 注意事项

- 删除/修改前先确认
- 跨天任务写入 WORKING.md
- 不确定时列出疑问点，禁止脑补

---

*Tags: #claude-code #project-spec #vault*