---
title: Single Source State Ownership / 单一状态所有权
page_type: pattern
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://github.com/langchain-ai/langchain/pull/37103
  - https://github.com/Arize-ai/phoenix/pull/12959
confidence: 0.9
status: accepted
tags:
  - read:unread
---

## Knowledge Point / 知识点

Give one layer or object authoritative ownership over derived state. / 让一个层或对象对派生状态拥有权威所有权。

## Pain Point / 痛点

When schema, prompt, or trace state is duplicated across layers, behavior drifts and cache invalidation becomes fragile. / 当 schema、prompt 或 trace 状态跨层重复时，行为会漂移，缓存失效也会变脆。

## Method / 方法

Centralize derived state behind a root object or server-owned assembly boundary. / 把派生状态集中到根对象或服务端组装边界后面。

## Technology / 技术

Root schema cache, server-side prompt composition, explicit cache metadata. / 根 schema 缓存、服务端 prompt 组合、显式缓存元数据。

## Solves / 解决了什么

It reduces state drift, cache invalidation errors, and cross-layer ambiguity. / 它减少状态漂移、缓存失效错误和跨层权责模糊。

## Graph Edges / 图谱边

- Evidence / 证据: [LangChain Tool State Ownership](../repos/langchain-2026-05-week1.md)
- Evidence / 证据: [Phoenix Server-Owned Agent Prompts](../repos/phoenix-2026-05-week1.md)
- Related / 相关: [Observability Context Propagation](observability-context-propagation.md)
- Index / 首页: [AI Knowledge Graph Index](../index.md)
