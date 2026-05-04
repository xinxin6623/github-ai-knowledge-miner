---
title: LlamaIndex - OTel Span Context Propagation Fix
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

## Problem Background / 问题背景

Spans were linked correctly in parent/child metadata, but they were not attached to the active Python context. That breaks downstream tracing tools that rely on `contextvars`. / span 在父子元数据里是连对了的，但它们没有附着到当前 Python context 上。这会破坏依赖 `contextvars` 的下游 tracing 工具。

## Change Summary / 变更摘要

The fix attaches created spans to context, stores the token, and detaches before span exit or drop so the active trace stack stays coherent. / 这个修复会把创建出的 span 附着到 context、保存 token，并在 span 退出或丢弃前 detach，保证当前 trace 栈保持一致。

## Architecture Idea / 架构想法

Trace systems need both structural parentage and active context. If you only set one, you can still end up with broken observability graphs. / trace 系统需要结构上的父子关系，也需要活跃的上下文。只设置其中一个，最终还是可能得到坏掉的可观测性图。

## Key Files And Symbols / 关键文件与符号

- `OTelCompatibleSpanHandler`
- `start_span(context=ctx)`
- `context.attach()`
- `prepare_to_exit_span`
- `prepare_to_drop_span`

## Reusable Pattern / 可复用模式

Whenever you bridge your own span abstraction to OpenTelemetry, verify both parent linkage and current-context behavior. / 只要把你自己的 span 抽象桥接到 OpenTelemetry，就要同时验证父子关系和当前上下文行为。

## Tradeoffs / 取舍

This is a small code change with an outsized operational payoff. The main cost is that tracing integrations now have one more place to get subtly wrong if future refactors skip detach logic. / 这是一个代码量很小、但运维收益很大的改动。主要代价是未来重构如果漏掉 detach 逻辑，tracing 集成又多了一个容易出错的地方。

## Tests Or Eval Evidence / 测试与证据

The PR included a concrete before/after trace example, but no new unit tests were added. / 这个 PR 给出了明确的前后 trace 示例，但没有新增单元测试。

## Related Papers Or Issues / 相关论文或问题

- This is a useful companion to the Phoenix tracing work in the same week. / 它和本周 Phoenix 的 tracing 工作可以一起看。

## When To Reuse / 何时复用

Reuse this exact attach/detach discipline anywhere spans need to be visible to downstream instrumented libraries. / 只要 span 需要被下游已接入 instrumentation 的库看见，就可以复用这套 attach/detach 纪律。

## When Not To Reuse / 何时不复用

Do not use this pattern if your tracing layer does not participate in ambient context propagation. / 如果你的 tracing 层不参与 ambient context 传播，就不必用这个模式。
