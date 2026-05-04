---
title: LangChain - Tool Schema and HITL Updates
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

## Problem Background / 问题背景

LangChain has two related sources of complexity here: tool schema bookkeeping and human-in-the-loop control flow. Both are places where implicit state can drift from what downstream code expects. / LangChain 在这里有两类相关复杂性：工具 schema 的账本管理，以及人类介入的控制流。两者都容易让隐式状态偏离下游代码的预期。

## Change Summary / 变更摘要

The `ToolSchema` PR replaced multiple overlapping cached properties on `BaseTool` with a single cached root object. The HITL PR added a `respond` decision type so a human can supply the effective tool result without executing the tool itself. / `ToolSchema` 这次 PR 把 `BaseTool` 上多个重叠的 cached property 收敛成一个根缓存对象。HITL 那个 PR 则新增了 `respond` 决策类型，让人类可以直接提供工具结果，而不必真的执行工具。

## Architecture Idea / 架构想法

The schema refactor makes one object own validator, JSON schema, args, and token-size estimates. The HITL change extends the decision model with a success-path synthetic `ToolMessage`, which keeps the message protocol intact. / schema 重构让一个对象同时拥有 validator、JSON schema、args 和 token 大小估计。HITL 的改动则给决策模型补了一个成功路径的合成 `ToolMessage`，保持消息协议不变。

## Key Files And Symbols / 关键文件与符号

- `langchain_core.tools.ToolSchema`
- `BaseTool.tool_schema`
- `HumanInTheLoopMiddleware`
- `RespondDecision`
- `_process_decision`

## Reusable Pattern / 可复用模式

Use a single root cache for derived tool metadata when several public properties all depend on the same underlying schema. For HITL, represent human answers as a first-class success response rather than a special-case bypass. / 当多个公开属性都依赖同一个底层 schema 时，用单一根缓存来承载派生工具元数据。对 HITL 来说，把人类回答表示成一等公民的成功响应，而不是特殊旁路。

## Tradeoffs / 取舍

The schema refactor is cleaner but introduces a new exported dataclass that integrations may start depending on. The HITL `respond` path increases flexibility, but it also makes it easier to blur the line between tool execution and human input if governance is weak. / schema 重构更干净，但也引入了一个新的导出 dataclass，集成方可能开始依赖它。HITL 的 `respond` 路径提高了灵活性，但如果治理不足，也更容易模糊工具执行和人工输入的边界。

## Tests Or Eval Evidence / 测试与证据

- The schema PR kept identity-based caching tests passing and added coverage for `NotRequired` TypedDict handling. / schema PR 维持了基于 identity 的缓存测试，并补了 `NotRequired` TypedDict 处理的覆盖。
- The HITL PR preserved provider-required tool-call/tool-message pairing in the synthetic success path. / HITL PR 在合成成功路径里保留了 provider 要求的 tool-call/tool-message 配对。

## Related Papers Or Issues / 相关论文或问题

- This repo movement lines up with [RunAgent](../papers/runagent-constraint-guided-execution.md), [the procedural-execution diagnostic](../papers/procedural-execution-llm-steps.md), and the [weekly digest](../weekly-digests/2026-05-05-ai-weekly.md): stronger execution control and better state ownership. / 这次仓库变动和 [RunAgent](../papers/runagent-constraint-guided-execution.md)、[过程执行诊断](../papers/procedural-execution-llm-steps.md) 以及 [周报](../weekly-digests/2026-05-05-ai-weekly.md) 的主题一致：更强的执行控制，以及更清晰的状态所有权。

## Local Links / 本地索引

- [Wiki index / Wiki 首页](../index.md)
- [Weekly digest / 周报](../weekly-digests/2026-05-05-ai-weekly.md)
- Related papers / 相关论文: [RunAgent](../papers/runagent-constraint-guided-execution.md), [Procedural execution diagnostic](../papers/procedural-execution-llm-steps.md)
- Related repos / 相关仓库: [Phoenix](phoenix-2026-05-week1.md)

## When To Reuse / 何时复用

Reuse the schema pattern in any agent or tool framework that has multiple cached derivations of the same tool definition. / 只要是 agent 或工具框架里有多个缓存派生结果依赖同一定义，就可以复用这个 schema 模式。

## When Not To Reuse / 何时不复用

Do not use the HITL `respond` pattern where human answers must be auditable as separate review artifacts rather than tool results. / 如果人类回答必须作为单独可审计的 review 产物，而不是工具结果，就不要用这个 HITL `respond` 模式。
