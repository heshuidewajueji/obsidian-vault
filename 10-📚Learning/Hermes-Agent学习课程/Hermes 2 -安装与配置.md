# Hermes 2: 安装与配置

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已完成
> 📌 基础课程 #2

---

## 一、安装前检查

### 1.1 检查 Python 版本

```bash
python3 --version
# 需要 Python 3.10+
```

### 1.2 检查 Git

```bash
git --version
```

### 1.3 检查网络

```bash
# 测试 GitHub 连接
curl -I https://github.com
```

---

## 二、安装 Hermes Agent

### 2.1 一键安装（推荐）

```bash
# macOS / Linux / WSL2
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

### 2.2 如果网络慢，使用镜像

```bash
# 国内镜像（如果官方地址无法访问）
curl -fsSL https://ghfast.top/https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

### 2.3 验证安装

```bash
# 重启终端后
hermes --version

# 或加载环境变量
source ~/.zshrc
hermes --version
```

---

## 三、初始化配置

### 3.1 运行初始化向导

```bash
hermes setup
```

### 3.2 配置流程

```
┌─────────────────────────────────────────┐
│  Hermes Agent 初始化向导                  │
│  ─────────────────────────────────────  │
│                                         │
│  1. Quick Setup (快速配置)              │
│     └→ 自动检测并安装依赖               │
│                                         │
│  2. Custom Setup (自定义配置)           │
│     └→ 手动选择组件                      │
│                                         │
│  选择 [1/2]: 1                          │
│                                         │
└─────────────────────────────────────────┘
```

### 3.3 选择 LLM 后端

| 后端 | 说明 | 适用场景 |
|------|------|----------|
| **OpenRouter** | 聚合多个模型 | 通用推荐 |
| **OpenAI** | OpenAI API | 有 OpenAI Key |
| **Anthropic** | Claude API | 有 Claude Key |
| **Ollama** | 本地模型 | 隐私敏感 |
| **Groq** | 免费额度 | 预算有限 |

### 3.4 配置 API Key

```bash
# 设置 API Key
hermes config set OPENROUTER_API_KEY sk-xxxxx

# 或使用环境变量
export OPENROUTER_API_KEY=sk-xxxxx
```

---

## 四、启动方式

### 4.1 终端对话模式

```bash
hermes
```

启动后直接对话：
```
你：你好
Hermes：你好！有什么可以帮你的？

你：记住我喜欢喝咖啡
Hermes：好的，我已记住你喜欢喝咖啡。

你：明天提醒我买咖啡豆
Hermes：好的，我会帮你记住这个提醒。
```

### 4.2 Web 面板模式

```bash
# 启动 Web 面板
hermes start

# 浏览器访问
# http://localhost:8080
```

### 4.3 后台运行

```bash
# 使用 nohup 后台运行
nohup hermes start > hermes.log 2>&1 &

# 查看日志
tail -f hermes.log
```

---

## 五、基础配置

### 5.1 配置文件位置

```
~/.hermes/
├── config.yaml          # 主配置
├── memory/              # 记忆存储
├── skills/              # 技能目录
├── logs/                # 日志文件
└── credentials/         # 凭证存储
```

### 5.2 查看当前配置

```bash
hermes config show
```

### 5.3 修改模型

```bash
# 查看可用模型
hermes models list

# 切换模型
hermes config set model anthropic/claude-3-5-sonnet
```

### 5.4 常用配置命令

```bash
# 查看帮助
hermes --help

# 设置代理（可选）
hermes config set proxy http://127.0.0.1:7890

# 设置语言
hermes config set language zh
```

---

## 六、常见问题

### 6.1 安装失败

| 问题 | 解决 |
|------|------|
| `curl: command not found` | 安装 curl：`brew install curl` |
| `git: command not found` | 安装 Git：`brew install git` |
| Python 版本过低 | 安装 Python 3.10+ |

### 6.2 启动失败

| 问题 | 解决 |
|------|------|
| API Key 无效 | 检查 `hermes config show` |
| 端口被占用 | `lsof -i :8080` 查看 |
| 网络超时 | 配置代理或检查网络 |

### 6.3 验证运行

```bash
# 检查进程
ps aux | grep hermes

# 检查端口
lsof -i :8080
```

---

## 💡 今日要点

> 1. **一行命令安装**：`curl -fsSL ... | bash`
> 2. **初始化**：`hermes setup`
> 3. **启动方式**：终端对话 `hermes` 或 Web 面板 `hermes start`
> 4. **配置检查**：`hermes config show`

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、告知我下，hermes文件夹里面的文件分别是起什么作业，我该怎么人工维护？

---

## 🔗 关联资源

- [[Hermes 1 -认识Hermes]]
- [[Hermes 3 -核心功能]]
- [Hermes Agent GitHub](https://github.com/NousResearch/Hermes-Agent)

---

*Tags: #hermes #基础课程 #lesson-2 #安装 #配置*
