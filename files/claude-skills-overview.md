# Claude Code Skills 生态概览

> 生成日期: 2026-06-14

---

## 一、已安装的 Skills

| Skill | 位置 | 用途 |
|-------|------|------|
| **drawio** | `.claude/skills/drawio/` | 生成 R9 SoC 系统框图（.drawio 格式），有严格的视觉模板和确认流程 |

---

## 二、内置 Skills（无需安装，开箱即用）

这些 Skills 在系统 prompt 中自动注册，直接通过触发词激活即可使用：

| Skill | 一句话说明 |
|-------|-----------|
| **init** | 分析代码库，自动生成 CLAUDE.md 项目文档 |
| **review** | 审查 Pull Request |
| **security-review** | 对当前分支的改动做安全审查 |
| **simplify** | 审查代码的重用性、质量和效率，并修复问题 |
| **loop** | 定时重复执行某个 prompt 或 slash command |
| **update-config** | 管理 `settings.json`（权限、环境变量、hooks 配置等） |
| **keybindings-help** | 自定义键盘快捷键 |
| **fewer-permission-prompts** | 扫描历史记录，自动生成常用工具的 allowlist，减少权限弹窗 |
| **claude-api** | 构建/调试 Claude API / Anthropic SDK 应用（含 prompt caching 等高级特性） |

---

## 三、官方插件市场（claude-plugins-official）

市场路径: `~/.claude/plugins/marketplaces/claude-plugins-official/`

安装方式:
```bash
/plugin install <plugin-name>@claude-plugins-official
```

### 开发工作流类（强烈推荐入门）

| 插件 | 一句话说明 |
|------|-----------|
| **feature-dev** | 7 阶段结构化功能开发：发现 → 代码探索 → 澄清需求 → 架构设计 → 实现 → 质量审查 → 总结。提供 `/feature-dev` 命令 |
| **code-review** | 4 个并行 agent 审查 PR，含置信度评分（≥80 分才报告），自动过滤误报。提供 `/code-review` 命令 |
| **pr-review-toolkit** | 6 个专项 agent：评论分析、测试覆盖、错误处理、类型设计、代码审查、代码简化。通过自然语言触发 |
| **commit-commands** | `/commit` 自动生成符合仓库风格的 commit message；`/commit-push-pr` 一键 commit + push + 创建 PR；`/clean_gone` 清理远程已删除的本地分支 |
| **plugin-dev** | 插件开发工具箱，含 7 个 skill：hooks 开发、MCP 集成、插件结构、settings 配置、command 开发、agent 开发、skill 开发。提供 `/plugin-dev:create-plugin` 命令 |
| **agent-sdk-dev** | 创建和验证 Claude Agent SDK 应用（Python/TypeScript），提供 `/new-sdk-app` 交互式脚手架 |
| **claude-md-management** | 管理 CLAUDE.md 文件 |
| **claude-code-setup** | Claude Code 安装配置向导 |

### 输出风格类

| 插件 | 一句话说明 |
|------|-----------|
| **learning-output-style** | 教学/学习风格的输出 |
| **explanatory-output-style** | 解释性风格的输出 |

### LSP 语言支持类

| 插件 | 语言 |
|------|------|
| **typescript-lsp** | TypeScript |
| **pyright-lsp** | Python (Pyright) |
| **gopls-lsp** | Go |
| **rust-analyzer-lsp** | Rust |
| **ruby-lsp** | Ruby |
| **jdtls-lsp** | Java |
| **kotlin-lsp** | Kotlin |
| **swift-lsp** | Swift |
| **csharp-lsp** | C# |
| **php-lsp** | PHP |
| **lua-lsp** | Lua |

### 前端 / 设计

| 插件 | 一句话说明 |
|------|-----------|
| **frontend-design** | 自动生成有辨识度、生产级的前端界面，避免"AI 味"设计 |

### 工具 / 集成类

| 插件 | 一句话说明 |
|------|-----------|
| **mcp-tunnels** | MCP 隧道工具 |
| **hookify** | Hooks 相关工具 |
| **ralph-loop** | 循环/自动化工具 |
| **security-guidance** | 安全指南 |
| **math-olympiad** | 数学奥林匹克相关 |
| **playground** | 实验性/测试用 |
| **cwc-makers** | CWC 相关 |

### 第三方外部插件（external_plugins）

| 插件 | 一句话说明 |
|------|-----------|
| **serena** | 代码理解/导航工具 |
| **firebase** | Firebase 集成 |
| **playwright** | 浏览器自动化测试 |
| **gitlab** | GitLab 集成 |

---

## 四、插件目录结构

每个插件遵循标准结构：

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # 插件元数据（必需）
├── .mcp.json            # MCP 服务器配置（可选）
├── commands/            # Slash 命令（可选）
├── agents/              # Agent 定义（可选）
├── skills/              # Skill 定义（可选）
└── README.md            # 文档
```

---

## 五、入门推荐路径

1. **先体验内置 Skill**：在项目目录下直接对话 "帮我初始化 CLAUDE.md" 来触发 `init`，或者用 `/security-review` 做一次安全审查
2. **安装一个工作流插件**：
   ```bash
   /plugin install feature-dev@claude-plugins-official
   ```
   然后 `/feature-dev <你的需求>` 体验完整的 7 阶段结构化开发流程
3. **学习 Skill 源码写法**：
   - 已安装的 `drawio` 的 `SKILL.md` 是现成的参考模板
   - 安装 `plugin-dev` 插件获取 `skill-development` 指导
   - 研究 `feature-dev` 和 `code-review` 的 README 了解 agent/command/skill 如何组合
4. **写自己的第一个 Skill**：实践是最好的学习方式，从一个小场景开始
