# Hermes 7: 配置维护

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已完成
> 📌 进阶课程 #1

---

## 一、配置文件夹结构

### 1.1 完整目录树

```
~/.hermes/
├── config.yaml           # 主配置文件
├── memory/               # 记忆存储
│   ├── conversations/    # 对话历史
│   ├── preferences/      # 用户偏好
│   └── knowledge/        # 知识库
├── skills/              # 技能目录
│   ├── built-in/        # 内置技能
│   └── custom/          # 自定义技能
├── logs/                # 日志文件
│   ├── hermes.log       # 主日志
│   └── error.log        # 错误日志
├── credentials/         # 凭证存储（加密）
│   ├── api_keys/        # API 密钥
│   └── tokens/          # OAuth Token
├── data/               # 数据存储
│   ├── cache/          # 缓存
│   └── temp/           # 临时文件
└── backups/           # 备份文件
```

---

## 二、各文件/夹详解

### 2.1 config.yaml - 主配置

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `model.provider` | 模型提供商 | openrouter |
| `model.name` | 模型名称 | claude-3-5-sonnet |
| `gateway.*` | 网关配置 | - |
| `memory.enabled` | 启用记忆 | true |
| `skills.dir` | 技能目录 | ~/.hermes/skills |
| `log.level` | 日志级别 | info |

**手动维护**：编辑此文件可调整配置
```bash
# 查看当前配置
cat ~/.hermes/config.yaml

# 编辑配置
nano ~/.hermes/config.yaml
```

### 2.2 memory/ - 记忆存储

| 子目录 | 存储内容 | 大小 |
|--------|----------|------|
| `conversations/` | 对话历史记录 | 中 |
| `preferences/` | 用户偏好和设置 | 小 |
| `knowledge/` | 学习的知识 | 中-大 |

**手动维护**：
```bash
# 查看记忆大小
du -sh ~/.hermes/memory/*

# 手动导出记忆
cp -r ~/.hermes/memory ~/hermes-memory-backup

# 清理旧记忆
find ~/.hermes/memory -type f -mtime +30 -delete
```

### 2.3 skills/ - 技能目录

| 类型 | 位置 | 说明 |
|------|------|------|
| 内置技能 | `skills/built-in/` | 安装时自带 |
| 自定义技能 | `skills/custom/` | 用户添加 |

**手动维护**：
```bash
# 查看所有技能
ls ~/.hermes/skills/

# 查看内置技能
ls ~/.hermes/skills/built-in/

# 查看自定义技能
ls ~/.hermes/skills/custom/
```

### 2.4 logs/ - 日志文件

| 文件 | 内容 | 重要性 |
|------|------|--------|
| `hermes.log` | 运行时日志 | 中 |
| `error.log` | 错误日志 | 高 |
| `access.log` | 访问日志 | 低 |

**手动维护**：
```bash
# 查看日志大小
du -sh ~/.hermes/logs/*

# 查看最新错误
tail -20 ~/.hermes/logs/error.log

# 清理旧日志（保留7天）
find ~/.hermes/logs/ -name "*.log" -mtime +7 -delete

# 完全清空日志
rm ~/.hermes/logs/*.log
```

### 2.5 credentials/ - 凭证存储

| 子目录 | 存储内容 | 安全性 |
|--------|----------|--------|
| `api_keys/` | API 密钥 | 🔒 高 |
| `tokens/` | OAuth Token | 🔒 高 |

**手动维护**：
```bash
# 查看存储的凭证（仅名称）
ls ~/.hermes/credentials/

# ⚠️ 不要直接编辑或删除此目录
# ⚠️ 凭证以加密形式存储
```

### 2.6 data/ - 数据存储

| 子目录 | 内容 | 可清理？ |
|--------|------|----------|
| `cache/` | 缓存文件 | ✅ 是 |
| `temp/` | 临时文件 | ✅ 是 |

**手动维护**：
```bash
# 清理缓存
rm -rf ~/.hermes/data/cache/*

# 清理临时文件
rm -rf ~/.hermes/data/temp/*

# 完全重置 data 目录
rm -rf ~/.hermes/data/* && mkdir -p ~/.hermes/data/cache ~/.hermes/data/temp
```

---

## 三、日常维护任务

### 3.1 每周维护

```bash
#!/bin/bash
# hermes-weekly-maintenance.sh

echo "=== Hermes 周维护 ==="

# 1. 查看存储使用
echo "存储使用情况："
du -sh ~/.hermes/*

# 2. 清理旧日志
echo "清理 7 天前的日志..."
find ~/.hermes/logs/ -name "*.log" -mtime +7 -delete

# 3. 清理缓存
echo "清理缓存..."
rm -rf ~/.hermes/data/cache/*

# 4. 备份配置
echo "备份配置..."
cp -r ~/.hermes/config.yaml ~/.hermes/backups/config-$(date +%Y%m%d).yaml

echo "=== 维护完成 ==="
```

### 3.2 每月维护

```bash
#!/bin/bash
# hermes-monthly-maintenance.sh

echo "=== Hermes 月维护 ==="

# 1. 清理旧记忆（保留 90 天）
echo "清理 90 天前的记忆..."
find ~/.hermes/memory -type f -mtime +90 -delete

# 2. 清理旧备份（保留 3 个月）
echo "清理旧备份..."
find ~/.hermes/backups -type f -mtime +90 -delete

# 3. 导出完整备份
echo "导出完整备份..."
BACKUP_NAME="hermes-full-backup-$(date +%Y%m%d)"
tar -czf ~/${BACKUP_NAME}.tar.gz ~/.hermes

# 4. 清理临时文件
echo "清理临时文件..."
rm -rf ~/.hermes/data/temp/*

echo "=== 月维护完成 ==="
echo "备份文件：~/${BACKUP_NAME}.tar.gz"
```

### 3.3 设置定时任务

```bash
# 添加每周维护 cron
crontab -e

# 每周日凌晨 2 点执行维护
0 2 * * 0 ~/.hermes/hermes-weekly-maintenance.sh >> ~/.hermes/logs/maintenance.log 2>&1
```

---

## 四、故障排查

### 4.1 记忆丢失

```bash
# 1. 检查记忆目录
ls -la ~/.hermes/memory/

# 2. 从备份恢复
cp -r ~/.hermes/backups/memory-backup-xxx ~/.hermes/memory

# 3. 重建记忆索引
hermes memory rebuild
```

### 4.2 配置损坏

```bash
# 1. 检查配置文件语法
cat ~/.hermes/config.yaml | python3 -c "import sys, yaml; yaml.safe_load(sys.stdin)"

# 2. 恢复默认配置
cp ~/.hermes/backups/config-xxx ~/.hermes/config.yaml

# 3. 或重置为默认
hermes config reset
```

### 4.3 存储满

```bash
# 1. 查看各目录大小
du -sh ~/.hermes/*/* | sort -h

# 2. 找出大文件
find ~/.hermes -type f -size +100M

# 3. 清理不需要的内容
rm -rf ~/.hermes/logs/*.log
rm -rf ~/.hermes/data/cache/*
```

---

## 五、备份与恢复

### 5.1 完整备份

```bash
# 创建备份
BACKUP_DATE=$(date +%Y%m%d)
tar -czf ~/hermes-backup-${BACKUP_DATE}.tar.gz \
  --exclude='~/.hermes/logs' \
  --exclude='~/.hermes/data/cache' \
  --exclude='~/.hermes/data/temp' \
  ~/.hermes

echo "备份完成：~/hermes-backup-${BACKUP_DATE}.tar.gz"
```

### 5.2 仅备份配置和记忆

```bash
# 轻量备份（不包含大文件）
BACKUP_DATE=$(date +%Y%m%d)
tar -czf ~/hermes-light-backup-${BACKUP_DATE}.tar.gz \
  ~/.hermes/config.yaml \
  ~/.hermes/memory/ \
  ~/.hermes/credentials/ \
  ~/.hermes/skills/custom/

echo "轻量备份完成：~/hermes-light-backup-${BACKUP_DATE}.tar.gz"
```

### 5.3 恢复备份

```bash
# 解压备份
tar -xzf ~/hermes-backup-20260522.tar.gz -C ~/

# 重启 Hermes
hermes restart
```

---

## 六、维护命令速查

| 任务 | 命令 |
|------|------|
| 查看大小 | `du -sh ~/.hermes/*` |
| 查看日志 | `tail -f ~/.hermes/logs/hermes.log` |
| 清理日志 | `find ~/.hermes/logs/ -name "*.log" -mtime +7 -delete` |
| 清理缓存 | `rm -rf ~/.hermes/data/cache/*` |
| 备份配置 | `cp ~/.hermes/config.yaml ~/hermes-backup.yaml` |
| 恢复配置 | `cp ~/hermes-backup.yaml ~/.hermes/config.yaml` |
| 重置记忆 | `rm -rf ~/.hermes/memory/* && hermes memory rebuild` |

---

## 💡 今日要点

> 1. **config.yaml**：主配置，可手动编辑
> 2. **memory/**：自动管理的记忆，定期备份即可
> 3. **logs/**：定期清理，防止占用空间
> 4. **credentials/**：加密存储，不要手动编辑
> 5. **skills/custom/**：可自由添加/删除自定义技能

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学习，无问题

---

## 🔗 关联资源

- [[Hermes 2 -安装与配置]]
- [[Hermes 5 -进阶用法]]
- [Hermes Agent GitHub](https://github.com/NousResearch/Hermes-Agent)

---

*Tags: #hermes #进阶课程 #lesson-7 #配置维护 #备份*
