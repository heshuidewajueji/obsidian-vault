# Lesson 99+: CC-Switch 使用指南

> 📅 学习日期: 2026-05-21
> 📝 学习状态: ✅ 已学习（2026-05-22）
> 📌 关联: [[Lesson 1: 认识 Claude Code]]

---

## 一、CC-Switch 是什么

CC-Switch 是一个 **Claude Code 模型切换工具**，让你的 Claude Code 可以灵活使用不同的 AI 模型。

### 核心功能

| 功能 | 说明 |
|------|------|
| **模型切换** | 在不同模型间切换（Haiku/Sonnet/Opus）|
| **代理转发** | 将请求转发到不同的 API 端点 |
| **提示词管理** | 预设系统级提示词规则 |
| **多配置管理** | 切换不同的 API 配置 |

---

## 二、你的当前配置

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "PROXY_MANAGED",
    "ANTHROPIC_BASE_URL": "http://127.0.0.1:15721",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "claude-haiku-4-5",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "claude-sonnet-4-6",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "claude-opus-4-7",
    "ANTHROPIC_DEFAULT_MODEL_NAME": "MiniMax-M2.7"
  },
  "name": "volcengine",
  "provider": "volcengine"
}
```

### 配置说明

| 配置项 | 说明 |
|--------|------|
| `baseURL` | API 代理地址（cc-switch 监听 :15721）|
| `haiku` | 轻量快速模型（适合简单任务）|
| `sonnet` | 平衡模型（日常使用）|
| `opus` | 最强模型（复杂任务）|

---

## 三、基础命令

### 切换模型

```bash
# 查看可用模型
cc-switch models

# 切换到指定模型
cc-switch use haiku    # 切换到 Haiku
cc-switch use sonnet  # 切换到 Sonnet
cc-switch use opus    # 切换到 Opus
```

### 查看状态

```bash
# 查看当前配置
cc-switch status

# 查看所有可用配置
cc-switch list
```

### 管理配置

```bash
# 添加新配置
cc-switch add my-config --api-key YOUR_KEY --base-url YOUR_URL

# 切换到其他配置
cc-switch config my-config

# 删除配置
cc-switch remove my-config
```

---

## 四、提示词管理

### 设置全局提示词

```bash
# 设置默认提示词
cc-switch prompt set default "# 使用中文交流\n# 代码使用中文注释"

# 设置代码审查提示词
cc-switch prompt set code-review "# 专注于安全性\n# 发现问题请列出具体行号"
```

### 查看/删除提示词

```bash
# 查看所有提示词
cc-switch prompt list

# 删除某个提示词
cc-switch prompt remove code-review
```

### 提示词配置示例

在 `settings.json` 中配置：

```json
{
  "prompts": {
    "default": "# 使用中文回复\n# 重要操作前确认",
    "code-review": "# 专注于性能和安全性\n# 使用表格输出问题",
    "debug": "# 专注于分析错误原因\n# 提供可能的解决方案"
  }
}
```

---

## 五、使用场景

### 场景 1: 快速问答（Haiku）

```bash
cc-switch use haiku
claude "帮我解释什么是 API"
```

### 场景 2: 代码审查（Sonnet）

```bash
cc-switch use sonnet
claude "审查 src/ 目录下的代码"
```

### 场景 3: 复杂重构（Opus）

```bash
cc-switch use opus
claude /plan
帮我重构这个项目到 TypeScript
```

### 场景 4: 调试问题（指定提示词）

```bash
cc-switch prompt set debug "# 分析错误原因\n# 提供修复方案"
cc-switch use sonnet
claude "我遇到这个错误：[粘贴错误]"
```

---

## 六、推荐工作流

### 日常使用推荐

| 任务类型 | 推荐模型 | 说明 |
|----------|----------|------|
| 简单问答 | Haiku | 快速、便宜 |
| 文件修改 | Sonnet | 平衡性能 |
| 复杂规划 | Opus | 最强推理 |
| 调试排错 | Sonnet + debug 提示词 | 针对性强 |

### 自动化切换

可以将切换命令设置别名：

```bash
# 在 ~/.zshrc 中添加别名
alias cc-fast="cc-switch use haiku && claude"
alias cc-work="cc-switch use sonnet && claude"
alias cc-hard="cc-switch use opus && claude"
```

---

## 七、常见问题

| 问题 | 解决 |
|------|------|
| cc-switch 无法连接 | 检查是否在运行：`lsof -i :15721` |
| 切换模型无效 | 确认使用 `cc-switch use xxx` |
| 提示词不生效 | 检查配置文件格式 |
| Token 消耗过快 | 多用 Haiku 处理简单任务 |

---

## 八、检查 CC-Switch 状态

```bash
# 检查端口是否被占用
lsof -i :15721

# 应该看到 cc-switch 进程
# cc-switch  56850  ...  TCP  localhost:15721 (LISTEN)
```

---

## 💡 今日要点

> 1. **Haiku/Sonnet/Opus** - 按任务复杂度选模型
> 2. **`cc-switch use xxx`** - 快速切换模型
> 3. **提示词预设** - 提前配置常用规则
> 4. **别名简化** - 用 `alias` 简化常用命令

---

## 📝 个人笔记区（老师已解答）

1、能指导我下 cc-switch 目录文件的各自作用吗，我该怎么人工维护，`~/.cc-switch`？
   > ✅ 回答：
   >
   > **你的 ~/.cc-switch 目录结构**：
   >
   > ```
   > ~/.cc-switch/
   > ├── settings.json          # 配置文件（GUI 设置）
   > ├── cc-switch.db           # 主数据库
   > ├── providers.db           # Provider 数据库
   > ├── skills/               # Skills 库（重要！）
   > ├── backups/              # 备份目录
   > ├── logs/                 # 日志目录
   > └── copilot_auth.json     # 认证信息
   > ```
   >
   > **文件详解**：
   >
   > | 文件/目录 | 作用 | 是否需要手动维护 |
   > |----------|------|------------------|
   > | `settings.json` | GUI 设置、语言、启动项等 | ❌ 不建议手动改 |
   > | `cc-switch.db` | 主数据库，存储配置和状态 | ❌ 不要动 |
   > | `providers.db` | Provider 配置（API 端点等）| ❌ 不要动 |
   > | `skills/` | **Skills 库**，你安装的技能 | ✅ 可以管理 |
   > | `backups/` | 配置备份 | ⬜ 自动生成 |
   > | `logs/` | 运行日志 | ⬜ 查看用 |
   > | `copilot_auth.json` | 认证信息 | ❌ 不要动 |
   >
   > **你的 skills 目录内容**（已安装的 Skills）：
   > ```
   > skills/
   > ├── architecture-health-check/
   > ├── brainstorming/
   > ├── composition-patterns/
   > ├── doc-coauthoring/
   > ├── dogfood/
   > ├── frontend-design/
   > ├── full-test-coverage/
   > ├── hermes-jury-system/
   > ├── hermes-self-evolution/
   > ├── hermes-shared-learnings/
   > ├── hermes-task-state/
   > ├── hermes-to-openclaw/
   > └── ...
   > ```
   >
   > **settings.json 关键配置**：
   > ```json
   > {
   >   "language": "zh",                    // 中文界面
   >   "showInTray": true,                 // 显示在托盘
   >   "launchOnStartup": true,             // 开机启动
   >   "enableClaudePluginIntegration": true, // Claude 插件集成
   >   "skillSyncMethod": "symlink",        // Skills 同步方式
   >   "visibleApps": {                    // 显示哪些应用
   >     "claude": true,
   >     "claude-desktop": true,
   >     "hermes": true,
   >     "openclaw": true
   >   }
   > }
   > ```
   >
   > **人工维护建议**：
   >
   > | 操作 | 方法 |
   > |------|------|
   > | **查看 Skills** | `ls ~/.cc-switch/skills/` |
   > | **添加 Skills** | cc-switch GUI 中添加，或复制到 skills/ |
   > | **删除 Skills** | 删除 skills/ 下对应文件夹 |
   > | **备份配置** | `cp settings.json backups/` |
   > | **查看日志** | `cat ~/.cc-switch/logs/*.log` |
   >
   > **⚠️ 注意事项**：
   > - ❌ 不要手动编辑 `.db` 文件（数据库）
   > - ❌ 不要删除 `copilot_auth.json`
   > - ✅ 可以管理 `skills/` 目录
   > - ✅ 可以备份 `settings.json`
   > - ⬜ `logs/` 用于排查问题，可查看

2、cc-switch 目录底下 skills，我能让 ai-agent 的 skills 文件夹软连接这吗，是否代表 ai-agent 就能直接使用 cc-switch 底下的全部 skill 了？或者正确的用法该是怎么样的，因为我实际操作了 cc-switch 的 skill 管理发现可以点亮 skill 在不同的 ai-agent 图标，这是什么形式的让 ai-agent 支持安装使用？
   > ✅ 回答：
   >
   > **cc-switch Skills 管理机制**：
   >
   > cc-switch 实际上是一个** Skills 超市**，它本身不运行 AI，而是为多个 AI 工具提供 Skills 支持。
   >
   > **你看到的效果**：
   > - cc-switch GUI 中"点亮" skill 在不同 AI 工具图标
   > - 这表示**为该 AI 工具启用/同步**这个 Skill
   >
   > **工作原理**：
   > ```
   > cc-switch skills/      # Skills 仓库（中央存储）
   >      ↓
   > symlink / 复制到
   >      ↓
   > ~/.claude/skills/      # Claude Code
   > ~/.hermes/skills/       # Hermes
   > ~/.openclaw/skills/     # OpenClaw
   > ```
   >
   > **你的 settings.json 配置**：
   > ```json
   > {
   >   "skillSyncMethod": "symlink",  // 使用软链接同步
   >   "skillStorageLocation": "cc_switch"
   > }
   > ```
   > 这说明 cc-switch 使用**软链接**方式同步 Skills。
   >
   > **创建软链接的方法**：
   > ```bash
   > # 为 Claude Code 创建软链接
   > ln -sf ~/.cc-switch/skills ~/.claude/skills
   >
   > # 为 Hermes 创建软链接（如果需要）
   > ln -sf ~/.cc-switch/skills ~/.hermes/skills
   > ```
   >
   > **⚠️ 注意**：你的 `~/.claude/skills/` 目前**不存在**（未创建），所以软链接未建立。
   >
   > **推荐做法**：
   > ```
   > 方法 1：使用 cc-switch GUI
   >        在 GUI 中"点亮" Claude 图标，cc-switch 会自动创建软链接
   >
   > 方法 2：手动创建软链接
   >        ln -sf ~/.cc-switch/skills ~/.claude/skills
   > ```
   >
   > **效果**：
   > - ✅ 创建软链接后，Claude Code 可以使用 cc-switch 中的所有 Skills
   > - ⚠️ 但注意：不同 AI 工具的 Skills 格式可能略有不同，不是所有 Skill 都兼容
   >
   > **各 AI 工具 Skills 位置**：
   >
   > | AI 工具 | Skills 目录 | 你当前状态 |
   > |---------|-----------|----------|
   > | Claude Code | `~/.claude/skills/` | ❌ 未创建 |
   > | Hermes | `~/.hermes/skills/` | ✅ 已存在 |
   > | OpenClaw | `~/.openclaw/skills/` | ❌ 未创建 |
   > | cc-switch | `~/.cc-switch/skills/` | ✅ 41 个 Skills |

---

## 🔗 关联资源

- [[Lesson 1: 认识 Claude Code]]
- [CC-Switch 项目地址](https://github.com/your-repo/cc-switch)

---

*Tags: #claude-code #cc-switch #补充课程 #模型切换 #提示词管理*