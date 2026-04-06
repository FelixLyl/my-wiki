# zhangpelf/wiki-compiler V2

来源：https://github.com/zhangpelf/wiki-compiler
抓取时间：2026-04-06

---

复刻 Andrej Karpathy 的 LLM 知识库理念，融合深层做梦流与强约束 Map-Reduce 综述架构。完全由 AI 自主驾驶、零幻觉守卫、运维、除草与沉思的 Obsidian 第二大脑工厂。

## 核心理念

Andrej Karpathy 曾分享：不要再自己痛苦地写笔记！把所有的论文、文章、截图粗暴地扔进一个 raw/ 文件夹中，然后让 LLM 作为"知识库编译器 (Wiki Compiler)"，自动把它们提炼、打双链、分类到结构化的 Obsidian 知识库中。

本项目 1:1 实现了 Karpathy 的构想，并引入了更多机制：

## V2 核心机制

### 1. 幂等防重引擎
内置强大的哈希防重叠引擎，只揪出 raw/ 文件夹下新增或修改过的文件，无论触发多少次编译指令都不会重复处理。

### 2. 做梦机制（/wiki-dream）
- **夜间巡逻 (Night Watch)**：扫描并捞起遗忘处理的生数据
- **灵感跨界 (Latent Connection)**：Gap/Stale/Bridge/Tag 多维加权算法，挑出被遗忘的"幽灵卡片"或"高频交通枢纽"，强制在不相干知识碎片间寻找关联，结晶产生跨界洞见

### 3. 零幻觉健康体检（V2 核心进化）
面对断链或死链时，绝不强行捏造概念，而是提取库内所有源头上下文片段，生成 `status: ⚠️ Definition_Needed` 锚点警告追踪卡，将"知识盲区"显性暴露。

### 4. Map-Reduce 文献综述（/wiki-weaver）
- **Map 阶段**：底层 Python 探针仅嗅探和派发绝对路径，指令大模型作为节点独立、并行逐篇萃取几十篇文献的论据元数据
- **Reduce 阶段**：清洗聚类，寻找研究共识与突兀的学术分歧
- **Synthesis 约束**：执行"句级溯源死锁"——最终产出的每一句结论，句末必须标注具体出处卡片引用；查无出处，推断直接抹除

### 5. Obsidian 原生特性支持
- **Dataview Metadata**：所有笔记强制约束 Maturity 等生命周期 YAML 标签；支持 `--apply-tags` 基于图论中心度算法全自动注入标签阵列
- **自动伴生 Marp 幻灯片**：针对宏大知识主题，AI 编译时顺带生成 `.marp.md`

## 工作流图

```
raw/ → /wiki-compiler → 防重检查 → 研读、抽取实体、写成MD → Obsidian wiki/ → 生成YAML/Mermaid/Marp

/wiki-dream → 捕捉遗忘raw数据 → 多维提取知识死角 → 结晶Insight.md

/wiki-weaver → Map(Agent逐篇独立萃取) → Reduce(聚类共识与分歧) → Synthesis(强制句级溯源的大纲)
```

## 使用方式
下载或 Clone 本仓库作为 Agent Skills 之一，告诉 Agent raw/ 资料库绝对路径以及 wiki/ 知识库绝对路径，对话框内输入 /wiki-compiler。
