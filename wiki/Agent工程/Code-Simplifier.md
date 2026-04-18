---
type: project
maturity: stub
date: 2026-04-18
updated: 2026-04-18
tags: [claude-code, plugin, code-quality, refactoring, simplification, anthropic-official]
aliases: [code-simplifier, Code Simplifier]
sources: ["raw/code-simplifier-plugin-2026-04-18.md"]
related: ["[[Superpowers]]", "[[Claude-Code源码架构]]"]
---

# Code Simplifier

Code Simplifier 是 Anthropic 官方 Claude 插件市场的一个 Claude Code Agent，专注于代码简化和可维护性提升。自动处理最近修改的代码段，在不改变功能的前提下改善可读性。

## 具体改善内容

- 减少嵌套深度
- 消除冗余代码
- 改善变量/函数命名
- 用 switch 等清晰结构替换嵌套三元运算
- 遵循 ES modules、显式返回函数声明、React 最佳实践

## 安装

```bash
/plugin install code-simplifier   # Claude Code 官方市场
```

## 定位

聚焦「可读性」单一维度，与 [[Superpowers]] 的 code-review（逻辑正确性）互补。代表了 Claude Code 生态「插件化」趋势：专项能力以独立插件形式发布，按需组合。
