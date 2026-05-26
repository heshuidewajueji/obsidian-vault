# Lesson 7: 调试与排错

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已学习
> 📌 进阶课程 #1

---

## 一、常见错误分类

### 1.1 安装问题

| 错误 | 原因 | 解决方案 |
|------|------|----------|
| `command not found: claude` | 安装失败或路径问题 | 重新安装：`npm install -g @anthropic-ai/claude-code` |
| `permission denied` | npm 权限不足 | Mac/Linux：`sudo npm install -g @anthropic-ai/claude-code` |
| `Node.js version too old` | Node 版本过低 | 更新 Node.js：`nvm install 25` |

### 1.2 认证问题

| 错误 | 原因 | 解决方案 |
|------|------|----------|
| `API key invalid` | API Key 错误 | 检查 settings.json 中的 API Key |
| `Connection refused` | 代理未运行 | 检查 cc-switch 是否运行：`lsof -i :15721` |
| `Timeout` | 网络超时 | 检查代理设置或网络连接 |

### 1.3 执行问题

| 错误 | 原因 | 解决方案 |
|------|------|----------|
| `Permission denied` | 命令未授权 | 在 settings.local.json 中添加权限 |
| `File not found` | 路径错误 | 使用相对路径，不要用绝对路径 |
| `Read only` | 文件只读 | 检查文件权限：`ls -la file.md` |

---

## 二、排错工具

### 2.1 检查 Claude 状态

```bash
# 检查是否安装
claude --version

# 检查正在运行的进程
ps aux | grep claude

# 检查端口占用
lsof -i :15721
```

### 2.2 检查 API 连接

```bash
# 测试 API 端点
curl -s http://127.0.0.1:15721/v1/models

# 测试直接 API
curl -s https://ark.cn-beijing.volces.com/api/v3/models \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 2.3 查看日志

```bash
# Claude Code 日志
cat ~/.claude/telemetry/*.log

# cc-switch 日志
cat ~/.cc-switch/logs/*.log

# Hermes 日志
cat ~/.hermes/logs/*.log
```

---

## 三、权限问题解决

### 3.1 临时授权命令

```bash
# 单次授权
+ Bash(npm install *)
claude "执行这个命令"
```

### 3.2 永久授权

编辑 `~/.claude/settings.local.json`：

```json
{
  "permissions": {
    "allow": [
      "Bash(npm install *)",
      "Bash(git commit *)",
      "Bash(git push)"
    ]
  }
}
```

### 3.3 常用权限配置

| 权限 | 用途 |
|------|------|
| `Bash(ls *)` | 列出目录 |
| `Bash(git *)` | Git 操作 |
| `Bash(npm *)` | npm 操作 |
| `WebFetch` | 获取网页 |
| `WebSearch` | 网络搜索 |

---

## 四、网络与代理问题

### 4.1 代理配置检查

```bash
# 检查环境变量
env | grep -i proxy

# 检查 cc-switch 是否运行
lsof -i :15721

# 测试代理连通性
curl -s --connect-timeout 5 http://127.0.0.1:15721
```

### 4.2 常见代理错误

| 错误 | 原因 | 解决 |
|------|------|------|
| `ECONNREFUSED` | 代理未运行 | 启动 cc-switch |
| `Proxy authentication failed` | 代理需要认证 | 检查代理配置 |
| `SSL certificate error` | 证书问题 | 更新 CA 证书或绕过验证 |

---

## 五、文件操作问题

### 5.1 Read 问题

| 问题 | 解决 |
|------|------|
| 文件不存在 | 检查路径，使用 Glob 搜索 |
| 编码问题 | Claude Code 自动处理 UTF-8 |
| 大文件 | 使用 `limit` 和 `offset` 参数分块读取 |

### 5.2 Write/Edit 问题

| 问题 | 解决 |
|------|------|
| Edit 找不到内容 | 检查空格、缩进是否精确匹配 |
| Write 覆盖了文件 | 使用 Edit 代替 Write |
| 权限不足 | `chmod` 修改文件权限 |

---

## 六、调试技巧

### 6.1 分步执行

```bash
# 把复杂任务分解成小步骤
claude "第一步：查看项目结构"
claude "第二步：分析依赖"
claude "第三步：执行构建"
```

### 6.2 使用 --dry-run

```bash
# 只看计划，不执行
claude /plan --dry-run
帮我重构用户模块
```

### 6.3 查看上下文

```bash
# 查看当前上下文大小
claude "查看上下文 token 使用量"

# 压缩上下文
/claude compact
```

---

## 七、常见场景排错

### 场景 1: Claude 不响应

```
排查步骤：
1. 检查是否在运行：ps aux | grep claude
2. 检查 API 连接：curl http://127.0.0.1:15721
3. 重启 cc-switch
4. 检查日志
```

### 场景 2: 命令执行失败

```
排查步骤：
1. 检查权限：是否在 settings.local.json 中授权
2. 检查命令语法：手动在终端执行是否成功
3. 检查路径：是否使用相对路径
```

### 场景 3: Skills 不触发

```
排查步骤：
1. 检查描述：description 是否包含触发关键词
2. 检查位置：是否在 ~/.claude/skills/ 或项目 .claude/skills/
3. 检查格式：SKILL.md 是否包含正确的 frontmatter
```

---

## 八、排错清单

```
□ Claude 是否安装：claude --version
□ API 是否可用：curl http://127.0.0.1:15721
□ 代理是否运行：lsof -i :15721
□ 权限是否足够：settings.local.json
□ 路径是否正确：相对路径
□ 日志是否有错：cat ~/.claude/telemetry/*.log
```

---

## 💡 今日要点

> 1. **先检查基础**：安装、API 连接、代理
> 2. **权限问题最常见**：查看 settings.local.json
> 3. **日志是最好的线索**：查看 telemetry/ 和 logs/
> 4. **分步执行**：复杂问题拆成小步骤排查

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已完成学习，无疑问

---

*Tags: #claude-code #进阶课程 #lesson-7 #调试 #排错*