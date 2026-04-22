---
type: project
maturity: draft
date: 2026-04-22
updated: 2026-04-22
tags: [claude-code, codex, multi-agent, automation, github, issue-driven, quality-gate, ci-cd, tdd]
aliases: [autoresearch, smallnest/autoresearch, GitHub-Issue-自动化开发]
sources: ["raw/autoresearch-smallnest-2026-04-22.md"]
related: ["[[Superpowers]]", "[[Agent-Skills-addyosmani]]", "[[Claude-Sub-Agents]]", "[[GStack-虚拟工程团队]]", "[[Claude-Code源码架构]]"]
---

# autoresearch

smallnest/autoresearch 是基于 GitHub Issue 驱动的全自动化软件开发工具，核心思路来自 karpathy/autoresearch。它让多个 AI Coding Agent（Claude、Codex、OpenCode）轮流对同一个 Issue 进行实现和交叉审核，直到代码质量达标，然后自动创建 PR、合并、关闭 Issue。

> 你只需负责喝茶和睡觉。一觉醒来，Features 全自动高质量地实现了。

## 核心机制：多 Agent 轮转交叉审核

autoresearch 的关键创新不是单纯的自动化，而是**不同 Agent 之间的交叉审核**：

1. **首轮实现**：`-a` 列表中第一个 Agent 做初始实现
2. **轮转审核**：多个 Agent 按 `(iter-1) % N` 公式依次审核并修复
3. **双轨质量门禁**：
   - 硬门禁：Build / Lint / Test 必须通过
   - 软门禁：LLM 评分 ≥ 85（可调，默认 85 分）
4. **自动收尾**：达标后自动创建 PR → 合并 → 评论 → 关闭 Issue

交叉审核的价值在于每个 Agent 有不同偏好和盲区，轮转能捕获单 Agent 反复迭代会忽略的问题。

## 完整工作流

```
GitHub Issue
    ↓
首个 Agent 实现
    ↓
轮转审核 + 修复（Agent A → B → C → A → ...）
    ↓
评分门控（≥ 85?）
    ↓ 通过
自动创建 PR → 合并 → 关闭 Issue
```

端到端 PRD 链路：

```
/prd skill 生成 PRD
    → Agent 拆分为细粒度 Issue（每个含验收标准 checkbox）
    → autoresearch 逐 Issue 实现
    → 全部完成
```

实际案例：19 个 Issues（#22~#40）从 PRD 到全部实现一个桌面 App。

## 快速使用

```bash
# 处理当前项目 Issue#10
autoresearch/run.sh 10

# 指定项目目录 + 最大迭代次数
autoresearch/run.sh -p /path/to/project 10 16

# 指定使用的 agents 顺序
autoresearch/run.sh -a claude,codex 10

# Continue 模式：从上次中断继续
autoresearch/run.sh -c 42 10

# 调整达标线
PASSING_SCORE=90 autoresearch/run.sh 10
```

前置依赖：GitHub CLI（gh）、Claude Code CLI、OpenAI Codex CLI、OpenCode CLI（按需）

## 项目配置

```
.autoresearch/
├── agents/
│   ├── claude.md       # 自定义 Claude 指令
│   ├── codex.md        # 自定义 Codex 指令
│   └── opencode.md     # 自定义 OpenCode 指令
├── program.md          # 实现规则与约束（含多语言规范模板）
├── workflows/          # 各 Issue 详细记录（自动生成）
└── results.tsv         # 处理结果日志（自动生成）
```

`program.md` 包含多语言规范模板，使用前应只保留目标语言相关章节，减少 token 消耗。

## 参数速查

| 参数 | 默认值 | 说明 |
|------|--------|------|
| PASSING_SCORE | 85 | LLM 评分达标线 |
| MAX_CONSECUTIVE_FAILURES | 3 | 连续失败停止阈值 |
| MAX_RETRIES | 5 | 单次 agent 调用重试次数 |
| -a | claude,codex,opencode | 启用 agents 及顺序 |
| -c | 关闭 | Continue 模式，从中断处恢复 |

## 与同类工具对比

| 维度 | autoresearch | [[Superpowers]] | ralph |
|------|-------------|----------------|-------|
| 驱动方式 | GitHub Issue | 手动 /命令 + 自动触发 | PRD（prd.json） |
| Agent 数量 | 多 Agent 轮转 | 主 Agent + 子 Agent | 单 Agent |
| 质量门禁 | 硬+软双轨 | TDD 强制 + 评分 | 纯工具链 |
| 交叉审核 | ✅ 不同 Agent 互审 | ❌ | ❌ |
| 端到端闭环 | Issue → PR → 合并 → 关闭 | 止步代码 | 止步代码 |
| Continue 模式 | ✅ -c 恢复 | ❌ | ❌ |
| 上下文溢出处理 | 自动交接 | 子 Agent 隔离 | 无 |

autoresearch 的独特之处：**全自动 GitHub 闭环**（Issue 驱动到 PR 合并），这是其他工具不具备的端到端自动化。

## 设计洞见

autoresearch 反映了 AI Coding 工具的一个成熟方向：**把代码 Review 从人工异步流程变成 Agent 同步迭代**。人类 Code Review 是异步的（提 PR → 等人 Review → 修改 → 再 Review），而 autoresearch 把这个周期压缩到分钟级自动循环，且引入了多视角（不同 Agent 的不同偏好）。

质量门禁的双轨设计也值得注意：Build/Test 是客观的硬门禁，LLM 评分是主观的软门禁，两者结合比单纯依赖工具链检查更接近真实 Code Review 标准。

## 相关项目

- 原版思想：[karpathy/autoresearch](https://github.com/karpathy/autoresearch)
- 类似工具：[snarktank/ralph](https://github.com/snarktank/ralph)
- 达尔文 Skill：[alchaincyf/darwin-skill](https://github.com/alchaincyf/darwin-skill)（用 autoresearch 自动优化 Skill）

## 来源

- GitHub: https://github.com/smallnest/autoresearch
- 作者：smallnest（Go 开发者，rpcx 作者）
- License: 未注明
