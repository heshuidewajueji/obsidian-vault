# AI 工具共享记忆层

> 📅 创建日期: 2026-05-23
> 🔄 最后更新: 2026-05-23
> 🏷️ 用途: Claude Code 与 Hermes 共享记忆

---

## 目录结构

```
00-AI-Shared/
├── 01-Preferences/      # 共享偏好（L1 当前活跃）
├── 02-Projects/        # 项目上下文（L3）
├── 03-Context/         # 当前会话上下文（L1）
├── 04-Tasks/           # 任务队列
└── 02-Archive/         # 归档记忆（L2）
```

## 层级说明

| 层级 | 目录 | 保留时间 | 说明 |
|------|------|----------|------|
| L1 | 01-Preferences, 03-Context | 持续更新 | 当前活跃记忆 |
| L2 | 02-Archive | 90 天 | 归档记忆 |
| L3 | 02-Projects | 按项目 | 项目隔离记忆 |

## 维护规则

1. **自动清理**：每周日凌晨 Cron 任务清理
2. **归档策略**：超过 30 天的记忆自动归档
3. **MCP Server**：未来将作为主存储，此目录作为可视化备份

## 关联工具

- Claude Code: 通过 MCP Client 读写
- Hermes: 通过 native-mcp Skill 读写

---

*Tags: #ai-shared #记忆层 #mcp*
