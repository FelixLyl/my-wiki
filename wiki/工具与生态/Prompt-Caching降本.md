---
type: concept
maturity: draft
date: 2026-04-06
updated: 2026-05-11
tags: [cost-reduction, prompt-caching, token, llm-engineering, optimization, kv-cache, bedrock, litellm]
aliases: [Prompt Caching, 提示词缓存, Token降本, 上下文缓存, KV Cache]
sources: ["raw/ai-research-list-2026-04-06.md", "raw/articles/claude-code-source-study-github.md", "raw/shao-meng-prompt-caching-2026-05-11.md"]
related: ["[[CLAUDE-md配置方法论]]", "[[Claude-Agent团队协作]]", "[[Agent记忆持久化]]", "[[Claude-Code源码架构]]"]
---

# Prompt Caching 降本

Prompt Caching 是 Anthropic 提供的 API 功能，允许缓存对话中重复出现的上下文前缀，后续请求复用缓存而无需重复计费全量 Token。在高频调用或长系统提示的场景下，理论上可降低高达 90% 的重复上下文 Token 成本。

## 适用场景

| 场景 | 效果 |
|------|------|
| 长系统提示（CLAUDE.md 注入） | 系统提示不变时，每次调用缓存命中 |
| 多轮对话 | 对话历史前缀缓存，只计费新增内容 |
| 文档批量分析 | 同一文档反复查询时，文档内容只付一次 |
| 知识库问答 | wiki 内容注入时缓存命中 |

## 缓存机制

Prompt Caching 以消息前缀为缓存键。若连续请求的前 N 个 Token 完全相同，则命中缓存，只对差量部分（新增内容）计费。缓存有效期约 5 分钟，适合连续的 API 调用会话，不适合分散的低频调用。

## 与其他降本方案的关系

- **AIClient-2-API**：将 Gemini CLI/Kiro/Qwen Code 等免费大模型封装为本地 OpenAI 兼容接口，支持账号池管理。成本为零，但受限于免费账户配额和稳定性
- **openclaw-token-optimizer**：调研中，关注其具体优化策略
- **混合模型策略**（[[Claude-Agent团队协作]]）：高价模型做高价值工作，低价模型做低价值工作

三类方案可组合使用：高频核心任务走 Prompt Caching，低优先级任务走免费模型代理，复杂任务启用混合团队策略。

## Claude Code 中的实现

Claude Code 源码分析（参见 [[Claude-Code源码架构]]）揭示了 Prompt Cache 在生产级产品中的具体实现：System Prompt 采用分段构建策略，静态部分（行为规范、工具定义）作为缓存前缀，动态部分（用户输入、工具输出）后置。跨模块缓存策略确保不同功能模块共享同一缓存边界，避免缓存失效。这一实现验证了"前缀不变 + 差量计费"的理论模型在大规模用户场景下的可行性。

## 底层机制：KV Cache

LLM 生成文本时分为两个阶段（来源：@shao__meng）：

- **预填充阶段（Prefill）**：一次性处理整个输入提示，为每个 token 计算 Key（K）和 Value（V）向量，形成 KV Cache。计算量大，但结果高度可复用。
- **解码阶段（Decode）**：逐 token 自回归生成输出。

Prompt Caching 的本质是在服务器端持久化 KV Cache。当后续请求的前缀与之前完全一致时，API 服务器直接加载已缓存的 KV 张量，跳过预填充阶段，只执行解码阶段，从而降低延迟和成本。

## Bedrock + LiteLLM 实战

通过 LiteLLM 接入 Bedrock Claude API 时，可使用 `cache_control` 参数标记缓存断点。命中率的关键在于保证请求前缀完全一致——任何细微改动（包括空格、换行）都会导致缓存失效。

## 实施要点

1. 系统提示应尽量固定，避免在系统提示中注入动态内容（时间戳、用户 ID 等）
2. 将静态内容（规范、规则、文档）前置，动态内容（用户输入、查询结果）后置
3. 批量处理任务时，保持系统提示一致可最大化缓存命中率
4. 通过 LiteLLM 接入 Bedrock 时，使用 `cache_control` 显式标记缓存边界
