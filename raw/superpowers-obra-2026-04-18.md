# Superpowers — 面向 AI Coding Agent 的完整软件开发方法论

来源：GitHub README
收集日期：2026-04-18
GitHub: https://github.com/obra/superpowers
作者: Jesse Vincent（Prime Radiant）
License: MIT
官方发布文章: https://blog.fsck.com/2025/10/09/superpowers/

---

## 核心定位

**"A complete software development methodology for your coding agents, built on top of a set of composable skills."**

Superpowers 解决的核心问题：AI Coding Agent 的默认行为是「拿到需求就写代码」——缺乏规格澄清、缺乏测试优先、缺乏计划拆分，导致大量返工。

Superpowers 把一套完整的软件工程最佳实践（TDD、YAGNI、DRY、系统化调试）封装为自动触发的 Skills。**不需要手动调用**，Skills 在合适的时机自动激活。

---

## 核心工作流（7 步自动流水线）

1. **brainstorming** — 写代码前触发。通过 Socratic 对话提炼需求，分段展示设计方案供确认，保存设计文档
2. **using-git-worktrees** — 设计确认后触发。在新分支创建隔离工作区，运行项目配置，验证测试基线干净
3. **writing-plans** — 设计确认后触发。把工作拆分为每个 2-5 分钟的任务，每个任务包含精确文件路径、完整代码、验证步骤
4. **subagent-driven-development / executing-plans** — 计划就绪后触发。为每个任务派发新的子 Agent，进行两阶段 Review（规格合规 + 代码质量），或按批次执行并设置人工检查点
5. **test-driven-development** — 实现过程中触发。强制执行 RED-GREEN-REFACTOR 循环：先写失败测试，看它失败，写最少代码，看它通过，提交。删除在测试前写的代码
6. **requesting-code-review** — 任务间触发。按严重等级报告问题，Critical 问题阻塞推进
7. **finishing-a-development-branch** — 所有任务完成后触发。验证测试，提供选项（merge/PR/保留/丢弃），清理工作区

**关键点：Agent 在每个任务前都会检查相关 Skills。这是强制工作流，不是建议。**

---

## Skills 库清单

### Testing
- **test-driven-development** — RED-GREEN-REFACTOR 循环（含测试反模式参考）

### Debugging
- **systematic-debugging** — 4 阶段根因分析流程（含根因追踪、纵深防御、条件等待技术）
- **verification-before-completion** — 确保问题真正已修复

### Collaboration
- **brainstorming** — Socratic 设计提炼
- **writing-plans** — 详细实现计划
- **executing-plans** — 分批执行 + 检查点
- **dispatching-parallel-agents** — 并发子 Agent 工作流
- **requesting-code-review** — 评审前检查清单
- **receiving-code-review** — 响应反馈流程
- **using-git-worktrees** — 并行开发分支
- **finishing-a-development-branch** — Merge/PR 决策工作流
- **subagent-driven-development** — 快速迭代 + 两阶段 Review

### Meta
- **writing-skills** — 按最佳实践创建新 Skills
- **using-superpowers** — Skills 系统介绍

---

## 设计哲学

- **测试驱动（TDD）** — 先写测试，永远如此
- **系统化 > 临时应对** — 流程优于猜测
- **降低复杂度** — 简洁是首要目标
- **证据优于声明** — 声称成功前先验证

---

## 安装方式（多平台）

| 平台 | 安装命令 |
|------|---------|
| Claude Code（官方市场）| `/plugin install superpowers@claude-plugins-official` |
| Claude Code（Superpowers 市场）| `/plugin marketplace add obra/superpowers-marketplace` → `/plugin install superpowers@superpowers-marketplace` |
| Cursor | `/add-plugin superpowers` |
| Gemini CLI | `gemini extensions install https://github.com/obra/superpowers` |
| GitHub Copilot | `copilot plugin install superpowers@superpowers-marketplace` |
| OpenAI Codex CLI | `/plugins` → 搜索 superpowers → 安装 |

---

## 与同类项目的对比

| 项目 | 侧重点 | Agent 数 | 自动触发 |
|------|--------|---------|---------|
| **Superpowers** | 软件工程方法论（TDD+计划+子Agent） | 1 个主 Agent + 子 Agent | ✅ 自动 |
| **GStack** | Sprint 流程闭环（23 个专家角色） | 23 | ❌ 手动调用 |
| **CCGS** | 游戏工作室层级（49 个 Agent） | 49 | 部分 Hooks 自动 |
| **The Agency** | 全业务人格库（工程/设计/营销）| 80+ | ❌ 手动激活 |

Superpowers 的独特性：**不靠人数多取胜，靠自动化流程约束取胜**。技能自动触发是关键差异——开发者无需记住何时调用什么，Agent 自己知道。

---

## Subagent-Driven Development 模式

这是 Superpowers 的核心创新之一：

1. 主 Agent 负责设计和计划
2. 每个任务派发一个**全新的子 Agent**（fresh context，无历史包袱）
3. 子 Agent 完成后，主 Agent 做两阶段 Review（规格合规 → 代码质量）
4. Claude 能够自主连续工作「数小时」而不偏离计划

这是 [[Claude-Sub-Agents]] 在工程场景中的高密度实践。

---

## 社区

- Discord: https://discord.gg/35wsABTejz
- 构建团队：Jesse Vincent + Prime Radiant
- 上架 Claude 官方插件市场（Anthropic 官方认证）
