# farzaa：Personal Wiki Skill（Claude Code 实战版）

来源：https://gist.github.com/farzaa/c35ac0cfbeb957788650e36aabea836d
作者：farzaa
抓取时间：2026-04-06

---

## 定位

将个人数据（日记、笔记、消息，任何格式）编译为个人知识 wiki。你是一位编译个人知识 wiki 的**作家**，不是档案管理员。你的工作是阅读条目、理解其含义，并写出能捕捉理解的文章。**Wiki 是一张思维地图。**

## 目录结构

```
your-project/
  data/           # 你的源文件（ingest后不要修改）
  raw/entries/    # 每个条目一个.md文件（由ingest生成）
  wiki/           # 编译后的知识库
    _index.md     # 带别名的主索引
    _backlinks.json   # 反向链接索引
    _absorb_log.json  # 追踪哪些条目已被吸收
    {directories}/    # 从数据中涌现，不要预创建
```

## 核心命令

- `/wiki ingest` — 将数据转换为原始 markdown 条目
- `/wiki absorb all` — 将条目编译进 wiki 文章
- `/wiki query <q>` — 针对 wiki 提问
- `/wiki cleanup` — 审计和丰富现有文章
- `/wiki breakdown` — 找到并创建缺失文章
- `/wiki status` — 显示统计

## 五大工作流

### 1. Ingest（摄入）
将源数据转换为 `raw/entries/` 中的单个 .md 文件。写 Python 脚本 ingest.py 来实现，这一步是机械的，不需要 LLM 智能。

支持格式：Day One JSON、Apple Notes、Obsidian Vault、Notion Export、纯文本/Markdown、iMessage、CSV/电子表格、Email、Twitter/X 归档。脚本必须是**幂等**的。

### 2. Absorb（吸收）
核心编译步骤。对每个条目：
- 阅读条目（文本、frontmatter、元数据，查看任何附件照片）
- **理解其含义**——不是"包含什么事实"，而是"这告诉我什么"
- 与索引匹配——哪些现有文章被触及？是否需要新文章？
- 更新和创建文章——每次接触文章都应显著变好，不要只追加到底部，要整合使文章读起来像一个连贯整体
- 连接模式——当相同主题在多个条目中出现时，那个模式值得拥有自己的文章

**命名实体获得页面的门槛**：能写至少 3 句有意义的话才建页，否则先在相关文章中 mention。
**最大引力陷阱**：总是更容易追加段落到大文章而不是创建新文章，这会产生 5 篇臃肿文章而非 30 篇聚焦文章。

### 3. Query（查询）
- 读 _index.md → 扫描相关文章（每个条目有 `also:` 别名字段）
- 查 _backlinks.json → 高反向链接数表示中心主题
- 读 3-8 篇相关文章，沿 [[wikilinks]] 延伸 2-3 跳
- 综合，以答案为先，引用文章名，使用直接引用，承认空白

**绝不**读取 raw/entries/ 原始日记条目；绝不猜测；绝不修改 wiki 文件（查询只读）。

### 4. Cleanup（清理）
并行子代理审计和丰富每篇文章。评估结构（主题驱动还是日记驱动）、行数（臃肿或存根）、内容，必要时重构。

**Steve Jobs 测试**：Wikipedia 的 Steve Jobs 文章使用"Early life"、"Career"（按时代的子章节），而不是"The Xerox PARC Visit"、"The Lisa Project Failure"。

### 5. Breakdown（拆解）
找到并创建缺失文章。使用**具体名词测试**识别值得独立页面的实体：
- 命名的人物、地点、公司、组织、机构
- 有日期的命名事件或转折点
- 被引用的书籍、电影、音乐、游戏
- 显著使用的工具、平台
- 有名字的项目

**不提取**：通用技术（React、Python、Docker）除非有记录的学习弧；已涵盖的实体；一次性提及。

## 文章目录体系（从数据中涌现，不要预创建）

| 目录 | 类型 | 放什么 |
|------|------|--------|
| people/ | person | 命名个人 |
| projects/ | project | 认真投入构建的事物 |
| places/ | place | 城市、建筑、社区 |
| events/ | event | 特定日期事件 |
| philosophies/ | philosophy | 关于如何工作和构建的知识立场 |
| patterns/ | pattern | 有触发器、机制、结果的重复行为循环 |
| tensions/ | tension | 两个价值观之间不可解决的矛盾 |
| eras/ | era | 连接多篇文章的主要传记阶段 |
| transitions/ | transition | 承诺之间的过渡期 |
| decisions/ | decision | 有列举推理的拐点 |
| experiments/ | experiment | 有假设和结果的时间盒测试 |
| insights/ | insight | 跨领域洞见 |

## 写作风格原则

**这不是关于事物的维基百科，而是关于事物在主人生活中的角色。**

Wikipedia 体写作：
- 平铺直叙，陈述事实
- 每句一个声明，短句
- 简单过去时或现在时
- 归因而非断言："他描述它为充满活力的"而不是"它是充满活力的"
- 让事实暗示重要性
- 日期和细节取代形容词
- **每篇文章最多 2 处直接引用**——引用承载情感，文章本身保持中立

**绝不使用**：破折号滥用、"深刻地"/"真正地"/"令人印象深刻"、"有趣的是"/"重要的是"、修辞性问题、"踏上了旅程"/"即将前往"。

## 文章长度参考

| 类型 | 行数 |
|------|------|
| 人物（1次提及） | 20-30 |
| 人物（3次+提及） | 40-80 |
| 哲学/模式/关系 | 40-80 |
| 时代 | 60-100 |
| 决策/过渡 | 40-70 |
| 最低（任何类型） | 15 |

## Wikipedia UI 实战
作者用 Claude Code + Opus 将 wiki 渲染为 Wikipedia UI：
`make a new vercel project here and run it, it should be a complete visual clone of wikipedia i can play with, backlinks, images, everything.`
结果：一次 prompt 生成了完美的 Wikipedia 克隆。
