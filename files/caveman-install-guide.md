# Caveman 安装与使用指南

> 基于 [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) — 70K+ Stars
>
> 一句话简介：让 Claude 像原始人一样说话，输出 token 减少 ~65%，技术内容保持精确。

---

## 一、安装

### 方式 A：Claude Code 插件市场（推荐，你的首选）

```bash
claude plugin marketplace add JuliusBrussee/caveman

claude plugin install caveman@caveman
```

安装后重启 Claude Code。该插件**每次会话自动激活**，无需手动开启。

### 方式 B：一键安装脚本（多 agent 通用）

如果你还同时用 Codex、Cursor 等工具，跑这个一次性全装上：

```bash
curl -fsSL https://raw.githubusercontent.com/JuliusBrussee/caveman/main/install.sh | bash
```

脚本会自动检测你机器上装的所有 AI coding agent，各自装好，已装的跳过。

### 验证安装成功

重启后，在 Claude Code 里输入：

```bash
/caveman-help
```

出现快速参考卡片就说明安装成功。

---

## 二、基本使用

### 激活模式

| 方式 | 示例 |
|------|------|
| 斜杠命令 | `/caveman`（默认 full 档） |
| 自然语言 | "caveman mode" / "talk like caveman" / "less tokens" / "be brief" |

### 四档强度

| 命令 | 效果 | 适用场景 |
|------|------|----------|
| `/caveman lite` | 去掉填充词，保留完整语法 | 既要简洁又不想太野 |
| `/caveman full` | 去冠词、片段句、短同义词（默认，最常用） | 日常编码 |
| `/caveman ultra` | 极限压缩，缩写散文词汇，箭头表因果 | 调试/极速模式 |
| `/caveman wenyan` | 文言文压缩（另有 lite/full/ultra 三档） | 中文用户极致省 token |

### 关闭

```bash
/caveman off
# 或者说
normal mode / stop caveman
```

---

## 三、效果对比

### 示例 1：解释 React 重渲染

**普通 Claude（~69 tokens）：**
> The reason your React component is re-rendering is likely because you're creating a new object reference on each render cycle. When you pass an inline object as a prop, React's shallow comparison sees it as a different object every time, which triggers a re-render. I'd recommend using useMemo to memoize the object.

**Caveman full（~19 tokens）：**
> New object ref each render. Inline object prop = new ref = re-render. Wrap in `useMemo`.

### 示例 2：定位 bug

**普通 Claude：**
> Sure! I'd be happy to help you with that. The issue you're experiencing is most likely caused by your authentication middleware not properly validating the token expiry. Let me take a look and suggest a fix.

**Caveman full：**
> Bug in auth middleware. Token expiry check use `<` not `<=`. Fix: `new Date() <= token.exp`.

### 10 个任务基准测试

| 指标 | 普通 | Caveman | 节省 |
|------|------|---------|------|
| Explain React re-render | 1,180 tokens | 159 tokens | **87%** |
| Fix auth middleware | 704 tokens | 121 tokens | **83%** |
| PostgreSQL connection pool | 2,347 tokens | 380 tokens | **84%** |
| React error boundary | 3,454 tokens | 456 tokens | **87%** |
| **10 任务平均** | **1,214 tokens** | **294 tokens** | **~65%** |

---

## 四、配套命令

安装后这些命令也全部可用：

| 命令 | 作用 |
|------|------|
| `/caveman-commit` | 生成 ≤50 字符的 Conventional Commit message |
| `/caveman-review` | 一行 PR 评论：`L42: :red_circle: bug: user null. Add guard.` |
| `/caveman-compress <file>` | 把文件（如 CLAUDE.md）压缩成 caveman 风格，节省 ~46% 输入 token |
| `/caveman-stats` | 查看当前会话 token 用量 + 累计节省量 + 美元折算 |
| `/caveman-help` | 快速参考卡片 |

---

## 五、推荐玩法

### 1. 保持 full 档为日常默认

重启后自动激活 full 模式，日常编码、调试、重构都够用，几乎零感知。

### 2. 压缩 CLAUDE.md（收益最大）

```bash
/caveman-compress CLAUDE.md
```

CLAUDE.md **每次会话都会加载**，压缩一次，以后每次对话省 token。会自动备份原文件为 `CLAUDE.original.md`。

### 3. 按场景切换

| 场景 | 模式 |
|------|------|
| 写代码 / 调试 / 修 bug | full 或 ultra |
| 头脑风暴 / 学新东西 / 探索代码库 | normal mode（关掉） |
| Code review | `/caveman-review` |
| 提交代码 | `/caveman-commit` |

### 4. 偶尔看看省了多少

```bash
/caveman-stats
```

---

## 六、重要注意事项

- **节省的是输出 token**，不压缩 reasoning/thinking token。如果你主要做需要深度思考的任务，总节省比例会比 65% 低
- 不适合需要详细解释的场景（学习新技术、架构讨论），这时说 `normal mode` 关掉就好
- **代码块、路径、技术术语、错误信息保持原样**，不会被压缩
- **安全操作自动暂停**：遇到 `DROP TABLE` 等不可逆操作时会自动切回正常模式确认

---

Source: [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)
