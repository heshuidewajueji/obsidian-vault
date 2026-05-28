# 场景课: Obsidian 笔记管理
> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已学习
> 📌 场景化课程 #1

---

## 一、Obsidian + Claude Code 工作流

### 1.1 核心优势

```
Claude Code                      Obsidian
┌──────────────┐                 ┌──────────────┐
│  AI 智能分析  │ ←────────────→  │   知识存储    │
│  批量处理     │                 │   双向链接   │
│  结构化整理   │                 │   笔记网络   │
└──────────────┘                 └──────────────┘
```

| 能力 | Claude Code | Obsidian |
|------|-------------|----------|
| 内容生成 | ✅ AI 写作 | ❌ 纯编辑器 |
| 知识连接 | ⚠️ 有限 | ✅ 双向链接 |
| 批量操作 | ✅ 强大 | ⚠️ 插件支持 |
| 可视化 | ❌ 无 | ✅ 图形视图 |
| 搜索 | ✅ 全文+语义 | ✅ 全文搜索 |

### 1.2 最佳工作流

```
日常笔记                          知识整理
    │                                │
    ▼                                ▼
Obsidian 快速记录              Claude Code 批量处理
    │                                │
    ▼                                ▼
Claude Code 分析/总结          Obsidian 链接/可视化
```

---

## 二、MCP 连接配置

### 2.1 安装 Obsidian MCP

```bash
# 1. 安装 MCP 服务器
npm install -g @modelcontextprotocol/server-obsidian

# 2. 在 Claude Code 中添加
claude mcp add obsidian npx @modelcontextprotocol/server-obsidian \
  --vault-path ~/Newone_Obsidian-Vault/Newone_Obsidian-Vault

# 3. 验证配置
claude mcp list
```

### 2.2 MCP 可用工具

```bash
# 查看可用资源
ListMcpResourcesTool

# 读取笔记
ReadMcpResourceTool
  - server: "obsidian"
  - uri: "obsidian://path/to/note.md"
```

### 2.3 MCP 提示词

```
Claude，我可以：
- "读取你的 Obsidian 笔记"
- "搜索笔记中的相关内容"
- "帮你整理和重命名笔记"
- "生成笔记之间的链接"
```

---

## 二Claudian 插件：内置 AI 对话

### 2.4 什么是 Claudian

Claudian 是 Obsidian 的 **Claude Code 内置插件**，让你在 Obsidian 侧边栏直接与 Claude 对话。

```
┌─────────────────────────────────────────────────┐
│  Obsidian                                       │
│  ┌─────────┬──────────────────┬──────────────┐  │
│  │ 文件列表 │    笔记编辑区      │  Claude 对话 │  │
│  │         │                  │   (Claudian) │  │
│  │         │                  │              │  │
│  │         │                  │  ┌─────────┐ │  │
│  │         │                  │  │ 对话窗口 │ │  │
│  │         │                  │  └─────────┘ │  │
│  └─────────┴──────────────────┴──────────────┘  │
└─────────────────────────────────────────────────┘
```

### 2.5 Claudian vs MCP

| 对比项 | Claudian | MCP |
|--------|----------|-----|
| **交互方式** | 侧边栏聊天 | 命令行对话 |
| **使用场景** | 即时问答、笔记讨论 | 批量处理、自动化 |
| **与笔记交互** | 直接引用当前笔记 | 通过工具读写文件 |
| **适合人群** | 笔记时实时使用 | 深度任务处理 |

### 2.6 Claudian 安装

#### 方法一：BRAT 安装（推荐）

```bash
# 1. 在 Obsidian 中安装 BRAT 插件
# 设置 → 社区插件 → 搜索 "BRAT" → 安装 → 启用

# 2. 添加 Claudian 测试版
# 设置 → BRAT → Add Beta plugin
# 输入：farion/Claudian

# 3. 等待安装完成，启用 Claudian
```

#### 方法二：手动安装

```bash
# 1. 从 GitHub 下载
# https://github.com/farion1231/Claudian/releases

# 2. 下载三个文件：
# - main.js
# - manifest.json
# - styles.css

# 3. 在 Vault 中创建目录
mkdir -p .obsidian/plugins/claudian

# 4. 将三个文件放入 claudian 目录

# 5. 在 Obsidian 中启用
# 设置 → 第三方插件 → 找到 Claudian → 启用
```

### 2.7 Claudian 配置

#### 配置 API 连接

Claudian 支持两种连接方式：

**方式 1：使用 cc-switch（推荐）**

```
设置 → Claudian → 配置：
- API Endpoint: http://127.0.0.1:15721
- API Key: 任意值（cc-switch 不验证）
- Model: claude-sonnet-4-20250514
```

**方式 2：直接配置 API**

```
设置 → Claudian → 配置：
- API Endpoint: https://api.minimaxi.com/v1
- API Key: YOUR_MINIMAX_API_KEY
- Model: minimax/M2.7-200K
```

#### 配置 Claude CLI 路径（可选）

```bash
# Mac 路径
/Users/king/.nvm/versions/node/v24.13.1/bin/claude

# 或通过 npm 全局安装路径
which claude
# /Users/king/.nvm/versions/node/v24.13.1/bin/claude
```

### 2.8 Claudian 使用方法

#### 打开 Claudian

```
方法 1：点击左下角机器人图标 🤖
方法 2：快捷键 Ctrl/Cmd + Shift + C
```

#### 引用笔记内容

```
在 Claudian 对话框中：
@笔记名  → 引用整篇笔记
@笔记名#标题 → 引用特定章节
@笔记名::标签 → 引用特定块
```

#### 常用操作

| 操作 | 命令 | 说明 |
|------|------|------|
| 总结笔记 | `@当前笔记 总结这篇笔记的核心观点` | AI 总结当前打开的笔记 |
| 翻译 | `@笔记 翻译成英文` | 保留双链格式翻译 |
| 润色 | `@笔记 优化这段文字的表达` | 改进写作质量 |
| 提问 | `@笔记 关于这个概念有什么问题？` | 基于笔记内容提问 |
| 扩展 | `@笔记 扩展这个观点` | AI 补充详细内容 |

### 2.9 Claudian + Obsidian Skills

Claudian 支持使用 Obsidian 内置的 Skills：

```bash
# 1. 在 Vault 中创建 .claude/skills 目录
mkdir -p .claude/skills

# 2. 下载 obsidian-skills
# https://github.com/anthropics/claude-code-skills

# 3. 复制 skills 内容到 .claude/skills/

# 4. Claudian 会自动识别并可用
```

### 2.10 Claudian 与 cc-switch 配合

```
┌─────────────────────────────────────────────────┐
│                                                 │
│   cc-switch (后台运行 :15721)                    │
│       │                                         │
│       ├──→ Claude Code CLI                      │
│       │                                         │
│       └──→ Claudian 插件 (Obsidian 侧边栏)       │
│                                                 │
└─────────────────────────────────────────────────┘
```

**优势**：
- cc-switch 只需配置一次
- Claude Code 和 Claudian 共享同一 API
- 统一管理模型切换和费用

### 2.11 使用场景

#### 场景 1：边写边问

```
1. 打开笔记「学习方法」
2. 点击 Claudian 机器人
3. 问：「这个方法适合职场学习吗？」
4. Claude 基于笔记内容回答
5. 点击「插入」将回答写入笔记
```

#### 场景 2：笔记问答

```
1. 问：「我的知识库里关于 AI 的笔记有哪些？」
2. Claudian 搜索并列出相关笔记
3. 点击直接跳转到对应笔记
```

#### 场景 3：写作助手

```
1. 选中笔记中的一段文字
2. Claudian 对话框自动引用
3. 说：「润色这段话」
4. AI 优化后点击「替换」
```

---

## 三、笔记创建与整理

### 3.1 快速创建笔记

```bash
# 通过 Claude Code 创建笔记
claude "在 Obsidian 中创建笔记 '今日学习计划'，
内容包含：
- 上午任务
- 下午任务
- 晚上复盘"
```

### 3.2 批量重命名

```bash
# 整理混乱的笔记名
claude "将 '新建笔记' '新建笔记 2' 'untitled' 
重命名为有意义的名称"
```

### 3.3 移动笔记到正确位置

```bash
# 按内容分类
claude "分析以下笔记，将它们移动到合适的文件夹：
- [[购房笔记]]
- [[买车考虑]]
- [[投资记录]]
建议分类：财务/房产、财务/车辆、财务/投资"
```

---

## 四、知识库构建

### 4.1 主题笔记创建

```bash
# 创建主题笔记
claude "创建一个主题笔记 '知识管理方法论'，
包含以下内容：
1. 什么是知识管理
2. PKM 工具对比
3. 我的知识管理流程
4. 推荐资源"
```

### 4.2 MOC (Map of Content)

```bash
# 创建主题索引
claude "为 '学习方法' 创建一个 MOC 笔记，
包含所有相关笔记的链接"
```

### 4.3 笔记模板

```bash
# 创建标准模板
claude "创建以下模板：
1. daily-note-template.md
2. project-note-template.md
3. book-note-template.md
4. meeting-note-template.md"
```

---

## 五、双向链接管理

### 5.1 自动生成链接

```bash
# 根据内容生成链接
claude "分析 '学习方法' 笔记，找出与它相关的笔记，
自动添加 [[链接]]"
```

### 5.2 链接修复

```bash
# 修复断开的链接
claude "检查整个 Vault 中的断链，
并尝试修复或删除"
```

### 5.3 反向链接整理

```bash
# 查看并整理引用
claude "查看 '效率工具' 笔记的所有反向链接，
删除不相关的引用"
```

---

## 六、搜索与发现

### 6.1 内容搜索

```bash
# 搜索关键词
claude "在 Vault 中搜索包含 'AI' 的笔记"

# 语义搜索
claude "找出与 '第二大脑' 概念相关的笔记"
```

### 6.2 批量查找

```bash
# 找出一类笔记
claude "找出所有 '课程笔记'，统计数量"

# 查找重复内容
claude "找出内容相似的笔记，可能需要合并"
```

### 6.3 标签管理

```bash
# 统一标签格式
claude "检查并统一所有笔记的标签格式，
规范：统一使用小写，加连字符"

# 清理未使用标签
claude "找出所有未使用的标签"
```

---

## 七、定期整理工作流

### 7.1 每日整理

```bash
# CronCreate 每日提醒
CronCreate cron="20 21 * * *" \
  prompt="整理今天的笔记：
1. 将 Inbox 中的笔记分类
2. 为新笔记添加标签
3. 检查是否有断链
4. 总结今天学习" \
  recurring=true durable=true
```

### 7.2 每周回顾

```bash
# 每周总结
CronCreate cron="0 10 * * 5" \
  prompt="执行本周笔记回顾：
1. 统计本周新建笔记数
2. 找出热点笔记（访问最多）
3. 整理未完成的笔记
4. 生成周报" \
  recurring=true durable=true
```

### 7.3 月度整理

```bash
# 月度大整理
CronCreate cron="0 9 1 * *" \
  prompt="执行月度笔记整理：
1. 清理旧笔记
2. 更新 MOC 索引
3. 归档已完成项目笔记
4. 备份 Vault" \
  recurring=true durable=true
```

---

## 八、高级技巧

### 8.1 笔记合并

```bash
# 合并相关笔记
claude "将 'React 入门' 'React 进阶' 'React 实战'
合并为一个完整的 React 学习笔记"
```

### 8.2 内容提取

```bash
# 从网页提取内容并保存
claude "抓取这个网页 https://example.com/article，
保存为 Obsidian 笔记，使用 frontmatter 添加元数据"
```

### 8.3 批量添加 frontmatter

```bash
# 为笔记批量添加元数据
claude "为 2024 年 1-3 月的所有日记笔记
添加 frontmatter：
- date: 创建日期
- tags: [daily, journal]"
```

### 8.4 导出与转换

```bash
# 导出为指定格式
claude "将 '项目笔记' 文件夹导出为 PDF，
并生成目录"
```

---

## 九、Obsidian 插件推荐

### 9.1 与 Claude Code 配合

| 插件 | 用途 | Claude Code 协作 |
|------|------|-----------------|
| **Templater** | 高级模板 | 生成模板内容 |
| **Dataview** | 数据查询 | 分析笔记数据 |
| **QuickAdd** | 快速添加 | 触发 Claude 创建 |
| **Surfing** | 内置浏览器 | Claude WebFetch |

### 9.2 Surfing + Claude

```
Surfing 打开网页 → 选中文本 → Claude 分析 → 保存笔记
```

---

## 十、实际场景示例

### 场景 1: 课程学习笔记

```bash
# 学习新知识时
claude "帮我整理这个页面的内容为 Obsidian 笔记：
https://modelcontextprotocol.org
保存为 'MCP 学习笔记'，包含：
- 什么是 MCP
- 核心概念
- 使用场景
- 代码示例"
```

### 场景 2: 读书笔记

```bash
# 读完一本书后
claude "创建 '原子习惯' 读书笔记，包含：
- 书籍信息
- 核心观点（4个）
- 实践行动清单
- 与我其他习惯笔记的链接"
```

### 场景 3: 项目管理

```bash
# 项目启动时
claude "在 Obsidian 中创建项目笔记 '新项目X'，
包含：
- 项目概述
- 目标与里程碑
- 资源链接
- 会议纪要模板
- 任务清单"
```

### 场景 4: 会议记录

```bash
# 会议结束后
claude "将以下会议内容整理为 Obsidian 笔记：
[粘贴会议内容]
使用 meeting-note-template 模板，
提取：决策、行动项、负责人"
```

---

## 💡 今日要点

> 1. **Claudian 插件**：Obsidian 内置 AI 对话，侧边栏即时使用
> 2. **MCP 配置**：命令行深度集成，批量处理能力
> 3. **双剑合璧**：Claudian 负责即时问答，MCP 负责批量操作
> 4. **快速创建**：Claude 生成，Obsidian 存储
> 5. **定期自动化**：Cron 任务驱动整理
> 6. **vin-prompts 命令**：25 个 AI 分析命令，把笔记变成"第二大脑"

---

## 附录：vin-prompts 命令速查

### 每日工作流
| 命令 | 功能 |
|------|------|
| `/obsidian-day-today` | 今日计划 |
| `/obsidian-day-close` | 复盘收尾 |
| `/obsidian-day-7plan` | 本周规划 |
| `/obsidian-day-weekly` | 周总结 |

### 知识挖掘
| 命令 | 功能 |
|------|------|
| `/obsidian-mine-emerge` | 挖掘隐藏结论 |
| `/obsidian-mine-make` | 识别可发布内容 |

### 深度分析
| 命令 | 功能 |
|------|------|
| `/obsidian-think-challenge` | 压力测试信念 |
| `/obsidian-think-contradict` | 发现观点矛盾 |

### 内容输出
| 命令 | 功能 |
|------|------|
| `/obsidian-write-learned` | 生成文章 |
| `/obsidian-write-interview` | 生成面试题 |

> 完整命令列表见 [[Lesson 6 -Skills系统]] 第九、十章

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学习，无问题

---

## 🔗 关联资源

- [[Lesson 8 -MCP配置与使用]]
- [[Lesson 9 -自动化工作流]]
- [Obsidian 官网](https://obsidian.md)
- [MCP Obsidian Server](https://github.com/modelcontextprotocol/servers/tree/main/src/obsidian)
- [Claudian 插件 GitHub](https://github.com/farion1231/Claudian)
- [cc-switch](https://github.com/farion1231/cc-switch)

---

*Tags: #claude-code #场景课 #obsidian #笔记管理 #知识管理 #MCP #Claudian #vin-prompts*
