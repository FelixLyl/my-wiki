---
type: project
maturity: draft
date: 2026-04-25
updated: 2026-04-25
tags: [spec-driven-development, ai-coding, openspec, workflow, claude-code, cursor, requirements]
aliases: [openspec, OpenSpec SDD, Spec-Driven Development]
sources: ["raw/openspec-superpowers-gstack-2026-04-25.md"]
related: ["[[Superpowers]]", "[[GStack-虚拟工程团队]]", "[[CLAUDE-md配置方法论]]", "[[AI增强开发三件套]]"]
---

# OpenSpec

OpenSpec（Fission-AI/OpenSpec）是一套轻量的 Spec-Driven Development（SDD）框架，核心论断是：AI coding assistant 的失控根源在于需求只活在聊天历史里，OpenSpec 在写代码前先建立规格工件，让人与 AI 在动手前对齐"做什么"。

## 核心洞见

> AI coding assistants are powerful but unpredictable when requirements live only in chat history.

OpenSpec 的答案是**在每次变更前建立一套工件链**，而非靠 Prompt 或 memory 凑合：每个功能/重构都获得独立目录，包含 proposal、specs、design、tasks 四个文件，变更的来龙去脉完全可追溯。

## 工作流

```
/opsx:propose "add-dark-mode"
→ openspec/changes/add-dark-mode/
  ✓ proposal.md   — why + what（变更动机和边界）
  ✓ specs/        — requirements + scenarios（验收标准）
  ✓ design.md     — technical approach（实现方案）
  ✓ tasks.md      — implementation checklist（逐条任务）

/opsx:apply        → AI 按 tasks.md 逐条实现
/opsx:archive      → 归档变更，更新 specs，清理上下文
```

update any artifact anytime，不设刚性阶段门控，支持随时迭代。

扩展工作流命令还有：`/opsx:new`、`/opsx:continue`、`/opsx:ff`、`/opsx:verify`、`/opsx:sync`、`/opsx:bulk-archive`、`/opsx:onboard`。

## 设计哲学

- fluid not rigid
- iterative not waterfall
- easy not complex
- built for brownfield（不只是绿地项目）
- scalable from personal to enterprise

## 与竞品对比

| 工具 | 定位 | 差异 |
|------|------|------|
| **OpenSpec** | 轻量 SDD，迭代自由 | 无刚性阶段门控，支持 20+ AI 工具 |
| Spec Kit（GitHub） | 严格规格管理 | 较重，强制 phase gates，Python 环境 |
| Kiro（AWS） | IDE 内置 SDD | 锁定 AWS IDE，仅限 Claude 模型 |
| nothing | 裸 Prompt | 结果不可预测，变更无法追溯 |

## 安装与使用

```bash
npm install -g @fission-ai/openspec@latest
cd your-project
openspec init
# 然后告诉 AI：/opsx:propose <what-you-want-to-build>
```

支持 Claude Code、Cursor、GitHub Copilot 等 20+ 工具。推荐配合高推理模型（Opus 4.5、GPT 5.2）。

## 在三件套中的位置

OpenSpec 解决"做什么"——在 [[Superpowers]] 管执行纪律、[[GStack-虚拟工程团队]] 管角色流程之前，先把需求锁定为可执行的工件。三件套的典型层次：

```
OpenSpec /opsx:propose → 结构化 tasks
  ↓
GStack /office-hours + /plan-eng-review → sprint 计划
  ↓  
Superpowers brainstorming → 边界案例挖掘
  ↓
Superpowers subagent-driven-development → 逐任务执行
  ↓
GStack /review + /qa + /ship → 评审与交付
```

## 来源

- GitHub: https://github.com/Fission-AI/OpenSpec（MIT）
- 官网: https://openspec.pro / https://openspec.dev
- 原始收录：微信文章 https://mp.weixin.qq.com/s/X-NKySPzSVs6zhvDkMpbBg（笨小葱，嘎嘎的地球小屋）
