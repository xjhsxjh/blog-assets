# Superpowers 安装与使用指南

> 适用: Claude Code CLI | 插件版本: v5.1.0 (2026-05) | 安装量: 780K+

---

## 一、Superpowers 是什么

Superpowers 是一套**开源、可组合的软件开发方法论 Skills**，它让 Claude 不再"拿到需求就写代码"，而是遵循严格的工程流程：

```
头脑风暴 → 写计划 → 创建分支 → TDD 实现 → 代码审查 → 验证 → 完成
```

核心哲学：**先想清楚再动手，每一步都有检查点。**

---

## 二、安装

### 方式 A：官方市场安装（推荐）

```bash
/plugin install superpowers@claude-plugins-official
```

### 方式 B：Superpowers 社区市场安装

```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

### 安装后激活

```bash
/reload-plugins
```

### 验证安装成功

```bash
/help
```

如果看到 `/brainstorm`、`/write-plan`、`/execute-plan` 三个命令，就说明安装成功了。

---

## 三、核心 Skills 一览

| Skill | 触发场景 | 做什么 |
|-------|----------|--------|
| **brainstorming** | 提出新功能/创意需求 | Socratic 问答式需求澄清，探索 2-3 个方案并对比，输出设计文档。**硬约束：未确认设计前不写代码** |
| **writing-plans** | 设计确认后，准备动手 | 将任务拆成 2-5 分钟的 bite-sized 步骤，精确到文件路径。每步含：失败测试 → 验证失败 → 实现 → 验证通过 → commit |
| **executing-plans** | 计划写好后 | 加载计划，逐条执行，在每个 review 检查点停下来确认 |
| **using-git-worktrees** | 开始实现时 | 自动创建隔离的 git worktree 分支 |
| **test-driven-development** | 写代码时 | 强制执行 RED → GREEN → REFACTOR 循环 |
| **systematic-debugging** | 遇到 bug | 结构化调试：复现 → 假设 → 验证 → 修复 |
| **root-cause-tracing** | 需要追根因 | 从现象逐层追踪到根因 |
| **requesting-code-review** | 实现完成后 | 启动 5 个并行 agent 做代码审查，对照计划检查 |
| **verification-before-completion** | 即将完成时 | 完成前最终检查清单 |
| **dispatching-parallel-agents** | 多任务并行 | 将独立任务分派给多个 agent 同时执行 |
| **subagent-driven-development** | 执行计划时 | 每个子任务分派给独立 subagent 执行 |
| **finishing-a-development-branch** | 开发完毕 | 验证测试、合并/创建 PR |

---

## 四、Slash Commands

安装后会提供以下命令（可直接输入）：

| 命令 | 作用 |
|------|------|
| `/brainstorm` | 启动需求澄清 & 方案设计 |
| `/write-plan` | 基于确认的设计写实现计划 |
| `/execute-plan` | 执行已有计划 |

---

## 五、使用示例

### 示例 1：开发一个新功能

**你说：**
```
/brainstorm 我想给 API 服务加上请求限流功能
```

**Claude 会做（brainstorming 触发）：**

1. 先用 Socratic 对话搞清楚需求，一次只问一个问题：
   ```
   Claude: 限流策略你想用哪种？
   A. 固定窗口（简单，但边界处可能突增）
   B. 滑动窗口（更平滑，但内存占用稍高）
   C. Token Bucket（最灵活，支持突发）
   ```

2. 你回答后，继续追问：
   ```
   Claude: 超出限流时怎么响应？
   A. 返回 429 Too Many Requests
   B. 排队等待
   C. 降级返回缓存数据
   ```

3. 需求澄清完毕，给出 2-3 个方案对比：
   ```
   Claude: 我整理了 3 个方案：

   方案 1 — 最小改动
   - 新增一个限流中间件，基于 Redis + 固定窗口
   - 优点：快速、风险低
   - 缺点：边界突增，Redis 单点依赖

   方案 2 — 健壮架构
   - 抽象 RateLimiter 接口 + 滑动窗口 + 内存 fallback
   - 优点：可扩展、Redis 故障时自动降级
   - 缺点：开发量大

   方案 3 — 务实平衡（推荐）
   - 方案 2 的接口设计 + 先用固定窗口实现
   - 后续可平滑切换到滑动窗口
   - 优点：架构好、当前开发量可控

   推荐方案 3，你怎么看？
   ```

4. 你确认方案后，自动保存设计文档到 `docs/superpowers/specs/2026-06-14-api-rate-limiting-design.md`

5. **自动触发 writing-plans**，进入下一步。

---

### 示例 2：计划执行过程

writing-plans 输出类似：

```
实现计划：API 请求限流

步骤 1: 定义 RateLimiter 接口
  文件: src/rate_limiter/interface.ts
  测试: src/rate_limiter/interface.test.ts
  预估: 3 分钟

步骤 2: 实现 FixedWindowRateLimiter
  文件: src/rate_limiter/fixed_window.ts
  测试: src/rate_limiter/fixed_window.test.ts
  预估: 4 分钟

步骤 3: 创建限流中间件
  文件: src/middleware/rate_limit.ts
  测试: src/middleware/rate_limit.test.ts
  预估: 5 分钟

...

步骤 7: 集成到路由 + 端到端验证
  文件: src/app.ts
  预估: 2 分钟
```

然后你说 `/execute-plan`，Claude 逐条执行，每一步：写失败测试 → 跑测试确认红 → 写实现 → 跑测试确认绿 → commit。

---

### 示例 3：调试

**你说：**
```
/systematic-debugging 用户反馈登录后偶尔被踢回首页，不是必现
```

Claude 会：
1. 帮你构造复现步骤
2. 列出可能假设（token 过期？中间件顺序？session 存储抖动？）
3. 逐一加日志/断点验证
4. 定位根因并修复
5. 补回归测试

---

### 示例 4：代码审查

实现完成后，说：
```
帮我 review 一下改动
```

触发 `requesting-code-review`，启动 5 个并行 agent：
- Agent 1, 2: 对照计划检查（冗余保障）
- Agent 3: 检查明显的 bug
- Agent 4: 检查 git blame 历史上下文
- Agent 5: 检查代码简洁性和重复

每个 issue 打分 0-100，低于 80 分自动过滤。

---

## 六、不需要手动调用 Skill

Superpowers 的 Skill 全部**自动触发** — 你只需要用自然语言描述意图，Claude 会匹配对应的 Skill：

| 你说的话 | 自动触发的 Skill |
|----------|-----------------|
| "帮我分析一下这个需求" | brainstorming |
| "拆一下实现步骤" | writing-plans |
| "开始实现" | executing-plans |
| "这个 bug 帮我看看" | systematic-debugging |
| "review 一下" | requesting-code-review |
| "这几个任务一起跑" | dispatching-parallel-agents |

---

## 七、插件文件位置

如果需要查看或修改 skill 源码：

```bash
~/.claude/plugins/cache/superpowers-marketplace/superpowers/<version>/
```

结构：
```
superpowers/
├── skills/
│   ├── brainstorming/SKILL.md
│   ├── writing-plans/SKILL.md
│   ├── executing-plans/SKILL.md
│   ├── test-driven-development/SKILL.md
│   ├── systematic-debugging/SKILL.md
│   ├── requesting-code-review/SKILL.md
│   ├── ...
│   └── meta/                    # 元技能（写 skill、分享 skill）
└── commands/
    ├── brainstorm.md
    ├── write-plan.md
    └── execute-plan.md
```

---

## 八、建议的日常工作流

```
1. 接到需求 → /brainstorm
2. 方案确认 → (自动触发 writing-plans)
3. 计划审阅 → /execute-plan
4. 实现完毕 → "帮我 review 一下"
5. Review 通过 → /commit-push-pr  (配合 commit-commands 插件)
```

坦白说，前两个步骤（brainstorm + plan）是价值最大的——即使你后续手写代码，把需求想清楚、拆细，已经省下了大量返工时间。

---

Sources:
- [obra/superpowers GitHub](https://github.com/obra/superpowers)
- [obra/superpowers-marketplace](https://github.com/obra/superpowers-marketplace)
- [Superpowers 中文介绍（腾讯云）](https://cloud.tencent.cn/developer/article/2657596)
- [iancleary/dotfiles — Superpowers 评估 Issue](https://github.com/iancleary/dotfiles/issues/15)
