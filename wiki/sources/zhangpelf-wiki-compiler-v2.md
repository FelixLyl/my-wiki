---
type: source
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [wiki-compiler, knowledge-management, llm-engineering, obsidian]
aliases: [zhangpelf wiki-compiler, wiki-compiler V2]
sources: ["raw/zhangpelf-wiki-compiler-v2.md"]
related: ["[[个人知识库编译器模式]]", "[[幂等防重模式]]", "[[做梦机制]]", "[[Andrej-Karpathy]]"]
---

# 来源摘要：zhangpelf/wiki-compiler V2

**原始来源**：https://github.com/zhangpelf/wiki-compiler  
**抓取时间**：2026-04-06

## 定位

1:1 复刻 [[Andrej-Karpathy]] 构想，并引入多项 V2 增强。目标是"完全由 AI 自主驾驶、零幻觉守卫、运维、除草与沉思的 Obsidian 第二大脑工厂"。

## V2 五大核心机制

1. **[[幂等防重模式]]**：哈希防重叠引擎，只处理新增或修改的文件
2. **[[做梦机制]]**（`/wiki-dream`）：夜间巡逻 + 多维加权灵感碰撞算法
3. **零幻觉健康检查**：断链时生成 `⚠️ Definition_Needed` 追踪卡，不强行捏造
4. **Map-Reduce 文献综述**（`/wiki-weaver`）：并行萃取多篇文献，强制句级溯源死锁
5. **Obsidian 原生支持**：Dataview Metadata、基于图论中心度的自动标签、伴生 Marp 幻灯片

## 使用方式

下载或 Clone 作为 Agent Skills 之一，指定 `raw/` 和 `wiki/` 路径，在对话框输入 `/wiki-compiler`。
