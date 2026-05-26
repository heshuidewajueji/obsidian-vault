# Lesson 9: 自动化工作流

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已学习
> 📌 进阶课程 #3

---

## 一、自动化工具概述

Claude Code 提供了多个自动化执行工具：

| 工具 | 用途 | 示例 |
|------|------|------|
| **ScheduleWakeup** | 延迟执行 | 30分钟后提醒 |
| **CronCreate** | 定时任务 | 每天9点执行 |
| **CronDelete** | 删除定时任务 | 取消某个提醒 |
| **/loop** | 循环执行 | 每10分钟检查状态 |

---

## 二、ScheduleWakeup 延迟执行

### 2.1 基本语法

```bash
ScheduleWakeup delaySeconds=1800 prompt="30分钟后提醒我开会"
```

### 2.2 参数说明

| 参数 | 类型 | 说明 |
|------|------|------|
| `delaySeconds` | number | 延迟秒数（60-3600）|
| `prompt` | string | 要执行的提示词 |
| `reason` | string | 简短说明（用于日志）|

### 2.3 使用示例

```bash
# 5分钟后提醒
ScheduleWakeup delaySeconds=300 "提醒我检查邮件" reason="检查新邮件"

# 30分钟后提醒
ScheduleWakeup delaySeconds=1800 "检查今天的任务完成情况" reason="每日任务检查"

# 1小时后执行
ScheduleWakeup delaySeconds=3600 "运行测试并报告结果" reason="测试完成检查"
```

### 2.4 限制

- 延迟范围：60-3600 秒（1分钟-1小时）
- 精确时间使用 CronCreate

---

## 三、CronCreate 定时任务

### 3.1 基本语法

```bash
CronCreate cron="0 9 * * *" prompt="每天早上9点提醒我查看日程" recurring=true
```

### 3.2 Cron 表达式

```
┌───────────── 分钟 (0-59)
│ ┌───────────── 小时 (0-23)
│ │ ┌───────────── 日 (1-31)
│ │ │ ┌───────────── 月 (1-12)
│ │ │ │ ┌───────────── 星期 (0-7)
│ │ │ │ │
* * * * *
```

### 3.3 常用 Cron 表达式

| 表达式 | 含义 | 示例 |
|--------|------|------|
| `0 9 * * *` | 每天9点 | 每日提醒 |
| `*/10 * * * *` | 每10分钟 | 状态检查 |
| `0 9 * * 1-5` | 工作日9点 | 工作日提醒 |
| `30 8 * * *` | 每天8:30 | 早会提醒 |
| `0 */2 * * *` | 每2小时 | 定时检查 |

### 3.4 一次性任务

```bash
CronCreate cron="0 14 25 5 *" prompt="端午节快乐！" recurring=false
# 2026年5月25日14:00执行
```

### 3.5 recurring 参数

| 值 | 说明 |
|-----|------|
| `true` | 循环执行，直到删除 |
| `false` | 一次性执行，执行后自动删除 |

**注意**：循环任务 7 天后自动过期。

---

## 四、CronDelete 删除任务

### 4.1 查看任务 ID

```bash
CronList
```

返回：
```json
{
  "jobs": [
    {
      "id": "abc123",
      "cron": "0 9 * * *",
      "prompt": "每天早上9点提醒"
    }
  ]
}
```

### 4.2 删除任务

```bash
CronDelete id="abc123"
```

---

## 五、/loop 循环执行

### 5.1 基本语法

```bash
/loop 10m /检查状态
```

### 5.2 参数说明

| 参数 | 格式 | 说明 |
|------|------|------|
| 间隔 | 数字+单位 | 10m=10分钟，1h=1小时 |

### 5.3 使用示例

```
# 每5分钟检查部署状态
/loop 5m /检查 ci-deploy

# 每10分钟检查外部API
/loop 10m "检查 API https://api.example.com/health

# 每小时检查服务器状态
/loop 1h "检查服务器 CPU 和内存使用情况"
```

### 5.4 停止循环

```bash
/exit
# 或
Ctrl+C
```

---

## 六、自动化场景

### 场景 1: 定时提醒

```bash
# 每天早上8点提醒
CronCreate cron="0 8 * * *" prompt="早上好！今天的任务是：查看邮件、回复消息、跟进项目" recurring=true

# 下午3点提醒检查进度
CronCreate cron="0 15 * * *" prompt="下午好！检查今天任务完成情况" recurring=true
```

### 场景 2: 定时检查

```bash
# 每15分钟检查 CI/CD 状态
/loop 15m "检查 GitHub Actions 状态"

# 每小时检查服务器健康
/loop 1h "检查服务器状态：CPU、内存、磁盘"
```

### 场景 3: 定时任务

```bash
# 每天凌晨2点备份数据
CronCreate cron="0 2 * * *" prompt="执行数据库备份并上传到 S3" recurring=true

# 每周一早上9点生成周报
CronCreate cron="0 9 * * 1" prompt="生成上周工作报告" recurring=true
```

### 场景 4: 延迟确认

```bash
# 30分钟后确认
ScheduleWakeup delaySeconds=1800 "确认一下部署是否成功？" reason="部署确认"
```

---

## 七、持久化定时任务

### 7.1 durable 参数

```bash
# 持久化任务（重启后保留）
CronCreate cron="0 9 * * *" prompt="早上提醒" recurring=true durable=true
```

### 7.2 持久化存储位置

持久化任务保存在 `~/.claude/scheduled_tasks.json`：

```json
{
  "jobs": [
    {
      "id": "xxx",
      "cron": "0 9 * * *",
      "prompt": "早上提醒",
      "durable": true
    }
  ]
}
```

---

## 八、注意事项

### 8.1 任务调度限制

| 限制 | 说明 |
|------|------|
| 延迟范围 | 60-3600 秒 |
| 循环过期 | 7 天后自动删除 |
| 最小间隔 | 1 分钟 |

### 8.2 最佳实践

```
✅ 简短 prompt：减少 token 消耗
✅ 明确时间：让 Claude 知道何时执行
✅ 定期清理：用 CronDelete 删除不需要的任务
```

---

## 💡 今日要点

> 1. **ScheduleWakeup**：延迟执行（1-60分钟）
> 2. **CronCreate**：定时任务（精确时间）
> 3. **CronDelete**：删除任务
> 4. **/loop**：循环执行（持续监控）
> 5. **durable=true**：持久化任务

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学会，暂无疑问

---

## 🔗 关联资源

- [[Lesson 10: 成本控制]]
- [Cron 表达式生成器](https://crontab.guru)

---

*Tags: #claude-code #进阶课程 #lesson-9 #自动化 #定时任务 #loop*