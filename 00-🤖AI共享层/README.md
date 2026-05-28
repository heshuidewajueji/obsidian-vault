# AI共享记忆层 · README

> 本目录为King的三个AI助手（Hermes/OpenClaw/Claude Code）的**共享记忆区**
> 所有AI启动时首先读取本目录

---

## 目录结构

```
00-🤖AI共享层/
├── MEMORY.md       ← King核心偏好、角色分工、协作约定（所有AI共享）
├── WORKING.md      ← 当前进行中任务、供应商/项目跟踪（所有AI共享）
├── README.md       ← 本文件，AI读取入口
└── 02-框架与原则/  ← 工程控制论监控框架文档（保留）
```

---

## 读取规则（Session Startup时）

所有AI在**每次会话开始**读取：

| AI | 读取文件 | 来源 |
|----|---------|------|
| Hermes | MEMORY.md + WORKING.md | vault/00-🤖AI共享层/ |
| OpenClaw | MEMORY.md + WORKING.md | vault/00-🤖AI共享层/ |
| Claude Code | MEMORY.md + WORKING.md | vault/00-🤖AI共享层/ |

> 注意：Claude Code通过vault根目录CLAUDE.md获知读取规则，不需直接配置

## 写入规则

| AI | 写入位置 | 何时写 |
|----|---------|--------|
| Hermes | MEMORY.md（偏好变更）、WORKING.md（任务状态） | 会话结束时 |
| OpenClaw | 30-📂项目管理/对应目录 | 任务执行完 |
| Claude Code | 30-📂项目管理/知识沉淀/ | 研究完 |

---

## 新知识存放规则

工作中产生的新知识 → `30-📂项目管理/知识沉淀/`
按主题分类：`EPS知识沉淀/`、`ESC知识沉淀/`、`功能安全知识沉淀/`、`VDA审核沉淀/`等

---

## 新知识流程

```
1. King向Hermes提出需求
2. Hermes判断是否需要执行
3. 需要执行 → 派给OpenClaw/Claude Code
4. 执行结果写入 30-📂项目管理/对应目录
5. 关键沉淀同步到 WORKING.md 记录
6. Hermes告知King结果
```

---

## 跨天任务规则

- 任何进行中任务写入 WORKING.md
- 会话开始时Hermes读取WORKING.md，显示进行中任务，询问是否继续
- 会话结束前更新WORKING.md

---

## 联系方式

- Vault路径: `~/Newone_Obsidian-Vault/Newone_Obsidian-Vault/`
- MCP Server: memory-sqlite（已配置）
- 同步GitHub: heshuidewajueji/king-ai-skills

---

*Tags: #ai-shared #readme*