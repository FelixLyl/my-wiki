# AI 增强开发的三件套：OpenSpec + Superpowers + gstack

来源：https://mp.weixin.qq.com/s/X-NKySPzSVs6zhvDkMpbBg
作者：笨小葱（嘎嘎的地球小屋）
日期：2026-04-25 收录

## 核心摘要（基于搜索结果综合）

文章标题揭示主题：OpenSpec、Superpowers、gstack 是 AI 辅助开发的三件核心配套工具。

---

## OpenSpec

GitHub: https://github.com/Fission-AI/OpenSpec
官网: https://openspec.pro / https://openspec.dev

**本质：规格与变更工件系统。**
重点不是人写规格给 AI 执行，而是通过工件链（proposal.md + specs/ + design.md + tasks.md）让每次代码变更都可追溯、有理据。

- Spec-Driven Development (SDD) for AI coding assistants
- AI coding assistants are powerful but unpredictable when requirements live only in chat history
- Adds a lightweight spec layer so you agree on what to build before any code is written
- 每个变更获得独立目录，含 proposal、specs、design、tasks 四类工件
- 支持 20+ AI assistants via slash commands（Claude Code, Cursor, GitHub Copilot 等）
- 轻量，反对 waterfall，支持 brownfield 项目
- 哲学：fluid not rigid / iterative not waterfall / easy not complex

核心工作流：
```
/opsx:propose "add-dark-mode"
→ 生成 openspec/changes/add-dark-mode/
  ✓ proposal.md（why + what）
  ✓ specs/（requirements + scenarios）  
  ✓ design.md（technical approach）
  ✓ tasks.md（implementation checklist）
/opsx:apply → 按任务实现
/opsx:archive → 归档，更新 specs，准备下一个功能
```

与竞品对比：
- vs Spec Kit（GitHub）：OpenSpec 更轻，无刚性阶段门控，迭代自由
- vs Kiro（AWS）：OpenSpec 不锁定 IDE，不限模型
- vs nothing：无规格 = vague prompts + unpredictable results

install: npm install -g @fission-ai/openspec@latest
Best with: Opus 4.5, GPT 5.2（高推理模型）

---

## Superpowers

GitHub: https://github.com/obra/superpowers
（详见 wiki/Agent工程/Superpowers.md）

本质：强约束的开发纪律系统。把 TDD、设计优先、代码评审、系统调试封装为强制 skills。
- Skills 自动触发，不靠手动调用
- Subagent-Driven Development：每任务派新 context 子 Agent
- 已上架 Anthropic 官方 Claude 插件市场

---

## gstack

GitHub: https://github.com/garrytan/gstack
（详见 wiki/工具与生态/GStack-虚拟工程团队.md）

本质：虚拟工程团队角色系统。23 个专家角色 slash command 覆盖 sprint 全流程：
think → plan → build → review → test → ship → reflect

---

## 三件套组合逻辑（来自知乎分析）

| 工具 | 本质 | 解决什么 |
|------|------|---------|
| OpenSpec | 规格与变更工件系统 | "做什么"——需求模糊、变更追踪、AI偏离预期 |
| Superpowers | 强约束开发纪律系统 | "怎么做"——TDD、代码质量、跳过设计 |
| gstack | 虚拟工程团队角色系统 | "谁来做"——流程缺失、角色分工、质量评审 |

组合方案：
- OpenSpec 管规格（写 proposal → tasks）
- gstack 管 sprint 流程（think→plan→build→review→test→ship）
- Superpowers 管执行约束（Skills 自动触发 TDD、code review）

层次关系：
- gstack /office-hours → 产出 design doc
- OpenSpec /opsx:propose → 结构化 tasks
- Superpowers brainstorming → 每个 task 深挖边界案例
- Superpowers subagent-driven-development → 实现 tasks
- gstack /review → 评审输出
- gstack /qa → 测试
- gstack /ship → 交付

---

## 备注

微信原文直接抓取失败（防爬限制），上述内容综合自：
- GitHub OpenSpec README（官方）
- 知乎分析文章：https://zhuanlan.zhihu.com/p/2021893675458806116
- 知乎文章摘要（DuckDuckGo 搜索结果）
- 腾讯云开发者文章摘要
- dev.to 文章摘要
