# smallnest/autoresearch

> 来源: https://github.com/smallnest/autoresearch
> 抓取时间: 2026-04-22

## 项目简介

全自动化软件开发工具，你只需负责喝茶和睡觉。一觉醒来，Features 全自动高质量的实现了。

基于 karpathy/autoresearch 思想实现的通用的基于 GitHub Issue 管理的全自动化开发工具。支持任意 Git + GitHub 项目（Go、Node.js、Python、Rust、Java 等）。

作者：smallnest（知名 Go 开发者，rpcx 作者）

## 核心机制：多 Agent 轮转交叉审核

- **触发输入**：GitHub Issue
- **首次实现**：`-a` 列表中第一个 Agent 做初始实现
- **轮转审核**：多个 Agent 按 `(iter-1) % N` 公式轮流审核+修复
- **质量门禁**：Build/Lint/Test 硬门禁 + LLM 评分 ≥ 85（软门禁）
- **自动收尾**：评分达标 → 自动创建 PR → 合并 → 评论 Issue → 关闭 Issue

## 快速使用

```bash
git clone git@github.com:smallnest/autoresearch.git

# 处理当前目录项目的 Issue#10
autoresearch/run.sh 10

# 处理指定目录项目的 Issue#10
autoresearch/run.sh -p /path/to/project 10

# 指定最大迭代次数为 16 次
autoresearch/run.sh -p /path/to/project 10 16

# 调整达标线为 90 分
PASSING_SCORE=90 autoresearch/run.sh 10

# 指定启用的 agents 及顺序（首个 agent 做初始实现）
autoresearch/run.sh -a claude,codex 10

# 继续处理 Issue#42，追加 10 次迭代
autoresearch/run.sh -c 42 10
```

## 前置条件

```bash
gh auth status   # GitHub CLI
which claude     # Claude Code CLI
which codex      # OpenAI Codex CLI
which opencode   # OpenCode CLI
```

## 工作流程

Issue → 首个 Agent 实现 → 轮转审核+修复 → 评分门控（≥85）→ 自动 PR → 合并 → 关闭 Issue

完整端到端工作流：
```
PRD 生成 → Issue 拆分与创建 → 选择 Issue → autoresearch 实现 → 选择下一个 Issue → ...
```

### PRD → Issue → 实现 完整链路

1. 用 `/prd` skill 生成交付需求文档
2. 让 Agent 基于 PRD 拆分为细粒度 Issue（每个可在单次开发会话完成，含验收标准 checkbox）
3. 运行 `./run.sh <issue_number>`
4. autoresearch 自动完成：规划 subtasks → 迭代实现 → 质量门禁 → PR 合并

实际案例：19 个 Issues (#22~#40) 从 PRD 到全部实现一个桌面 App。

## 项目配置结构

```
.autoresearch/
├── agents/
│   ├── codex.md        # 自定义 Codex 指令
│   ├── claude.md       # 自定义 Claude 指令
│   └── opencode.md     # 自定义 OpenCode 指令
├── program.md          # 自定义实现规则与约束
├── workflows/          # 各 Issue 详细记录（自动生成）
└── results.tsv         # 处理结果日志（自动生成）
```

## 参数说明

| 参数 | 默认值 | 说明 |
|------|--------|------|
| -p <path> | 当前目录 | 项目路径 |
| -a <agents> | claude,codex,opencode | 启用 agents 及顺序 |
| -c | 关闭 | Continue 模式，从上次中断继续 |
| PASSING_SCORE | 85 | 达标评分线（百分制） |
| MAX_CONSECUTIVE_FAILURES | 3 | 连续失败停止阈值 |
| MAX_RETRIES | 5 | 单次 agent 调用重试次数 |

## 与 ralph 对比

| 维度 | autoresearch | ralph |
|------|-------------|-------|
| 驱动方式 | GitHub Issue | PRD（prd.json） |
| Agent 模型 | 多 Agent 轮转交叉审核 | 单 Agent 反复迭代 |
| 质量门禁 | 硬门禁（Build/Lint/Test）+ 软门禁（LLM 评分） | 纯工具链检查 |
| 审核机制 | 不同 Agent 交叉审核 | 无独立审核 |
| 端到端 | Issue → PR → 合并 → 关闭（全自动闭环） | 止步于代码完成 |
| Continue 模式 | 支持 -c 恢复 | 无 |
| UI 验证 | 浏览器截图 + LLM 视觉验证 | dev-browser skill |

autoresearch 优势：双轨质量门禁覆盖更广、多 Agent 交叉审核提供不同视角、GitHub 端到端自动化闭环、上下文溢出自动交接、Continue 模式支持中断恢复

ralph 优势：PRD 驱动语义更丰富、四通道记忆完整知识体系、确定性质量门禁更稳定、Skills 插件系统可扩展、内置流程可视化

## 相关链接

- GitHub: https://github.com/smallnest/autoresearch
- 原版思想: https://github.com/karpathy/autoresearch
- 类似工具: https://github.com/snarktank/ralph
- 达尔文 Skill（用 autoresearch 优化 Skill）: https://github.com/alchaincyf/darwin-skill
