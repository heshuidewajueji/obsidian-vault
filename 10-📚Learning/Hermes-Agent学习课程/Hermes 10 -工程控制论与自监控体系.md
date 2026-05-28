# Hermes 10: 工程控制论与自监控体系

> 📅 学习日期: 2026-05-27
> 📝 学习状态: ✅ 已完成
> 📌 实践课程 #3

---

## 一、钱学森《工程控制论》核心框架

### 1.1 历史背景

钱学森 1954 年创立《工程控制论》，研究**如何用控制论原理指导工程系统设计**。

核心问题：**由不可靠组件构成的系统如何实现可靠运行？**

### 1.2 五层监控架构

| 层级 | 名称 | 功能 | 类比 Hermes |
|------|------|------|-------------|
| L0 | 组件存活层 | 进程是否存活 | health_check.sh 检测 gateway/CLI/MCP 进程 |
| L1 | 反馈信号层 | 输入输出质量、异常率 | api_quality_monitor / tool_quality_monitor |
| L2 | 稳定性分析层 | 振荡检测、级联半径 | compression_monitor / session_health |
| L3 | 自适应控制层 | 漂移检测、降级决策 | cusum_monitor（Provider 降级） |
| L4 | 进化评估层 | 技能新鲜度、知识衰减 | skill_health_check |

### 1.3 四大运行原则（第一层）

```
反馈优先：错即数据，异常即反馈；降级而非崩溃；铁律：不返回原始空输出
分散决策：去中心化；子Agent最大3个并发，禁止递归
结构化容错：可靠性来自架构而非完美组件；上下文>50%自动压缩
持续进化：经验沉淀与反思；技能自动生成
```

### 1.4 反馈回路设计

```
组件执行 ──► 测量输出 ──► 与期望比较 ──► 偏差纠正
                              ▲
                              │
                        反馈信号输入
```

**信噪比保护原则**：正常状态静默，异常才告警。监控系统自身也必须降级。

---

## 二、已部署监控脚本

### 2.1 脚本清单

| 脚本 | 优先级 | 监控内容 |
|------|--------|----------|
| `mcp_health_check.py` | P1 | MCP Server 连接成功率、重连频率、振荡检测 |
| `api_quality_monitor.py` | P1 | 缓存命中率、延迟趋势、Provider 稳定性、CUSUM |
| `tool_quality_monitor.py` | P2 | 工具成功率、错误类型、连续失败振荡检测 |
| `compression_monitor.py` | P2 | 压缩频率、膨胀速度、会话时长分布 |
| `session_health_analyzer.py` | P3 | 会话长度、消息节奏、AI/用户输出比 |
| `combined_health_report.py` | 汇总 | 聚合所有子监控，统一推送飞书 |
| `cusum_monitor.py` | L3 | CUSUM 漂移检测，Provider 降级决策 |
| `skill_health_check.py` | L4 | 技能新鲜度、知识衰减率、架构漂移 |
| `health_check.sh` | L0 | 组件存活检测（ec_l0_wrapper.sh 包装） |

### 2.2 脚本架构

```
combined_health_report.py（汇总器）
├── mcp_health_check.py       → MCP Server 健康
├── api_quality_monitor.py    → Model API 质量
├── tool_quality_monitor.py   → Tool 执行质量
├── compression_monitor.py     → 对话压缩行为
└── session_health_analyzer.py → Session 健康

Cron: ec-combined-health（每30m）──► 飞书推送
Cron: ec-l3-cusum（每120m）──────► 漂移检测 + 降级
Cron: ec-l0-heartbeat（每30m）────► L0 进程存活
Cron: ec-l4-evolution（每天09:00）► 技能进化评估
```

### 2.3 关键监控指标

**MCP Server（P1）**
- `minimax_coding`: 已连接，正常
- `minimax_search`: 持续重连失败，需要关注
- `memory`: 正常运行

**Model API（P1）**
- 缓存命中率正常 baseline
- Provider 延迟：minimax 异常高（29.77s）
- CUSUM 基线需约 18 小时后生效

**Tool 执行（P2）**
- `web_search` / `web_extract` / `search_files` / `memory` 成功率异常低
- 连续失败检测：防止错误循环失控

---

## 三、与 Agent 架构的关联

### 3.1 映射关系

| 控制论概念 | Hermes 实现 | 验证方式 |
|------------|-------------|----------|
| 被控对象 | gateway / CLI / MCP / Provider | health_check.sh |
| 传感器 | 日志输出 + 工具返回值 | agent.log / errors.log |
| 控制器 | Cron 调度 + 告警逻辑 | combined_health_report.py |
| 执行器 | 飞书推送 + Provider 降级 | send_message / config.yaml |
| 反馈回路 | 监控数据 → 分析 → 决策 → 纠正 | CUSUM 漂移检测 |

### 3.2 分散决策在 Hermes 的体现

```
Hermes 主控（规划/决策/质量把控）
    │
    └── OpenClaw 执行方（具体操作）
    └── 子Agent（最大3个并发，禁止递归）

协调层设定边界，执行层自主决策
    └── Cron Job 自主运行，无需人工干预
    └── Provider 降级自动触发
```

### 3.3 结构化容错设计

| 异常类型 | 检测方式 | 恢复策略 |
|----------|----------|----------|
| rate_limit | API 日志检测 | 指数退避 + Provider 切换 |
| context_overflow | token 计数 | 强制压缩或拆分任务 |
| provider_degradation | CUSUM 漂移 | 自动降级到备选 Provider |
| MCP_disconnect | mcp_health_check | 重连 + 告警 |
| tool_failure | 连续N次失败检测 | 振荡隔离 + 标记失控 |

---

## 四、Cron Job 部署状态

| Cron ID | 名称 | 频率 | 模式 | 状态 |
|---------|------|------|------|------|
| `48d058507969` | ec-l0-heartbeat | 每30m | no_agent | 运行中 |
| `259f82835cbc` | ec-combined-health | 每30m | no_agent | 运行中 |
| `ff1553eff065` | ec-l3-cusum | 每120m | glm-4-flash | 运行中 |
| `22b81cf83362` | ec-l4-evolution | 每天09:00 | glm-4-flash | 运行中 |

**推送目标**：飞书（oc_8d0a43e1451303f735f9c1dc696afee7）

---

## 五、今日告警摘要（2026-05-27）

```
🔴 P1 级 (4项)
  · web_search 成功率 0%（3/3次失败）
  · web_extract 成功率 0%（1/1次失败）
  · search_files 成功率 20%（4/5次失败）
  · memory 成功率 33%（2/3次失败）

🟡 P2 级 (6项)
  · minimax Provider 延迟 29.77s
  · read_file 成功率 67%
  · connection 错误出现 5 次
  · deepseek CUSUM 漂移检测 > H阈值

🟢 P3 级 (2项)
  · 1 个会话超过 50 条消息
  · 平均 AI/用户输出比 67.81x（过度分析）
```

---

## 六、文件路径汇总

### 监控脚本
```
~/.hermes/scripts/
├── mcp_health_check.py        # P1 - MCP 健康
├── api_quality_monitor.py     # P1 - API 质量
├── tool_quality_monitor.py    # P2 - 工具质量
├── compression_monitor.py     # P2 - 压缩监控
├── session_health_analyzer.py # P3 - 会话健康
├── combined_health_report.py  # 汇总聚合器
├── cusum_monitor.py           # L3 - CUSUM
├── skill_health_check.py      # L4 - 技能健康
├── health_check.sh            # L0 - 进程存活
└── ec_l0_wrapper.sh           # L0 - Cron 包装器
```

### 日志数据源
```
~/.hermes/logs/
├── gateway.log          # gateway 进程日志
├── health.log           # 组件存活历史
├── mcp-stderr.log       # MCP 服务错误
├── skill_health_report.json  # 技能健康报告
├── cusum_state.json     # CUSUM 基线状态
├── errors.log           # 错误分类统计
└── agent.log            # API 调用日志
```

### Skill 文档
```
~/.hermes/skills/engineering-cybernetics-monitoring/SKILL.md
```

---

## 七、关联资源

- [[Hermes 8 -自我进化系统]]
- [[Hermes 9 -实战应用与最佳实践]]
- [钱学森《工程控制论》1954]

---

## 💡 今日要点

> 1. **五层监控框架**：L0 存活 → L1 反馈 → L2 稳定 → L3 自适应 → L4 进化
> 2. **四大运行原则**：反馈优先 / 分散决策 / 结构化容错 / 持续进化
> 3. **信噪比保护**：正常静默，异常告警；监控系统自身也须降级
> 4. **分散决策**：子Agent 最大 3 个并发，禁止递归，协调层设边界
> 5. **黑箱监控**：只看输入输出行为，不看内部代码实现

---

*Tags: #hermes #工程控制论 #self-monitoring #control-theory #cron*