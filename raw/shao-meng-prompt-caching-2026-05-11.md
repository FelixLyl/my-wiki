# 再来学习一遍 Prompt Caching，怎么把缓存命中率提上去？

来源：https://x.com/shao__meng/status/2027905170252959765
作者：@shao__meng（Meng Shao，Anthropic）
日期：2026-02-28（推测，待确认）
采集方式：Tavily 搜索（部分内容）

---

## 原文摘录（Tavily 搜索所得，非完整线程）

最近在帮团队做 Prompt Caching 提升的专项，使用的 Bedrock Claude API，通过 Litellm 接入。

LLM 生成文本时分为两个阶段：

- **预填充阶段**：一次性处理整个输入提示，为每个 token 计算 Key（K）和 Value（V）向量，形成 KV Cache。这一步计算量大，但高度可复用。
- **解码阶段**：逐 token 自回归生成输出。

提示缓存的本质就是在服务器端持久化 KV Cache。当后续请求的前缀与之前完全一致时，LLM API 服务器直接加载已缓存的 KV 张量，跳过预填充阶段，只需进行解码阶段。

目标：把缓存命中率提上去。

相关技术：`cache_control`（Bedrock Claude API 的缓存控制参数）

---

## 注意

本文件为部分内容采集，推文为多推线程，完整内容未能获取。
如需补全，请主人提供完整推文截图或文字。
