---
title: AI Knowledge Graph Index / AI 知识图谱索引
page_type: query
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls: []
provenance: []
confidence: 0.8
status: accepted
tags:
  - read:unread
---

## Graph Focus / 图谱重点

This wiki is organized as linked knowledge nodes, not source dumps. Each node answers: what pain point, what method, what technology, and what it solves. / 这个 wiki 按知识节点组织，不做资料堆积。每个节点回答：什么痛点、用了什么方法、用了什么技术、解决了什么问题。

## Source Nodes / 来源节点

- [AI Weekly Digest - 2026-05-05 / AI 周报 - 2026-05-05](weekly-digests/2026-05-05-ai-weekly.md)
- [LangChain Tool State Ownership / LangChain 工具状态所有权](repos/langchain-2026-05-week1.md)
- [vLLM Low-Precision KV Cache / vLLM 低精度 KV Cache](repos/vllm-2026-05-week1.md)
- [Phoenix Server-Owned Agent Prompts / Phoenix 服务端 Agent Prompt](repos/phoenix-2026-05-week1.md)
- [LlamaIndex OTel Context Propagation / LlamaIndex OTel 上下文传播](repos/llama-index-2026-05-week1.md)
- [RunAgent Constraint-Guided Execution / RunAgent 约束引导执行](papers/runagent-constraint-guided-execution.md)
- [Procedural Execution Failure / 过程执行失效](papers/procedural-execution-llm-steps.md)
- [LightKV Multimodal KV Compression / LightKV 多模态 KV 压缩](papers/lightkv-lvlm-kv-cache.md)

## Concept Nodes / 概念节点

- [Constrained Agent Execution / 受约束 Agent 执行](patterns/constrained-agent-execution.md)
- [Single Source State Ownership / 单一状态所有权](patterns/single-source-state-ownership.md)
- [KV Cache Memory Efficiency / KV Cache 内存效率](patterns/kv-cache-memory-efficiency.md)
- [Observability Context Propagation / 可观测性上下文传播](patterns/observability-context-propagation.md)
- [Long-Horizon Procedural Drift / 长链路过程漂移](problems/long-horizon-procedural-drift.md)

## Edge-Type Nodes / 边类型节点

No semantic edge type has reached the promotion threshold yet. Current threshold: 10 semantic occurrences. Navigation edges such as `Index / 首页` and `Weekly context / 周报上下文` do not count. / 目前还没有语义边类型达到升级阈值。当前阈值：10 条语义边。`Index / 首页` 和 `Weekly context / 周报上下文` 这类导航边不计入。

## Update Policy / 更新策略

Before adding a new page, search existing titles, aliases, source URLs, and linked concepts. If the idea already exists, update the existing node and add a new source edge instead of creating a duplicate. / 新增页面前先检索已有标题、别名、来源链接和概念节点。如果想法已经存在，就更新旧节点并增加新的来源边，不创建重复节点。

When a semantic edge label appears 10 times, research that relationship and promote it into `edge-types/` as its own explanatory node. / 当某个语义边标签出现 10 次时，要调研这类关系，并把它升级到 `edge-types/` 中作为独立解释节点。
