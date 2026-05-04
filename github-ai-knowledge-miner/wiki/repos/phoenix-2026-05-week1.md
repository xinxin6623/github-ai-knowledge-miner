---
title: Phoenix - Server-Side Agent Prompt Assembly and Caching
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

## Problem Background / 问题背景

Agent prompt assembly was partly happening in the frontend, which makes caching harder and spreads static prompt logic across layers. / agent prompt 的组装有一部分在前端发生，这会让缓存更难做，也会把静态 prompt 逻辑分散到多层里。

## Change Summary / 变更摘要

Phoenix moved PXI prompt and tool guidance assembly to the server, kept only user-editable instructions in the frontend, and added Anthropic prompt caching for static instructions and tool definitions. / Phoenix 把 PXI prompt 和工具指引的组装移到服务端，前端只保留用户可编辑指令，并为静态指令和工具定义加入了 Anthropic prompt caching。

## Architecture Idea / 架构想法

Separate static prompt material from editable user input, then cache the static half at the server boundary. That gives you a cleaner contract and better token accounting. / 把静态 prompt 材料和可编辑用户输入拆开，然后在服务端边界缓存静态部分。这样契约更清楚，token 记账也更准确。

## Key Files And Symbols / 关键文件与符号

- PXI prompt assembly
- Anthropic prompt caching
- streamed chat metadata
- PXI trace token details

## Reusable Pattern / 可复用模式

Push prompt composition server-side when you want a stable cache boundary and lower frontend complexity. / 当你想要稳定的缓存边界、同时降低前端复杂度时，就把 prompt 组装推到服务端。

## Tradeoffs / 取舍

The server becomes more authoritative over prompt shape, which is good for consistency but reduces frontend flexibility. Cache telemetry also needs to be surfaced carefully so it stays understandable. / 服务端对 prompt 形状会更有权威性，这对一致性有好处，但会降低前端灵活性。cache telemetry 也得谨慎呈现，才能让人看得懂。

## Tests Or Eval Evidence / 测试与证据

The PR included backend and frontend test coverage, plus typecheck and lint verification. / 这个 PR 包含后端和前端测试覆盖，还做了 typecheck 和 lint 验证。

## Related Papers Or Issues / 相关论文或问题

- This sits in the same direction as [RunAgent](../papers/runagent-constraint-guided-execution.md) and [the procedural-execution diagnostic](../papers/procedural-execution-llm-steps.md): more explicit control over how agents are assembled and run. / 这和 [RunAgent](../papers/runagent-constraint-guided-execution.md)、[过程执行诊断](../papers/procedural-execution-llm-steps.md) 的方向一致：对 agent 的组装和运行施加更明确的控制。

## Local Links / 本地索引

- [Wiki index / Wiki 首页](../index.md)
- [Weekly digest / 周报](../weekly-digests/2026-05-05-ai-weekly.md)
- Related papers / 相关论文: [RunAgent](../papers/runagent-constraint-guided-execution.md), [Procedural execution diagnostic](../papers/procedural-execution-llm-steps.md)
- Related repos / 相关仓库: [LangChain](langchain-2026-05-week1.md), [LlamaIndex](llama-index-2026-05-week1.md)

## When To Reuse / 何时复用

Reuse when your agent UI is leaking static prompt logic or when caching needs a stable server-owned boundary. / 当你的 agent UI 泄漏了静态 prompt 逻辑，或者缓存需要一个稳定的服务端边界时，就复用这个模式。

## When Not To Reuse / 何时不复用

Do not centralize prompt assembly if the frontend genuinely needs to synthesize user-specific prompt structure in real time. / 如果前端确实需要实时合成用户特定的 prompt 结构，就不要把 prompt 组装完全中心化。
