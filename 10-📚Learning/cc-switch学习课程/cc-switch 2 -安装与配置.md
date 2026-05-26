# cc-switch 2: 安装与配置

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #2

---

## 一、安装方式

### 1.1 官方安装

```bash
# 访问 GitHub 查看最新安装方式
# https://github.com/farion1231/cc-switch

# 克隆仓库
git clone https://github.com/farion1231/cc-switch.git
cd cc-switch

# 安装依赖
npm install

# 启动应用
npm start
```

### 1.2 macOS 安装

```bash
# 使用 Homebrew（如果有）
brew install --cask cc-switch

# 或下载 DMG 文件
# https://github.com/farion1231/cc-switch/releases
```

### 1.3 验证安装

```bash
# 检查进程
ps aux | grep cc-switch

# 检查配置目录
ls -la ~/.cc-switch/
```

---

## 二、基础配置

### 2.1 配置文件

```
~/.cc-switch/
├── settings.json       # 全局设置
├── cc-switch.db       # 工具配置数据库
├── providers.db       # Provider 配置
├── skills/           # Skills 目录
└── backups/         # 备份目录
```

### 2.2 settings.json 详解

```json
{
  "showInTray": true,              // 显示系统托盘图标
  "minimizeToTrayOnClose": true,   // 关闭时最小化到托盘
  "useAppWindowControls": false,   // 使用 App 窗口控件
  "enableClaudePluginIntegration": true,  // Claude 插件集成
  "skipClaudeOnboarding": true,    // 跳过 Claude 引导
  "launchOnStartup": true,          // 开机启动
  "silentStartup": false,          // 静默启动
  "enableLocalProxy": true,        // 启用本地代理
  "proxyConfirmed": true,          // 代理已确认
  "usageConfirmed": true,          // 使用统计已确认
  "streamCheckConfirmed": true,    // 流检查已确认
  "enableFailoverToggle": true,     // 启用故障转移
  "failoverConfirmed": true,        // 故障转移已确认
  "firstRunNoticeConfirmed": true, // 首运行通知已确认
  "commonConfigConfirmed": true,    // 通用配置已确认
  "language": "zh",                // 语言
  "visibleApps": {                 // 可见应用
    "claude": true,
    "claude-desktop": true,
    "codex": true,
    "gemini": true,
    "opencode": true,
    "openclaw": true,
    "hermes": true
  },
  "currentProviderClaude": "default",
  "currentProviderClaudeDesktop": "default",
  "skillSyncMethod": "symlink",    // Skills 同步方式
  "skillStorageLocation": "cc_switch"
}
```

### 2.3 常用配置项

| 配置项 | 默认值 | 说明 |
|--------|--------|------|
| `showInTray` | true | 显示托盘图标 |
| `launchOnStartup` | true | 开机启动 |
| `language` | zh | 语言 (zh/en) |
| `skillSyncMethod` | symlink | symlink / copy |
| `enableFailoverToggle` | true | 故障转移开关 |

---

## 三、Provider 配置

### 3.1 Provider 数据库

```
~/.cc-switch/providers.db
```

用于存储各工具的 API Provider 配置：

| Provider | 支持类型 |
|-----------|----------|
| Claude | default, claude-3-5-sonnet |
| Claude Desktop | default |
| Gemini | default |
| OpenAI | default |

### 3.2 Provider 切换

```bash
# 切换 Provider（如果支持）
cc-switch set-provider claude <provider-name>

# 查看当前 Provider
cat ~/.cc-switch/settings.json | grep currentProvider
```

### 3.3 API Key 配置

```bash
# Claude API Key
export ANTHROPIC_API_KEY=sk-ant-xxx

# OpenAI API Key
export OPENAI_API_KEY=sk-xxx

# Gemini API Key
export GEMINI_API_KEY=xxx
```

---

## 四、Skills 配置

### 4.1 Skills 目录

```
~/.cc-switch/skills/
├── hermes-self-evolution/    # Hermes 自进化
├── hermes-jury-system/       # 评审系统
├── automotive-8d-report/    # 汽车 8D
├── obsidian-bases/          # Obsidian 基础
├── cloudflare/              # Cloudflare
├── pdf/                     # PDF 处理
└── ... (66+ Skills)
```

### 4.2 同步方式配置

```json
// symlink 模式
{
  "skillSyncMethod": "symlink",
  "skillStorageLocation": "cc_switch"
}

// copy 模式
{
  "skillSyncMethod": "copy",
  "skillStorageLocation": "cc_switch"
}
```

### 4.3 Skills 备份

```
~/.cc-switch/skill-backups/
```

---

## 五、数据库配置

### 5.1 cc-switch.db

工具配置数据库：

```bash
# 查看数据库
sqlite3 ~/.cc-switch/cc-switch.db ".tables"

# 查看配置
sqlite3 ~/.cc-switch/cc-switch.db "SELECT * FROM tools;"
```

### 5.2 数据库结构

```sql
-- 可能的表结构
CREATE TABLE tools (
  id INTEGER PRIMARY KEY,
  name TEXT,
  config_path TEXT,
  enabled BOOLEAN
);

CREATE TABLE app_settings (
  key TEXT PRIMARY KEY,
  value TEXT
);
```

### 5.3 数据库备份

```bash
# 备份数据库
cp ~/.cc-switch/cc-switch.db ~/.cc-switch/backups/cc-switch_$(date +%Y%m%d).db

# 备份 Providers
cp ~/.cc-switch/providers.db ~/.cc-switch/backups/providers_$(date +%Y%m%d).db
```

---

## 六、界面配置

### 6.1 系统托盘

```json
// settings.json
{
  "showInTray": true,
  "minimizeToTrayOnClose": true
}
```

托盘功能：
- 快速切换工具
- 查看状态
- 打开设置

### 6.2 开机启动

```json
{
  "launchOnStartup": true
}
```

### 6.3 语言设置

```json
{
  "language": "zh"  // zh / en
}
```

---

## 七、代理配置

### 7.1 本地代理

```json
{
  "enableLocalProxy": true,
  "proxyConfirmed": true
}
```

### 7.2 故障转移

```json
{
  "enableFailoverToggle": true,
  "failoverConfirmed": true
}
```

---

## 八、当前环境状态

### 8.1 目录权限

```
~/.cc-switch/           # 755
├── settings.json       # 644
├── cc-switch.db       # 600
├── providers.db       # 600
├── skills/           # Skills 目录
├── backups/          # 备份目录
├── logs/             # 日志
└── skill-backups/   # Skills 备份
```

### 8.2 数据库大小

```
cc-switch.db: ~1.3 MB
providers.db: 0 bytes
```

---

## 💡 今日要点

> 1. **安装方式**：Git 克隆 + npm install 或下载 DMG
> 2. **配置命令**：编辑 `~/.cc-switch/settings.json`
> 3. **Provider 管理**：`providers.db` 统一管理 API Keys
> 4. **Skills 同步**：`symlink` 或 `copy` 模式
> 5. **启动方式**：`npm start` 或 GUI 应用

---

## 📝 个人笔记区

---

## 🔗 关联资源

- [[cc-switch 1 -认识cc-switch]]
- [[cc-switch 3 -核心功能]]
- [[00-cc-switch课程目录]]

---

*Tags: #cc-switch #安装 #配置 #lesson-2*