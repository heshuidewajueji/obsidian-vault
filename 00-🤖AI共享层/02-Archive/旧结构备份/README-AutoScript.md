# Claude Code 自动化任务脚本

> 📅 创建日期: 2026-05-23
> 🏷️ 用途: Hermes 调用 Claude Code 的自动化脚本

---

## 使用方法

```bash
# 基本用法
python3 ~/scripts/claude_auto.py "任务描述"

# 指定模型
python3 ~/scripts/claude_auto.py "任务描述" sonnet

# 指定超时（秒）
python3 ~/scripts/claude_auto.py "任务描述" sonnet 600
```

## 返回格式

```json
{
  "success": true,
  "output": "Claude Code 输出内容",
  "task_id": "20260523_170000",
  "task_file": "/tmp/claude-tasks/task_20260523_170000.md"
}
```

## Hermes 集成

在 Hermes 中配置自定义技能调用此脚本。

---

*Tags: #automation #claude-code #script*
