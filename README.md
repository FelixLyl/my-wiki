# Felix's Personal Wiki

> 由 AI 全权维护的个人知识库。人负责投喂素材和提问，AI 负责其余一切。

## 快速上手

### Mac 本地配置

```bash
# 1. Clone 到本地
git clone https://github.com/FelixLyl/my-wiki.git ~/my-wiki

# 2. 用 Obsidian 打开
# Obsidian → Open folder as vault → 选择 ~/my-wiki

# 3. 推荐安装的 Obsidian 插件
# - Obsidian Web Clipper（浏览器扩展，剪藏网页到 raw/articles/）
# - Dataview（动态查询 frontmatter）
# - Git（自动 pull/push 同步）
```

### 日常使用

**投入素材：**
- 网页 → 用 Web Clipper 剪到 `raw/articles/`
- PDF → 直接拖入 `raw/pdfs/`
- 笔记/想法 → 写入 `raw/notes/`

**触发入库：**
- 在 OpenClaw（飞书）里说：「入库」或「处理新素材」

**提问：**
- 说：「问wiki：xxx」

**空闲增值：**
- 说：「做个梦」→ AI 自动产生跨领域洞见

**同步多设备：**
```bash
git pull origin main   # 在其他设备同步最新内容
```

## 目录结构

```
my-wiki/
├── raw/                  # 原始素材（只投入，不修改）
│   ├── articles/         # 网页文章
│   ├── pdfs/             # PDF/论文
│   ├── notes/            # 笔记
│   └── media/            # 图片/截图
├── wiki/                 # 编译后的知识库（AI 维护）
│   ├── _index.md         # 主索引
│   ├── _log.md           # 操作日志
│   ├── concepts/
│   ├── people/
│   ├── projects/
│   ├── insights/
│   ├── sources/
│   ├── eras/
│   ├── decisions/
│   ├── patterns/
│   └── philosophies/
├── skills/
│   └── wiki-compiler/
│       └── SKILL.md      # AI 工作规范
└── .meta/                # 系统元数据（不要手动改）
    ├── compiled_ledger.json
    └── backlinks.json
```

## Maturity 说明

每篇 wiki 文章都有成熟度标记：

| 标记 | 含义 |
|------|------|
| `stub` | 占位卡，内容待补充 |
| `draft` | 初稿，未经核查 |
| `reviewed` | 已复核 |
| `authoritative` | 多源验证，高可信 |
