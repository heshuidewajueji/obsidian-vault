# Lesson 13: 企业级应用
> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已学习
> 📌 进阶课程 #7

---

## 一、企业级架构

### 1.1 大型项目组织

```
enterprise-project/
├── apps/                    # 应用模块
│   ├── web/                 # 前端应用
│   ├── api/                 # 后端 API
│   └── admin/               # 管理后台
├── packages/                # 共享包
│   ├── ui/                  # UI 组件库
│   ├── utils/               # 工具函数
│   └── types/                # 类型定义
├── services/                # 微服务
│   ├── auth/                # 认证服务
│   ├── payment/             # 支付服务
│   └── notification/         # 通知服务
├── infra/                   # 基础设施
│   ├── terraform/           # 基础设施代码
│   ├── docker/              # 容器配置
│   └── k8s/                 # Kubernetes
└── docs/                    # 文档
```

### 1.2 Claude Code 多模块管理

```bash
# 进入特定模块
cd apps/api

# 分析整个 monorepo
cd enterprise-project
claude "分析项目架构，识别模块依赖关系"

# 跨模块修改
claude "在 api 和 admin 模块同时添加日志中间件"
```

### 1.3 企业级需求

| 需求 | 说明 | Claude Code 支持 |
|------|------|-----------------|
| 安全性 | 代码审计、敏感信息检测 | ✅ 内置 |
| 合规性 | 代码规范、许可证检查 | ✅ 可配置 |
| 性能 | 大型代码库处理 | ✅ 优化 |
| 协作 | 多团队、多仓库 | ✅ 分支管理 |

---

## 二、安全与合规

### 2.1 敏感信息检测

```bash
# Claude Code 自动检测
claude "检查代码中是否有：
- API 密钥
- 密码硬编码
- 私钥泄露
- 个人信息"

# .gitignore 规范
.env
*.pem
*.key
credentials.json
secrets.yaml
```

### 2.2 安全审查提示词

```
执行安全审查：
1. 硬编码凭证检查
2. SQL 注入风险
3. XSS 漏洞检查
4. 权限提升风险
5. 敏感数据暴露

输出格式：
- 高风险问题（必须修复）
- 中风险问题（建议修复）
- 低风险问题（可选优化）
```

### 2.3 合规检查清单

| 检查项 | 说明 | 工具 |
|--------|------|------|
| 许可证合规 | 检查第三方库许可证 | license-checker |
| 代码质量 | SonarQube 集成 | SonarQube |
| 依赖安全 | 检查漏洞依赖 | npm audit / Snyk |
| 代码覆盖率 | 测试覆盖要求 | Jest coverage |

### 2.4 CI/CD 安全集成

```yaml
# .github/workflows/security.yml
name: Security Scan

on: [push, pull_request]

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run Security Scan
        run: |
          claude "执行安全审查，输出报告"
      
      - name: Check Dependencies
        run: npm audit --audit-level=high
```

---

## 三、大型企业代码库

### 3.1 代码库规模分类

| 规模 | 文件数 | Claude Code 策略 |
|------|--------|-----------------|
| 小型 | < 100 | 直接读取全部 |
| 中型 | 100-1000 | 按模块读取 |
| 大型 | 1000-10000 | 使用 Agent 探索 |
| 超大型 | > 10000 | 分模块 + 多 Agent |

### 3.2 Claude Code 性能优化

```bash
# 使用 Explore Agent 探索大型代码库
claude -t explore
"探索 api 模块的结构"

# 使用 Plan Agent 规划大型重构
claude -t plan
"制定微服务拆分计划"

# 分批处理大任务
claude "先分析 auth 模块"
claude "再分析 payment 模块"
```

### 3.3 代码库索引

```bash
# 创建代码库索引文件
# .claude/code-index.md

## 模块索引
- apps/web: 前端应用
- apps/api: 后端 API
- services/auth: 认证服务

## 核心文件
- src/main.ts: 入口文件
- config/: 配置文件
```

### 3.4 模块化 CLAUDE.md

```markdown
# CLAUDE.md - 企业项目规范

## 项目结构
[模块索引]

## 开发规范
[规范列表]

## 特定模块规范
### API 模块
- RESTful 规范
- 错误码规范

### 前端模块
- React 规范
- 状态管理规范

### 数据库模块
- 迁移规范
- 备份策略
```

---

## 四、微服务架构支持

### 4.1 微服务项目结构

```
microservices/
├── service-a/              # 服务 A
│   ├── src/
│   ├── test/
│   ├── Dockerfile
│   └── CLAUDE.md
├── service-b/              # 服务 B
│   ├── src/
│   ├── test/
│   ├── Dockerfile
│   └── CLAUDE.md
├── service-c/              # 服务 C
└── infrastructure/         # 基础设施
```

### 4.2 独立 CLAUDE.md

每个微服务都有自己的 CLAUDE.md：

```markdown
# service-a/CLAUDE.md

## 服务职责
用户认证和授权

## 技术栈
- Node.js 18+
- Express
- PostgreSQL
- Redis

## API 端点
- POST /auth/login
- POST /auth/register
- POST /auth/refresh

## 环境变量
JWT_SECRET=
DB_URL=
REDIS_URL=
```

### 4.3 跨服务修改

```bash
# 修改单个服务
cd service-a && claude "添加 OAuth 支持"

# 修改多个服务
claude "在 service-a 和 service-b 添加统一日志格式"

# 批量更新依赖
claude "检查所有服务的依赖版本"
```

---

## 五、DevOps 集成

### 5.1 Docker 集成

```bash
# Claude Code 帮助编写 Dockerfile
claude "为这个 Node.js 应用编写 Dockerfile"

# 多阶段构建示例
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine AS runtime
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY . .
CMD ["node", "src/index.js"]
```

### 5.2 Kubernetes 配置

```bash
# 生成 K8s 配置
claude "为这个服务生成 K8s 部署配置"

# 输出：
# - deployment.yaml
# - service.yaml
# - ingress.yaml
# - configmap.yaml
```

### 5.3 Terraform 基础设施

```bash
# 生成 Terraform 配置
claude "生成 AWS ECS 部署的 Terraform 配置"

# 模块化 Terraform
claude "创建可复用的 VPC 模块"
```

---

## 六、监控与日志

### 6.1 日志规范

```bash
# Claude Code 帮助建立日志规范
claude "为微服务项目制定日志规范"
```

### 6.2 日志格式

```json
{
  "timestamp": "2026-05-22T10:30:00Z",
  "level": "info",
  "service": "auth-service",
  "trace_id": "abc123",
  "message": "User logged in",
  "user_id": "user_001",
  "metadata": {
    "ip": "192.168.1.1",
    "user_agent": "Mozilla/5.0..."
  }
}
```

### 6.3 告警规则

```yaml
# Prometheus 告警规则
groups:
  - name: auth-service
    rules:
      - alert: HighErrorRate
        expr: rate(http_errors_total[5m]) > 0.05
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "高错误率"
          description: "错误率超过 5%"
```

---

## 七、企业级提示词模板

### 7.1 新功能开发

```
任务：开发 {功能名称}

上下文：
- 项目：{项目名称}
- 技术栈：{技术栈列表}
- 相关模块：{相关模块}

要求：
1. 遵循现有代码风格
2. 包含单元测试
3. 更新相关文档
4. 通过代码审查

约束：
- 不引入新的外部依赖
- 保持向后兼容
```

### 7.2 代码重构

```
任务：重构 {模块/文件}

目标：
- 提高可读性
- 提升性能
- 降低复杂度

约束：
- 保持现有功能不变
- 不改变 API 接口
- 兼容性测试通过

步骤：
1. 分析现有代码
2. 制定重构计划
3. 分步执行
4. 验证测试
```

### 7.3 安全审查

```
任务：安全审查 {模块/仓库}

范围：
- 代码安全
- 依赖安全
- 配置安全

标准：
- OWASP Top 10
- 企业安全规范

输出：
- 问题清单（高/中/低）
- 修复建议
- 验证方法
```

---

## 八、企业部署检查表

### 8.1 部署前检查

```
□ 代码审查通过
□ 所有测试通过
□ 安全扫描无高危问题
□ 性能测试达标
□ 文档已更新
□ 监控告警已配置
□ 回滚方案已准备
□ 值班人员已通知
```

### 8.2 Claude Code 辅助检查

```bash
# 一键检查
claude "执行部署前检查，输出检查报告"

# 检查项目：
1. Git 状态
2. 测试覆盖率
3. 安全漏洞
4. 性能基准
5. 文档完整性
```

---

## 💡 今日要点

> 1. **多模块管理**：每个模块独立 CLAUDE.md
> 2. **安全审查**：敏感信息检测、合规检查
> 3. **大代码库策略**：Agent 探索、分模块处理
> 4. **微服务支持**：独立服务、独立配置
> 5. **DevOps 集成**：Docker、K8s、Terraform

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学习，无疑问

---

## 🔗 关联资源

- [[Lesson 12 -团队协作]]
- [[Lesson 7 -调试与排错]]
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

---

*Tags: #claude-code #进阶课程 #lesson-13 #企业级 #安全 #微服务*
