---
title: LangChain Tool State Ownership / LangChain 工具状态所有权
page_type: repo
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://github.com/langchain-ai/langchain/pull/37103
  - https://github.com/langchain-ai/langchain/pull/37095
provenance:
  repo: langchain-ai/langchain
  commit_sha: dc7a009371d4a1beafb7ab97c319045c8eaf5d5b
  pr_number: 37103
  issue_number:
license:
confidence: 0.93
status: accepted
tags:
  - read:unread
---

## Knowledge Point / 知识点

Use a single root object for derived tool schema state, and model human responses as explicit tool-flow decisions. / 用单一根对象管理派生工具 schema 状态，并把人工回答建模为显式的工具流程决策。

## Pain Point / 痛点

Tool metadata can drift when schema, args, validator, and token estimates are cached separately. HITL flows also become awkward when humans can only approve, edit, or reject. / schema、args、validator、token 估算分散缓存时，工具元数据容易漂移。HITL 如果只有批准、编辑、拒绝，也很难表达“人类直接回答工具”。

## Method / 方法

LangChain introduced `ToolSchema` as the root cache and added `respond` as a HITL decision that emits a synthetic successful `ToolMessage`. / LangChain 引入 `ToolSchema` 作为根缓存，并新增 `respond` 作为 HITL 决策，生成一个成功的合成 `ToolMessage`。

## Technology / 技术

Pydantic `TypeAdapter`, cached root dataclass, TypedDict schema conversion, HITL middleware, provider-compatible tool-call/tool-message pairing. / Pydantic `TypeAdapter`、根 dataclass 缓存、TypedDict schema 转换、HITL middleware、兼容 provider 的 tool-call/tool-message 配对。

## Solves / 解决了什么

It reduces schema invalidation bugs and gives agent runtimes a cleaner success path for human-supplied tool results. / 它减少 schema 失效 bug，并给 agent runtime 一个更干净的“人类提供工具结果”的成功路径。

## Graph Edges / 图谱边

- Supports / 支撑: [Single Source State Ownership / 单一状态所有权](../patterns/single-source-state-ownership.md)
- Supports / 支撑: [Constrained Agent Execution / 受约束 Agent 执行](../patterns/constrained-agent-execution.md)
- Related / 相关: [Phoenix Server-Owned Agent Prompts / Phoenix 服务端 Agent Prompt](phoenix-2026-05-week1.md)
- Weekly context / 周报上下文: [AI Weekly Digest - 2026-05-05 / AI 周报 - 2026-05-05](../weekly-digests/2026-05-05-ai-weekly.md)
- Index / 首页: [AI Knowledge Graph Index / AI 知识图谱索引](../index.md)

