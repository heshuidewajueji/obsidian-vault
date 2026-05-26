# Hermes 5: 进阶用法

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已完成
> 📌 基础课程 #5

---

## 一、Web 面板进阶

### 1.1 自定义端口

```bash
# 指定端口启动
hermes start --port 9090

# 浏览器访问 http://localhost:9090
```

### 1.2 开启认证

```bash
# 设置用户名和密码
hermes config set web_username admin
hermes config set web_password your_password

# 重启后生效
hermes restart
```

### 1.3 启用 HTTPS

```bash
# 配置 SSL 证书
hermes config set ssl_cert /path/to/cert.pem
hermes config set ssl_key /path/to/key.pem

# 重启后生效
hermes restart
```

---

## 二、执行后端进阶

### 2.1 Docker 后端

```bash
# 安装 Docker（如果没有）
brew install --cask docker

# 切换到 Docker 后端
hermes backend use docker

# 验证
hermes backend status
```

### 2.2 SSH 后端

```bash
# 添加 SSH 服务器
hermes backend add-ssh my-server \
  --host user@server.com \
  --port 22

# 切换到 SSH 后端
hermes backend use ssh my-server
```

### 2.3 Ollama 本地模型

```bash
# 安装 Ollama
brew install ollama

# 拉取模型
ollama pull llama2

# 配置 Hermes 使用 Ollama
hermes config set model ollama/llama2
hermes config set base_url http://localhost:11434
```

---

## 三、自定义技能开发

### 3.1 技能结构

```
~/.hermes/skills/my-skill/
├── skill.yaml       # 技能定义
├── main.py         # 技能逻辑
└── README.md       # 使用说明
```

### 3.2 skill.yaml 格式

```yaml
name: my-skill
description: 我的自定义技能
version: 1.0.0
author: Your Name

triggers:
  - "执行 my-skill"
  - "运行我的技能"

parameters:
  - name: input
    type: string
    required: true
    description: 输入参数

outputs:
  - name: result
    type: string
    description: 执行结果
```

### 3.3 main.py 示例

```python
def execute(input: str) -> dict:
    """技能执行函数"""
    # 处理逻辑
    result = f"处理了: {input}"
    
    return {
        "success": True,
        "result": result
    }
```

### 3.4 安装技能

```bash
# 从目录安装
hermes skill install ~/.hermes/skills/my-skill

# 从 GitHub 安装
hermes skill install username/repo

# 查看已安装
hermes skill list
```

---

## 四、高级配置

### 4.1 环境变量

```bash
# 设置环境变量
hermes config set-env VAR_NAME value

# 查看所有环境变量
hermes env list
```

### 4.2 代理配置

```bash
# HTTP 代理
hermes config set proxy http://127.0.0.1:7890

# HTTPS 代理
hermes config set proxy_https http://127.0.0.1:7890
```

### 4.3 日志配置

```bash
# 设置日志级别
hermes config set log_level debug

# 查看日志
tail -f ~/.hermes/logs/hermes.log
```

### 4.4 完整配置示例

```yaml
# ~/.hermes/config.yaml

model:
  provider: openrouter
  model: anthropic/claude-3-5-sonnet
  
gateway:
  telegram:
    enabled: true
    bot_token: xxx
    
memory:
  enabled: true
  storage: ~/.hermes/memory
  
skills:
  directory: ~/.hermes/skills
  auto_update: true
```

---

## 五、API 接口

### 5.1 REST API

Hermes 提供 REST API：

```bash
# 获取状态
curl http://localhost:8080/api/status

# 发送消息
curl -X POST http://localhost:8080/api/message \
  -H "Content-Type: application/json" \
  -d '{"text": "你好"}'

# 查询记忆
curl http://localhost:8080/api/memory
```

### 5.2 WebSocket

```javascript
// 连接 WebSocket
const ws = new WebSocket('ws://localhost:8080/ws');

// 发送消息
ws.send(JSON.stringify({
  type: 'message',
  text: '你好'
}));

// 接收消息
ws.onmessage = (event) => {
  console.log(JSON.parse(event.data));
};
```

---

## 六、自进化功能

### 6.1 自我学习

Hermes 会根据对话自动优化：

```
你：以后叫我老板，不要叫用户
Hermes：好的，老板。

你：你之前叫我什么？
Hermes：老板。
```

### 6.2 技能自动创建

```
你：帮我创建一个每天早上 8 点提醒我的技能
Hermes：[自动创建 morning-reminder 技能]
技能已创建并启用。
```

### 6.3 记忆优化

```bash
# 查看记忆统计
hermes memory stats

# 清理旧记忆
hermes memory clean --older-than 30d

# 导出记忆
hermes memory export --format json
```

---

## 七、备份与恢复

### 7.1 导出配置

```bash
# 导出全部配置
hermes backup export ~/hermes-backup.zip

# 只导出记忆
hermes backup export ~/hermes-memory.zip --memory-only
```

### 7.2 导入配置

```bash
# 导入备份
hermes backup import ~/hermes-backup.zip
```

### 7.3 定时备份

```bash
# 设置每日自动备份
hermes config set auto_backup true
hermes config set backup_path ~/hermes-backups
```

---

## 💡 今日要点

> 1. **Web 面板**：自定义端口、HTTPS、认证
> 2. **执行后端**：Docker、SSH、Ollama
> 3. **自定义技能**：创建和安装技能
> 4. **API 接口**：REST 和 WebSocket
> 5. **自进化**：自动学习和优化

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学习，无问题

---

## 🔗 关联资源

- [[Hermes 4 -消息网关]]
- [[Hermes 6 -与Claude Code协作]]
- [Hermes Agent GitHub](https://github.com/NousResearch/Hermes-Agent)

---

*Tags: #hermes #进阶课程 #lesson-5 #API #技能开发 #自进化*
