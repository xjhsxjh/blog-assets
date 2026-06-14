# Claude Code Skills & Plugins 排行榜

> 数据来源：官方市场安装量、GitHub Stars、社区反馈 | 更新于 2026-06

---

## 一、官方市场安装量 Top 10（按 installs 排序）

| 排名 | 插件 | 安装量 | 分类 | 易用度 |
|------|------|--------|------|--------|
| 1 | **Frontend Design** | 860K+ | UI 代码生成 | ★★★★★ |
| 2 | **Superpowers** | 780K+ | 工作流增强 | ★★★★☆ |
| 3 | **Context7** | 350K+ | 文档 & 代码示例 | ★★★★★ |
| 4 | **Code Review** | 350K+ | PR 审查 | ★★★★★ |
| 5 | **Skill Creator** | 290K+ | 工作流转 Skill | ★★★☆☆ |
| 6 | **GitHub MCP** | 270K+ | 仓库/PR/Issue 管理 | ★★★★☆ |
| 7 | **Playwright** | 250K+ | E2E 浏览器自动化 | ★★★★☆ |
| 8 | **CLAUDE.md Management** | 230K+ | 项目记忆 & 规则 | ★★★★★ |
| 9 | **Security Guidance** | 190K+ | 注入/XSS/漏洞预警 | ★★★★★ |
| 10 | **Commit Commands** | 140K+ | Git 工作流自动化 | ★★★★★ |

---

## 二、社区 GitHub Stars 排行

| 排名 | 项目 | Stars | 一句话说明 |
|------|------|-------|-----------|
| 1 | **caveman** | ~66K | 极简 meme skill，用"原始人说话"风格省 65% token |
| 2 | **get-shit-done (GSD)** | ~63K | Meta-prompting，spec 驱动开发 |
| 3 | **awesome-claude-skills** (ComposioHQ) | ~62K | 精选 Skills 目录 |
| 4 | **everything-claude-code (ECC)** | ~41K | 125+ skills / 28 agents / 60 commands 大合集 |
| 5 | **awesome-claude-code** | ~40K | 综合资源列表 |
| 6 | **anthropics/claude-plugins-official** | ~28K | 官方插件市场仓库 |
| 7 | **repomix** | ~23K | 将整个仓库打包成一个文件喂给 Claude |
| 8 | **SuperClaude_Framework** | ~22K | 增强工作流框架 |
| 9 | **ccusage** | ~13K | CLI token 消耗监控 |
| 10 | **context-mode** | ~8.8K | MCP token 膨胀解决方案 |

---

## 三、按场景精选

### 日常开发必备（易用 ★★★★★，装机即用）

| 插件 | 作用 | 安装命令 |
|------|------|----------|
| **Commit Commands** | `/commit` 自动 commit，`/commit-push-pr` 一键到 PR | 内置 |
| **Code Review** | `/code-review` 多 agent 并行审查，自动过滤误报 | 内置 |
| **Security Guidance** | 写代码时主动提示注入/XSS 等安全风险 | `/plugin install security-guidance@claude-plugins-official` |
| **CLAUDE.md Management** | 自动维护项目记忆文件 | `/plugin install claude-md-management@claude-plugins-official` |
| **你用的语言 LSP** | IDE 级代码智能（TS/Python/Rust/Go/Java...） | `/plugin install pyright-lsp@claude-plugins-official` |

### UI / 前端开发

| 插件 | 作用 | 安装命令 |
|------|------|----------|
| **Frontend Design** | 生成有辨识度的生产级 UI，告别 AI 味设计 | `/plugin install frontend-design@claude-plugins-official` |
| **Playwright** | 浏览器 E2E 自动化测试 | `/plugin install playwright@claude-plugins-official` |

### 效率增强

| 插件 | 作用 | 安装命令 |
|------|------|----------|
| **Superpowers** | 综合工作流增强 | 社区安装 |
| **Context7** | 实时文档 & 代码示例，减少幻觉 | `/plugin install context7@claude-plugins-official` |
| **feature-dev** | 7 阶段结构化功能开发 | `/plugin install feature-dev@claude-plugins-official` |

### Skill/Plugin 开发

| 插件 | 作用 | 安装命令 |
|------|------|----------|
| **plugin-dev** | 7 个 skill 教你写 hooks/agent/command/skill/MCP | `/plugin install plugin-dev@claude-plugins-official` |
| **Skill Creator** | 将工作流转为可复用 Skill | `/plugin install skill-creator@claude-plugins-official` |

### 集成 & DevOps

| 插件 | 作用 | 安装命令 |
|------|------|----------|
| **GitHub MCP** | 仓库/PR/Issue 管理 | `/plugin install github-mcp@claude-plugins-official` |
| **GitLab** | GitLab 集成 | `/plugin install gitlab@claude-plugins-official` |
| **Firebase** | Firebase 集成 | `/plugin install firebase@claude-plugins-official` |

---

## 四、社区推荐 "必备 5 件套" 起步配置

经社区 40+ 插件实测筛选，以下 5 个是公认最值得首先安装的：

1. **你主力语言的 LSP**（TS/Pyright/Rust/Go）— IDE 级智能补全和诊断
2. **Context7** — 实时文档查询，减少幻觉
3. **GitHub MCP** — 仓库/PR/Issue 一站式管理
4. **Security Guidance** — 主动漏洞预警
5. **Frontend Design**（做 UI 的话）或 **Superpowers**（通用工作流）

---

## 五、四大插件市场对比

| 市场 | 规模 | 运营方 | 审核 | 自动更新 |
|------|------|--------|------|----------|
| **claude-plugins-official**（官方） | 中等（119+） | Anthropic | 两级审核 | 可配置 |
| **claudemarketplaces.com** | 最大（2919+ 索引） | 社区个人 | 无 | 因源而异 |
| **skillsmp.com** | 未知 | 未知 | 未知 | — |
| **modu-ai/cc-plugins** | 小型（56★） | 韩国社区 | 部分审核 | — |

---

## 六、2026 年关键动态

- **5 月**: 官方目录 v2.1 — 新增插件上下文成本预估、依赖强制执行（`disable` 时拒绝移除被依赖项）、semver 依赖约束
- **安全方面**: SentinelOne (2026.01) 展示了插件供应链攻击可能性；Snyk 审计发现 13% agent-skills 包存在严重缺陷；PromptArmor 演示了注入插件引导 Claude 执行未授权操作
- **生态爆炸**: 插件系统从 2025.10 公开 beta，2026 年爆发增长，从分散的独立 GitHub 仓库逐步集中到官方目录

---

Sources:
- [Claude Code Plugin Marketplaces Compared](https://ice-ice-bear.github.io/posts/2026-03-20-claude-code-marketplaces/)
- [Best GitHub Repositories for Claude Code in 2026](https://docs.bswen.com/blog/2026-04-23-best-github-repos-claude-code/)
- [Claude Code Plugins Get Official Directory](https://www.techtimes.com/articles/317139/20260525/claude-code-plugins-get-official-directory-anthropic-flags-unverified-mcp-risks.htm)
- [Claude Code Plugin Ecosystem Complete Guide](https://qcode.cc/en/claude-code-plugins-guide)
- [The Ultimate Claude Code Resource List (2026 Edition)](https://www.scriptbyai.com/claude-code-resource-list/)
