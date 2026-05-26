# cc-switch 6: 实战应用

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #6

---

## 一、完整配置示例

### 1.1 场景：配置 Claude Code + Hermes

```bash
# 1. 确保 cc-switch 安装
# npm install 或下载 DMG

# 2. 配置 settings.json
cat > ~/.cc-switch/settings.json << 'EOF'
{
  "showInTray": true,
  "minimizeToTrayOnClose": true,
  "launchOnStartup": true,
  "enableLocalProxy": true,
  "enableFailoverToggle": true,
  "language": "zh",
  "visibleApps": {
    "claude": true,
    "hermes": true,
    "openclaw": true
  },
  "currentProviderClaude": "default",
  "skillSyncMethod": "symlink",
  "skillStorageLocation": "cc_switch"
}
EOF

# 3. 验证配置
cat ~/.cc-switch/settings.json | jq '.visibleApps'
```

### 1.2 场景：启用 Hermes Self-Evolution

```bash
# 1. 确保 hermes-self-evolution Skill 存在
ls ~/.cc-switch/skills/hermes-self-evolution/

# 2. 同步到 Hermes
ls -la ~/.king-ai/skills/hermes-self-evolution

# 3. 在 Hermes 中使用
# 对话中提及：复盘、总结、反思、学习
```

### 1.3 场景：配置 MCP Memory Server

```bash
# 1. 配置 Claude Code MCP
cat > ~/.claude/mcp.json << 'EOF'
{
  "mcpServers": {
    "memory-sqlite": {
      "command": "node",
      "args": ["/Users/king-macbookair/mcp-memory-sqlite/dist/index.js"],
      "env": {
        "SQLITE_DB_PATH": "/Users/king-macbookair/mcp-memory-sqlite/sqlite-memory.db"
      }
    }
  }
}
EOF

# 2. 配置 Hermes MCP
# 在 ~/.hermes/config.yaml 中添加 mcp_servers

# 3. 验证连接
cc-switch health
```

---

## 二、Skills 实战

### 2.1 场景：使用 8D 报告 Skill

```bash
# 1. 查看 Skill
cat ~/.cc-switch/skills/automotive-8d-report/SKILL.md

# 2. 在 Hermes 中调用
# "请使用 8D 报告格式处理这个问题：产品尺寸超差"

# 3. 自动生成 8D 报告
```

### 2.2 场景：使用 Obsidian Skill

```bash
# 1. 查看 Skill
cat ~/.cc-switch/skills/obsidian-cli/SKILL.md

# 2. 在任何支持 Skill 的工具中使用
# "扫描我的 Obsidian 金库，找出孤立文件"

# 3. vault-directory-deep-scan
cat ~/.cc-switch/skills/vault-directory-deep-scan/SKILL.md
```

### 2.3 场景：使用文档处理 Skills

```bash
# PDF 处理
cat ~/.cc-switch/skills/pdf/SKILL.md
# "提取这个 PDF 的主要内容"

# Word 文档
cat ~/.cc-switch/skills/docx/SKILL.md
# "将内容转换为 Word 格式"

# Excel
cat ~/.cc-switch/skills/xlsx/SKILL.md
# "分析这个 Excel 文件的数据"
```

---

## 三、故障排查

### 3.1 问题：Skills 不加载

```bash
# 检查 1：Skills 目录存在
ls -la ~/.cc-switch/skills/

# 检查 2：符号链接正确
ls -la ~/.king-ai/skills/ | head -5

# 检查 3：Skill 文件完整
ls ~/.cc-switch/skills/hermes-self-evolution/SKILL.md

# 解决：重新同步
rm -rf ~/.king-ai/skills/
ln -s ~/.cc-switch/skills/ ~/.king-ai/skills/
```

### 3.2 问题：Provider 切换失败

```bash
# 检查 1：Provider 数据库
sqlite3 ~/.cc-switch/providers.db ".tables"

# 检查 2：配置正确
cat ~/.cc-switch/settings.json | grep currentProvider

# 检查 3：API Key 有效
export ANTHROPIC_API_KEY=sk-ant-xxx
curl -s https://api.anthropic.com/v1/models \
  -H "x-api-key: $ANTHROPIC_API_KEY"
```

### 3.3 问题：托盘图标不显示

```bash
# 检查 1：托盘配置
cat ~/.cc-switch/settings.json | grep showInTray

# 检查 2：进程运行
ps aux | grep cc-switch

# 解决：重启应用
killall cc-switch
open cc-switch
```

---

## 四、最佳实践

### 4.1 配置管理

```bash
# 1. 定期备份
cron:
0 2 * * * cp ~/.cc-switch/settings.json ~/.cc-switch/backups/settings_$(date +\%Y\%m\%d).json

# 2. 版本控制
cd ~/.cc-switch
git add settings.json
git commit -m "update settings"
git push

# 3. 记录变更
echo "2026-05-24: 启用 openclaw visibleApps" >> CHANGELOG.md
```

### 4.2 Skills 管理

```bash
# 1. 定期更新
cd ~/.cc-switch/skills
git pull

# 2. 分类组织
# 按用途分类使用 Skills

# 3. 清理不用的
ls ~/.cc-switch/skills/
# 删除不需要的 Skill 目录
```

### 4.3 性能优化

```bash
# 1. 减少 visibleApps
# 只启用需要的工具

# 2. 定期清理日志
rm ~/.cc-switch/logs/*.log.old

# 3. 数据库维护
sqlite3 ~/.cc-switch/cc-switch.db "VACUUM;"
```

---

## 五、自动化脚本

### 5.1 备份脚本

```bash
#!/bin/bash
# backup_cc_switch.sh

DATE=$(date +%Y%m%d)
BACKUP_DIR="$HOME/cc-switch-backups/$DATE"

mkdir -p "$BACKUP_DIR"

# 备份配置
cp ~/.cc-switch/settings.json "$BACKUP_DIR/"

# 备份数据库
cp ~/.cc-switch/cc-switch.db "$BACKUP_DIR/"
cp ~/.cc-switch/providers.db "$BACKUP_DIR/"

# 备份 Skills
cp -r ~/.cc-switch/skills "$BACKUP_DIR/skills"

# 压缩
cd "$HOME/cc-switch-backups"
tar -czf "cc-switch-$DATE.tar.gz" "$DATE"
rm -rf "$DATE"

echo "备份完成: cc-switch-$DATE.tar.gz"
```

### 5.2 恢复脚本

```bash
#!/bin/bash
# restore_cc_switch.sh

BACKUP_FILE=$1

if [ -z "$BACKUP_FILE" ]; then
  echo "用法: $0 <备份文件>"
  exit 1
fi

# 解压
tar -xzf "$BACKUP_FILE"

# 恢复配置
BACKUP_DIR=$(basename "$BACKUP_FILE" .tar.gz)
cp "$BACKUP_DIR/settings.json" ~/.cc-switch/
cp "$BACKUP_DIR/cc-switch.db" ~/.cc-switch/
cp "$BACKUP_DIR/providers.db" ~/.cc-switch/

# 恢复 Skills
rm -rf ~/.cc-switch/skills
cp -r "$BACKUP_DIR/skills" ~/.cc-switch/

rm -rf "$BACKUP_DIR"
echo "恢复完成"
```

### 5.3 健康检查脚本

```bash
#!/bin/bash
# health_check_cc_switch.sh

echo "=== cc-switch 健康检查 ==="

# 1. 检查配置文件
if [ -f ~/.cc-switch/settings.json ]; then
  echo "✅ settings.json 存在"
else
  echo "❌ settings.json 缺失"
fi

# 2. 检查数据库
if [ -f ~/.cc-switch/cc-switch.db ]; then
  echo "✅ cc-switch.db 存在"
  SIZE=$(du -h ~/.cc-switch/cc-switch.db | cut -f1)
  echo "   大小: $SIZE"
else
  echo "❌ cc-switch.db 缺失"
fi

# 3. 检查 Skills
SKILL_COUNT=$(ls ~/.cc-switch/skills/ -1 | wc -l | tr -d ' ')
echo "✅ Skills 数量: $SKILL_COUNT"

# 4. 检查托盘
TRAY=$(cat ~/.cc-switch/settings.json | jq '.showInTray')
echo "   托盘图标: $TRAY"

# 5. 检查开机启动
LAUNCH=$(cat ~/.cc-switch/settings.json | jq '.launchOnStartup')
echo "   开机启动: $LAUNCH"

echo ""
echo "=== 检查完成 ==="
```

---

## 六、命令速查

| 场景 | 命令 |
|------|------|
| 启动应用 | `cc-switch` 或 `open cc-switch` |
| 列出工具 | `cc-switch list-apps` |
| 切换工具 | `cc-switch switch <tool>` |
| 同步 Skills | `git -C ~/.cc-switch/skills pull` |
| 备份配置 | `cp ~/.cc-switch/*.json backups/` |
| 健康检查 | 运行 `health_check_cc_switch.sh` |

---

## 💡 今日要点

> 1. **完整配置**：settings.json + Provider + Skills
> 2. **Skills 实战**：8D 报告、Obsidian、文档处理
> 3. **故障排查**：Skills 不加载、Provider 失败、托盘不显示
> 4. **最佳实践**：备份、版本控制、定期清理
> 5. **自动化脚本**：备份、恢复、健康检查

---

## 📝 个人笔记区

---

## 🔗 关联资源

- [[cc-switch 5 -高级功能]]
- [[cc-switch 7 -课程毕业总结]]
- [[cc-switch 1 -认识cc-switch]]
- [[Hermes 9 -实战应用与最佳实践]]
- [[00-cc-switch课程目录]]

---

*Tags: #cc-switch #实战 #问题排查 #最佳实践 #lesson-6*