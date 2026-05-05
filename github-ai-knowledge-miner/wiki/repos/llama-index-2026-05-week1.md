---
title: LlamaIndex OTel Context Propagation / LlamaIndex OTel 上下文传播
page_type: repo
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://github.com/run-llama/llama_index/pull/21533
provenance:
  repo: run-llama/llama_index
  commit_sha: db30f539427e86eb564aa7428bd95e783ffa944a
  pr_number: 21533
  issue_number:
license:
confidence: 0.88
status: accepted
tags:
  - read:unread
---

## Knowledge Point / 知识点

Tracing integrations need active context propagation, not only parent-child metadata. / tracing 集成不只需要父子元数据，也需要活跃上下文传播。

## Pain Point / 痛点

If spans are created but not attached to Python `contextvars`, downstream auto-instrumented spans appear as siblings instead of children. / 如果 span 被创建但没有附着到 Python `contextvars`，下游自动 instrumentation 的 span 会变成兄弟节点，而不是子节点。

## Method / 方法

Attach each OTel span to the active context, store the context token, and detach it before span exit or drop. / 将每个 OTel span 附着到活跃上下文，保存 context token，并在 span 退出或丢弃前 detach。

## Technology / 技术

OpenTelemetry, Python `contextvars`, span attach/detach tokens, LlamaIndex span handler. / OpenTelemetry、Python `contextvars`、span attach/detach token、LlamaIndex span handler。

## Solves / 解决了什么

It preserves trace hierarchy across agent spans, tool calls, HTTP clients, and observability backends. / 它在 agent span、工具调用、HTTP client 和可观测性后端之间保住 trace 层级。

## Graph Edges / 图谱边

- Supports / 支撑: [Observability Context Propagation / 可观测性上下文传播](../patterns/observability-context-propagation.md)
- Related / 相关: [Phoenix Server-Owned Agent Prompts / Phoenix 服务端 Agent Prompt](phoenix-2026-05-week1.md)
- Weekly context / 周报上下文: [AI Weekly Digest - 2026-05-05 / AI 周报 - 2026-05-05](../weekly-digests/2026-05-05-ai-weekly.md)
- Index / 首页: [AI Knowledge Graph Index / AI 知识图谱索引](../index.md)

