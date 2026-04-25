---
type: source
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [wiki-compiler, personal-knowledge, llm-engineering, claude-code]
aliases: [farzaa wiki skill, Personal Wiki Skill]
sources: ["raw/farzaa-personal-wiki-skill.md"]
related: ["[[个人知识库编译器模式]]", "[[幂等防重模式]]", "[[Andrej-Karpathy]]"]
---

# 来源摘要：farzaa — Personal Wiki Skill（Claude Code 实战版）

**原始来源**：https://gist.github.com/farzaa/c35ac0cfbeb957788650e36aabea836d  
**作者**：farzaa  
**抓取时间**：2026-04-06

## 定位差异

与 [[个人知识库编译器模式|zhangpelf/wiki-compiler]] 不同，farzaa 的变体针对**个人生活数据**（日记、笔记、消息）而非技术文档，强调 LLM 是"作家"而非"档案管理员"。核心定位：**wiki 是思维地图**。

## 五大命令

- `/wiki ingest`：源数据 → raw/entries/（Python 脚本，幂等）
- `/wiki absorb all`：条目 → wiki 文章（核心编译步骤）
- `/wiki query <q>`：查询（只读）
- `/wiki cleanup`：并行子代理审计和丰富现有文章
- `/wiki breakdown`：用"具体名词测试"找出并创建缺失文章

## 关键设计决策

- **最大引力陷阱**：注意不要总是追加到大文章，要主动创建聚焦的小文章
- **Steve Jobs 测试**：文章结构按主题组织（"Early life"、"Career"），不按事件列表
- **具体名词测试**：命名的人物/事件/工具/项目获得独立页面的判断标准
- **Wikipedia UI 实验**：用 Claude Code + Opus 一次 prompt 生成了完整 Wikipedia 克隆界面

## 数据来源支持

Day One JSON、Apple Notes、Obsidian Vault、Notion Export、纯文本、iMessage、CSV、Email、Twitter 归档。
