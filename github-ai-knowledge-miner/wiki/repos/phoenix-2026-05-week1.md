---
title: Phoenix Server-Owned Agent Prompts / Phoenix 服务端 Agent Prompt
page_type: repo
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://github.com/Arize-ai/phoenix/pull/12959
provenance:
  repo: Arize-ai/phoenix
  commit_sha: f223f922420a25152f2ef9624be35b21e6003e89
  pr_number: 12959
  issue_number:
license:
confidence: 0.89
status: accepted
tags:
  - read:unread
---

## Knowledge Point / 知识点

Put static agent prompt and tool guidance assembly on the server so the cache boundary and authority boundary are the same. / 把静态 agent prompt 和工具指引组装放到服务端，让缓存边界和权威边界合一。

## Pain Point / 痛点

When frontend code assembles static prompt material, prompt logic leaks across layers and prompt caching becomes fragile. / 当前端组装静态 prompt 材料时，prompt 逻辑会跨层泄漏，缓存也会变脆弱。

## Method / 方法

Phoenix moved PXI prompt assembly server-side, kept only user-editable instructions in the frontend, and surfaced prompt cache token usage. / Phoenix 把 PXI prompt 组装移到服务端，前端只保留用户可编辑指令，并展示 prompt cache token 用量。

## Technology / 技术

Server-side prompt assembly, Anthropic prompt caching, streamed chat metadata, trace token accounting. / 服务端 prompt 组装、Anthropic prompt caching、流式 chat metadata、trace token 记账。

## Solves / 解决了什么

It lowers frontend complexity, improves cache reliability, and makes agent prompt behavior easier to audit. / 它降低前端复杂度，提高缓存可靠性，并让 agent prompt 行为更容易审计。

## Graph Edges / 图谱边

- Supports / 支撑: [Single Source State Ownership / 单一状态所有权](../patterns/single-source-state-ownership.md)
- Supports / 支撑: [Constrained Agent Execution / 受约束 Agent 执行](../patterns/constrained-agent-execution.md)
- Related / 相关: [LangChain Tool State Ownership / LangChain 工具状态所有权](langchain-2026-05-week1.md)
- Related / 相关: [LlamaIndex OTel Context Propagation / LlamaIndex OTel 上下文传播](llama-index-2026-05-week1.md)
- Weekly context / 周报上下文: [AI Weekly Digest - 2026-05-05 / AI 周报 - 2026-05-05](../weekly-digests/2026-05-05-ai-weekly.md)
- Index / 首页: [AI Knowledge Graph Index / AI 知识图谱索引](../index.md)

