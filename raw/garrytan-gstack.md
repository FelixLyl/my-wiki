# garrytan/gstack — AI Agent 虚拟工程团队 Skills 库

来源：https://github.com/garrytan/gstack
抓取时间：2026-04-11

## 一句话定位

Garry Tan（Y Combinator CEO）个人使用的 Claude Code 工程团队配置。
23 个专家角色 + 8 个工具技能，全部 slash command，全部 Markdown，MIT 开源。
使用者：Garry Tan 本人在过去 60 天内输出了 60 万行生产代码（35% 测试），每天 1-2 万行，兼职状态。

## 核心理念

gstack 是一个流程，不是工具集合：

```
Think → Plan → Build → Review → Test → Ship → Reflect
```

每个 skill 喂给下一个：/office-hours 写 design doc → /plan-ceo-review 读取 → /plan-eng-review 生成测试计划 → /qa 执行。

## 23 个专家角色

### 规划阶段

| 命令 | 角色 | 职责 |
|------|------|------|
| /office-hours | YC Office Hours | 6 个逼问题，重构产品定义，挑战假设，生成设计文档 |
| /plan-ceo-review | CEO/Founder | 重新思考问题，找出隐藏的 10 星产品，4 种模式（扩张/缩减/锁定/重做） |
| /plan-eng-review | 工程经理 | 架构/数据流/ASCII 图/边界情况/测试矩阵 |
| /plan-design-review | 高级设计师 | 0-10 评分各设计维度，交互式逐项改进，AI Slop 检测 |
| /plan-devex-review | DX Lead | 开发者体验：用户旅程/TTHW 基准/竞品对比，20-45 个强制问题 |
| /autoplan | 全流程协调 | 一键运行 CEO → 设计 → 工程评审，只上浮品味决策 |

### 设计阶段

| 命令 | 角色 | 职责 |
|------|------|------|
| /design-consultation | 设计合伙人 | 从零搭建设计系统，生成产品级真实 mockup |
| /design-shotgun | 设计探索 | 生成 4-6 个 AI mockup 变体（GPT Image），浏览器对比板，口味记忆迭代 |
| /design-html | 设计工程师 | mockup → 可发布 HTML/CSS，Pretext 计算布局，30KB 零依赖，框架自适应 |

### 构建/评审阶段

| 命令 | 角色 | 职责 |
|------|------|------|
| /review | Staff 工程师 | 找到通过 CI 但在生产崩溃的 bug，自动修复明显问题 |
| /investigate | 调试专家 | 系统性根因分析，铁律：无调查不修复，3 次失败后停止 |
| /design-review | 会写代码的设计师 | 设计审计 + 直接修复，原子 commit，前后截图 |
| /devex-review | DX 测试员 | 实际测试 onboarding 流程，计时 TTHW，截图错误，与计划分数对比 |
| /codex | 第二意见 | OpenAI Codex CLI 独立代码评审，三模式：评审/对抗挑战/开放咨询 |
| /cso | 首席安全官 | OWASP Top 10 + STRIDE 威胁模型，8/10+ 置信度门槛，17 项误报过滤 |

### 测试/交付阶段

| 命令 | 角色 | 职责 |
|------|------|------|
| /qa | QA Lead | 真实浏览器测试，找 bug，原子 commit 修复，自动生成回归测试 |
| /qa-only | QA 报告员 | 同 /qa 方法论，仅报告不改代码 |
| /ship | 发布工程师 | 同步 main，跑测试，审计覆盖率，推送，开 PR，自动引导测试框架 |
| /land-and-deploy | 发布工程师 | 合并 PR → 等 CI → 等部署 → 验证生产健康，一条命令端到端 |
| /canary | SRE | 部署后监控循环，监控控制台错误/性能回退/页面失败 |
| /benchmark | 性能工程师 | 基准页面加载时间/Core Web Vitals/资源大小，PR 前后对比 |

### 复盘/维护阶段

| 命令 | 角色 | 职责 |
|------|------|------|
| /document-release | 技术写作 | 更新所有项目文档匹配最新发布（README/ARCHITECTURE/CONTRIBUTING），/ship 自动触发 |
| /retro | 工程经理 | 团队周复盘，人均细分，发布连胜，测试健康趋势；/retro global 横跨所有项目 |

## 8 个工具技能

| 命令 | 功能 |
|------|------|
| /browse | 给 Agent 眼睛：真实 Chromium，~100ms/命令 |
| /open-gstack-browser | GStack Browser：带 sidebar，反爬隐身，自动模型路由（Sonnet 操作/Opus 分析） |
| /setup-browser-cookies | 从真实浏览器（Chrome/Arc/Brave）导入 Cookie 到无头会话 |
| /pair-agent | 多 Agent 共享浏览器（OpenClaw/Hermes/Codex/Cursor），独立 tab，scoped token，速率限制 |
| /careful | 安全护栏：破坏性命令前警告（rm -rf/DROP TABLE/force-push） |
| /freeze | 编辑锁定：限制改动到指定目录，防调试时意外破坏其他代码 |
| /guard | /careful + /freeze 合一，生产环境最高安全模式 |
| /learn | 跨会话记忆：管理 gstack 学到的模式/偷坑/偏好，可复查/搜索/裁剪/导出 |

## 与 OpenClaw 集成

```
OpenClaw → sessions_spawn（runtime=acp）→ Claude Code 会话
                                            ↓
                                    调用 gstack skill
```

配置方式（粘贴给 OpenClaw Agent）：
```
Install gstack: run git clone --single-branch --depth 1 
https://github.com/garrytan/gstack.git ~/.claude/skills/gstack 
&& cd ~/.claude/skills/gstack && ./setup
```

AGENTS.md 中配置路由示例：
- 安全审计 → "Load gstack. Run /cso"
- 代码评审 → "Load gstack. Run /review"
- QA 测试 → "Load gstack. Run /qa https://..."
- 完整功能开发 → "Load gstack. Run /autoplan → implement → /ship"

ClawHub 直接安装的 4 个 OpenClaw 原生 Skill（无需 Claude Code）：
- gstack-openclaw-office-hours
- gstack-openclaw-ceo-review
- gstack-openclaw-investigate
- gstack-openclaw-retro

## 多 Agent 并行（与 Conductor 配合）

gstack + [Conductor](https://conductor.build) 实现 10-15 个并行 sprint：
- 每个 Claude Code 会话在独立隔离工作区
- 流程化结构（think→plan→build→review→test→ship）防止 10 个 Agent 变成 10 个混乱源

## 支持的 8 个 AI Coding Agent

Claude Code（主力）/ OpenAI Codex CLI / OpenCode / Cursor / Factory Droid / Slate / Kiro + OpenClaw（原生）

## 安装

```bash
# Claude Code（个人）
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
cd ~/.claude/skills/gstack && ./setup

# 团队共享
./setup --team
cd <your-repo>
~/.claude/skills/gstack/bin/gstack-team-init required
git add .claude/ CLAUDE.md && git commit -m "require gstack for AI-assisted work"
```

## 许可证
MIT，免费永久，无付费层，无等待列表。
