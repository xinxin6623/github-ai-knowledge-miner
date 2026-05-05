---
title: Observability Context Propagation / 可观测性上下文传播
page_type: pattern
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://github.com/run-llama/llama_index/pull/21533
confidence: 0.86
status: accepted
tags:
  - read:unread
---

## Knowledge Point / 知识点

Attach observability spans to active runtime context so downstream instrumentation inherits the correct parent. / 把可观测性 span 附着到活跃运行时上下文，让下游 instrumentation 继承正确父级。

## Pain Point / 痛点

Trace graphs become misleading when spans have metadata parentage but no active context propagation. / span 只有元数据父子关系、没有活跃上下文传播时，trace 图会误导人。

## Method / 方法

Attach span context on entry, store the token, and detach before exit or drop. / 进入时 attach span context，保存 token，退出或丢弃前 detach。

## Technology / 技术

OpenTelemetry, Python `contextvars`, span handler lifecycle. / OpenTelemetry、Python `contextvars`、span handler 生命周期。

## Solves / 解决了什么

It keeps trace graphs faithful across agent, tool, and client-library boundaries. / 它让 trace 图在 agent、工具和客户端库边界之间保持真实层级。

## Graph Edges / 图谱边

- Evidence / 证据: [LlamaIndex OTel Context Propagation](../repos/llama-index-2026-05-week1.md)
- Related / 相关: [Single Source State Ownership](single-source-state-ownership.md)
- Index / 首页: [AI Knowledge Graph Index](../index.md)
