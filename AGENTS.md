# AGENTS.md — Wiki Compiler 使用指南

> 本文件供 AI Agent（Claude Code、Codex、OpenClaw 等）读取。
> 打开此项目时，请先读完本文件，了解 wiki 结构和工作约定。

---

## 这是什么项目

这是一个由 AI 全权维护的个人知识库（Personal Wiki）。
人负责投喂素材，AI 负责理解、提炼、打双链、归档。

核心理念来自 Andrej Karpathy：不要自己维护 wiki，让 LLM 做所有账务工作。

---

## 路径约定

```
WIKI_ROOT  = ~/my-wiki                        （本项目根目录）
RAW_DIR    = ~/my-wiki/raw/                   （原始素材，只投入，不修改）
WIKI_DIR   = ~/my-wiki/wiki/                  （编译后的知识库，AI 维护）
META_DIR   = ~/my-wiki/.meta/                 （系统元数据，不要手动修改）
LEDGER     = ~/my-wiki/.meta/compiled_ledger.json
SKILL      = ~/my-wiki/skills/wiki/SKILL.md
```

---

## 三层架构

| 层 | 目录 | 谁来维护 | 规则 |
|----|------|---------|------|
| 第一层：原始来源 | `raw/` | 人投入 | **不可变**，AI 只读不改 |
| 第二层：知识库 | `wiki/` | AI 全权 | 结构化 Markdown，双链，有主索引 |
| 第三层：配置文件 | `AGENTS.md` / `SKILL.md` | 人+AI 共同维护 | 定义工作流和约定 |

---

## 常用指令

| 主人说 | AI 做什么 |
|--------|-----------|
| `入库` / `/wiki` | 检查 raw/ 新文件 → 理解编译 → 写入 wiki/ → git push |
| `做个梦` / `/wiki-dream` | 夜间巡逻 + 灵感碰撞 + 健康检查 → git push |
| `问wiki: xxx` | 读 _index.md → 找相关文章 → 综合回答（只读，不修改） |
| `知识库状态` | 统计报告：文章数/素材数/maturity 分布/最近日志 |

---

## 详细工作流

**完整规范见：** `skills/wiki/SKILL.md`

请在执行任何入库/做梦操作前，先读一遍 SKILL.md，严格按照其中的工作流执行。

关键约束（摘要）：
- **永不修改 raw/ 下的文件**
- 编译前先读 `wiki/_index.md` 查重，避免创建重复文章
- 每次触碰已有文章前必须先重新读一遍
- 新建文章门槛：能写满 3 句有意义的话才建页
- 每次入库/做梦结束后必须 `git push origin main`

---

## wiki/ 目录结构

```
wiki/
├── _index.md          # 主索引（所有文章目录 + 统计）
├── _log.md            # 操作日志（只追加）
├── concepts/          # 概念、技术方向
├── people/            # 人物
├── projects/          # 项目
├── insights/          # 做梦产生的跨领域洞见
├── sources/           # 重要原始素材的摘要页
├── patterns/          # 行为/思维模式
├── eras/              # 重要阶段
├── decisions/         # 决策记录
└── philosophies/      # 价值观、方法论
```

---

## 注意事项

- 本项目同时被 **OpenClaw**（服务器端，有定时做梦）和 **Claude Code**（本地开发）使用
- OpenClaw 负责：定时做梦、PDF 解析、cron 自动化
- Claude Code 负责：项目开发中随手入库、wiki 查询、经验沉淀到 raw/
- 两端共享同一个 GitHub 仓库，操作前建议先 `git pull`
