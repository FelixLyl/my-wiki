# repo-analyzer：一句话生成开源项目深度架构报告

来源：https://mp.weixin.qq.com/s/bDP6Ud5Y_dxV8xUETRslCg（公众号：攻防录）
GitHub：https://github.com/yzddmr6/repo-analyzer
作者：yzddmr6
收录时间：2026-04-19

---

## 一句话定义

AI coding agent skill，通过自然语言指令自动对开源项目进行深度架构分析，生成带 Mermaid 架构图的专业级报告。

## 兼容性

支持 Claude Code、Codex、OpenClaw 及任何支持 Skills 格式的 AI 编程 Agent。

## 安装方式

```bash
# npx（推荐）
npx skills add yzddmr6/repo-analyzer

# Plugin Marketplace
/plugin marketplace add yzddmr6/repo-analyzer
/plugin install repo-analyzer@repo-analyzer

# 手动 Git Clone（macOS/Linux）
git clone https://github.com/yzddmr6/repo-analyzer.git ~/.claude/skills/repo-analyzer
```

## 核心特性

1. **架构级分析** — 聚焦"为什么这样设计"，不是只描述"代码做了什么"
2. **自适应报告结构** — 无固定模板，章节根据项目特征动态设计
3. **并行 Sub-agent 分析** — 对大型代码库拆分核心模块，并发分析
4. **竞品对比定位** — 对比同类项目的设计哲学和技术权衡
5. **Mermaid 架构图** — 架构总览 + 数据流 + 每个模块的时序图
6. **交互式工作流** — 根据项目特征提出针对性问题后再深入分析

## 触发词（自动激活）

分析项目 / 分析仓库 / 分析 GitHub / 项目分析 / 源码分析 / 架构分析 / 代码分析 / 学习这个项目 / 研究这个框架 / 看看这个库怎么实现的 / 对比两个项目 / 项目评测 / 框架评测

## 三种分析深度

| 模式 | 核心模块覆盖 | 次要覆盖 | 适用场景 |
|------|------------|---------|---------|
| Quick | ≥30% | ≥10% | 快速了解架构 |
| Standard（默认）| ≥60% | ≥30% | 常规架构分析 |
| Deep | ≥90% | ≥60% | 深入研究每个设计决策 |

## 8 阶段工作流

1. **Clone & Scan** — 克隆仓库（或本地路径），按模块统计有效代码行数
2. **External Research** — Web 搜索评测、对比、架构讨论；爬取官网
3. **Adaptive Q&A** — 根据项目特征生成针对性问题（非固定检查清单）
4. **Dynamic Report Structure** — 根据回答和项目特征动态设计章节布局
5. **Parallel Deep Analysis** — 为每个核心模块 spawn sub-agent，并发分析
6. **Cross-Validation** — 跨模块验证结论，检查代码阅读覆盖率
7. **Multi-Source Fusion** — 融合调研、模块分析、洞见为完整叙事
8. **Final Report** — 输出单个 Markdown 文件（含 Mermaid 图）

## 报告输出路径

```
~/repo-analyses/{project-name}-{date}/ANALYSIS_REPORT.md
```

## 报告标准章节

- 问题背景：解决什么问题？现有方案为何不足？
- 竞品定位：与同类项目的设计哲学差异（非功能清单）
- 项目全景：架构一览
- 深度模块分析：Why > What，含权衡和行业对比
- 评估与启示：优缺点诚实评估 + 可借鉴的经验
- 架构图：Mermaid 系统总览 + 各模块时序图

## 可选增强（MCP 服务器）

- Tavily MCP：网站爬取（tavily_crawl）
- Brave Search MCP：替代搜索提供商

## 使用示例

```
分析项目 https://github.com/astral-sh/ruff
分析一下 ollama/ollama 这个仓库的架构
对比分析 express 和 fastify
```

## 许可证

MIT
