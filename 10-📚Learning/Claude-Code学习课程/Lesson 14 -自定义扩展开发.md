# Lesson 14: 自定义扩展开发
> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已学习
> 📌 进阶课程 #8

---

## 一、扩展开发概述

### 1.1 可扩展点

| 扩展点 | 说明 | 复杂度 |
|--------|------|--------|
| **Skills** | 自定义指令技能 | ⭐ 简单 |
| **MCP Servers** | 外部工具集成 | ⭐⭐ 中等 |
| **CLAUDE.md** | 项目配置规范 | ⭐ 简单 |
| **Shell Aliases** | 命令行别名 | ⭐ 简单 |
| **Plugins** | Claude Code 插件 | ⭐⭐⭐ 复杂 |

### 1.2 扩展选择指南

```
需要新命令？              → Shell Aliases
需要结构化指令流程？      → Skills
需要连接外部工具/服务？   → MCP Servers
需要项目级规范？          → CLAUDE.md
需要深度定制？             → Plugins (未来)
```

---

## 二、Skills 开发

### 2.1 Skills 目录结构

```
~/.claude/skills/                    # 全局 Skills
├── readme/
│   ├── SKILL.md                     # Skill 定义
│   └── README.md                    # 使用说明
├── git-commit/
│   └── SKILL.md
└── code-review/
    └── SKILL.md

项目 .claude/skills/                 # 项目级 Skills
├── api-doc/
│   └── SKILL.md
└── test-gen/
    └── SKILL.md
```

### 2.2 SKILL.md 格式

```markdown
---
name: git-commit
description: 智能 Git 提交助手
triggers:
  - /git-commit
  - "帮我提交代码"
  - "生成 commit message"
version: "1.0.0"
author: "Your Name"
---

# Git Commit Skill

## 功能说明
根据代码变更生成规范的 Git commit message。

## 使用方式
/git-commit

## 输出格式
遵循 Conventional Commits 规范：
- feat: 新功能
- fix: 修复 bug
- docs: 文档更新
- style: 代码格式
- refactor: 重构
- test: 测试
- chore: 杂项

## 示例
输入变更：
- 添加用户登录功能
- 修改密码验证逻辑

输出：
feat: add user authentication

fix: improve password validation
```

### 2.3 Skill 开发流程

```bash
# 1. 创建 Skill 目录
mkdir -p ~/.claude/skills/my-skill

# 2. 创建 SKILL.md
cat > ~/.claude/skills/my-skill/SKILL.md << 'EOF'
---
name: my-skill
description: 我的自定义技能
triggers:
  - /my-skill
version: "1.0.0"
---

# 我的自定义技能

## 功能说明
[描述技能功能]

## 使用方式
/my-skill
EOF

# 3. 验证 Skill
claude /my-skill
```

---

## 三、MCP 服务器开发

### 3.1 MCP 服务器结构

```typescript
// my-mcp-server/src/index.ts
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// 创建服务器
const server = new McpServer({
  name: "my-mcp-server",
  version: "1.0.0"
});

// 注册工具
server.tool(
  "my-tool",
  "执行自定义操作",
  {
    input: z.string()
  },
  async ({ input }) => {
    // 实现逻辑
    return {
      content: [{ type: "text", text: `结果: ${input}` }]
    };
  }
);

// 启动服务器
const transport = new StdioServerTransport();
server.run(transport);
```

### 3.2 MCP 服务器模板

```bash
# 初始化项目
mkdir my-mcp-server && cd my-mcp-server
npm init -y
npm install @modelcontextprotocol/sdk zod

# 创建目录结构
mkdir -p src
touch src/index.ts
```

### 3.3 MCP 资源与提示

```typescript
// 资源定义
server.resource(
  "config",
  "config://app",
  async (uri) => ({
    contents: [{
      uri: uri.href,
      mimeType: "application/json",
      text: JSON.stringify({ theme: "dark" })
    }]
  })
);

// 提示模板
server.prompt(
  "analyze-code",
  "分析代码质量",
  { file: z.string() },
  ({ file }) => ({
    messages: [{
      role: "user",
      content: { type: "text", text: `分析这个文件: ${file}` }
    }]
  })
);
```

### 3.4 完整 MCP 服务器示例

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import * as fs from "fs";

const server = new McpServer({
  name: "file-organizer",
  version: "1.0.0"
});

// 工具：整理文件
server.tool(
  "organize-files",
  "按规则整理文件",
  {
    sourceDir: z.string(),
    rules: z.object({
      byExtension: z.boolean().optional(),
      byDate: z.boolean().optional()
    })
  },
  async ({ sourceDir, rules }) => {
    const files = fs.readdirSync(sourceDir);
    const moved: string[] = [];

    for (const file of files) {
      const fullPath = `${sourceDir}/${file}`;
      
      if (rules.byExtension && fs.statSync(fullPath).isFile()) {
        const ext = file.split(".").pop() || "other";
        const destDir = `${sourceDir}/${ext}`;
        
        if (!fs.existsSync(destDir)) {
          fs.mkdirSync(destDir);
        }
        
        fs.renameSync(fullPath, `${destDir}/${file}`);
        moved.push(`${file} -> ${ext}/`);
      }
    }

    return {
      content: [{ type: "text", text: `整理完成，移动了 ${moved.length} 个文件:\n${moved.join("\n")}` }]
    };
  }
);

const transport = new StdioServerTransport();
server.run(transport);
```

---

## 四、Shell 别名与脚本

### 4.1 自定义 Aliases

```bash
# ~/.zshrc

# Claude Code 快捷命令
alias cc="claude"
alias ccr="claude -r"           # 只读模式
alias ccp="claude -p"            # 规划模式
alias ccs="cc-switch"

# 项目特定别名
alias cc-api="cd ~/projects/api && claude"
alias cc-web="cd ~/projects/web && claude"
alias cc-docs="cd ~/docs && claude"

# 功能别名
alias cc-pr="claude '审查当前 PR'"
alias cc-test="claude '运行测试并分析结果'"
alias cc-doc="claude '更新项目文档'"
```

### 4.2 自定义函数

```bash
# ~/.zshrc

# 快速代码审查
code-review() {
  echo "开始代码审查..."
  claude "审查 $(git diff --name-only HEAD~1) 的变更"
}

# 快速生成文档
gen-doc() {
  claude "为 $1 生成文档"
}

# 快速搜索
search-code() {
  claude "在项目中搜索 '$1' 相关代码"
}
```

### 4.3 Claude 包装脚本

```bash
#!/bin/bash
# ~/bin/claude-dev

# 根据目录切换模型
PROJECT_TYPE=$(basename "$(pwd)")

case $PROJECT_TYPE in
  api|backend)
    cc-switch use sonnet
    ;;
  frontend)
    cc-switch use haiku
    ;;
  ml|data)
    cc-switch use opus
    ;;
  *)
    cc-switch use sonnet
    ;;
esac

# 启动 Claude
exec claude "$@"
```

---

## 五、CLAUDE.md 深度定制

### 5.1 多环境配置

```markdown
# CLAUDE.md

## 环境识别
<!-- 读取当前环境变量 -->

## 开发规范
根据环境自动应用规范：

### 开发环境 (development)
- 热重载开启
- 详细日志
- Mock 数据可用

### 测试环境 (testing)
- 测试数据隔离
- 性能日志
- 覆盖率要求 > 80%

### 生产环境 (production)
- 最小化日志
- 错误监控开启
- 性能优化优先
```

### 5.2 项目状态感知

```markdown
# CLAUDE.md

## 当前版本
v2.1.145

## 开发阶段
- [x] 用户认证
- [x] 数据持久化
- [ ] 支付集成
- [ ] 邮件通知

## 紧急任务
- 修复 #123: 登录超时问题
- 优化 #456: API 响应时间

## 最近变更
- 2026-05-22: 添加用户模块
- 2026-05-20: 重构数据库层
```

### 5.3 动态规范

```markdown
# CLAUDE.md

## 代码风格规则

### TypeScript
```typescript
// 类型优先
const handler = async (req: Request): Promise<Response> => {
  return new Response(JSON.stringify({ status: "ok" }));
};

// 错误处理
try {
  await riskyOperation();
} catch (error) {
  console.error("Operation failed:", error);
  throw error;
}
```

### React 组件
```tsx
// 函数组件 + Hooks
const Component: React.FC<Props> = ({ title }) => {
  const [state, setState] = useState(initialValue);
  
  useEffect(() => {
    // 副作用逻辑
  }, []);
  
  return <div>{title}</div>;
};
```
```

---

## 六、模板系统

### 6.1 项目模板

```bash
# 创建模板目录
mkdir -p ~/.claude/templates

# 项目模板
mkdir -p ~/.claude/templates/react-app
mkdir -p ~/.claude/templates/node-api
mkdir -p ~/.claude/templates/python-cli
```

### 6.2 模板使用

```bash
# 快速创建 React 项目
mkdir new-app && cd new-app
cp ~/.claude/templates/react-app/CLAUDE.md ./CLAUDE.md
claude "初始化 React 项目"
```

### 6.3 模板内容示例

```markdown
# new-react-app/CLAUDE.md

## 项目类型
React + TypeScript + Vite

## 技术栈
- React 18
- TypeScript
- Vite
- Tailwind CSS

## 组件规范
- 位置: src/components/
- 命名: PascalCase
- Props 类型定义

## 状态管理
- 组件状态: useState
- 全局状态: React Context

## API 调用
- 使用 fetch 或 axios
- 自定义 hooks: useApi

## 测试
- Vitest + React Testing Library
- 覆盖率要求: 80%
```

---

## 七、配置管理

### 7.1 配置分层

```
~/.claude/settings.json          # 全局设置
├── 模型偏好
├── 默认参数
└── 通用配置

~/.claude/settings.local.json    # 本地覆盖
├── API 密钥
├── 自定义 aliases
└── 调试设置

./CLAUDE.md                      # 项目设置
├── 代码规范
├── 项目结构
└── 特定配置
```

### 7.2 配置同步

```bash
# Git 同步配置
git init
git remote add origin git@github.com:user/claude-config.git
git add .
git commit -m "Update Claude config"
git push

# 多设备同步
git clone git@github.com:user/claude-config.git ~/.claude
```

### 7.3 环境变量

```bash
# .env.claude
ANTHROPIC_API_KEY=sk-xxx
CC_DEFAULT_MODEL=sonnet
CC_TELEMETRY=false
```

---

## 八、发布与分享

### 8.1 发布 Skills

```bash
# 1. 创建 skill.json
cat > ~/.claude/skills/my-skill/skill.json << 'EOF'
{
  "name": "my-skill",
  "version": "1.0.0",
  "description": "技能描述",
  "author": "Your Name",
  "tags": ["git", "automation"],
  "readme": "README.md"
}
EOF

# 2. 创建 README.md
cat > ~/.claude/skills/my-skill/README.md << 'EOF'
# My Skill

## 安装
[安装说明]

## 使用
[使用示例]
EOF

# 3. 打包发布（未来支持 npm）
npm publish @your-org/skill-my-skill
```

### 8.2 社区分享

```
分享渠道：
1. GitHub Gist
2. npm (未来)
3. Claude Code Discord
4. 个人博客
```

---

## 💡 今日要点

> 1. **Skills**：自定义指令流程，最容易上手
> 2. **MCP Servers**：连接外部工具，扩展能力
> 3. **Shell Aliases**：命令行效率提升
> 4. **CLAUDE.md**：项目级配置规范
> 5. **模板系统**：快速启动新项目

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学习，无疑问

---

## 🔗 关联资源

- [[Lesson 6 -Skills系统]]
- [[Lesson 8 -MCP配置与使用]]
- [MCP SDK 文档](https://github.com/modelcontextprotocol/sdk)
- [Skills 示例库](https://github.com/anthropics/claude-code-skills)

---

*Tags: #claude-code #进阶课程 #lesson-14 #扩展开发 #skills #mcp*
