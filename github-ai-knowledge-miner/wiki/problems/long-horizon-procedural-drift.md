---
title: Long-Horizon Procedural Drift / 长链路过程漂移
page_type: problem
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://arxiv.org/abs/2605.00817v1
  - https://arxiv.org/abs/2605.00798v1
confidence: 0.9
status: accepted
tags:
  - read:unread
---

## Knowledge Point / 知识点

LLMs may follow short procedures but fail as step count and dependency depth increase. / LLM 可能能遵循短流程，但随着步骤数和依赖深度增加而失败。

## Pain Point / 痛点

Agent products often hide procedural drift until a long workflow is already in production. / agent 产品经常到长流程已经上线后，才暴露过程漂移。

## Method / 方法

Use controlled procedural benchmarks and trace-level failure taxonomies, not only final answer accuracy. / 使用受控过程 benchmark 和 trace 级失败分类，而不是只看最终答案准确率。

## Technology / 技术

Procedural benchmark, trace inspection, failure taxonomy, step dependency stress tests. / 过程 benchmark、trace 检查、失败分类、步骤依赖压力测试。

## Solves / 解决了什么

Add constraints, step validation, explicit control flow, prompt ownership, and HITL response paths. / 增加约束、步骤验证、显式控制流、prompt 所有权和 HITL 响应路径。

## Graph Edges / 图谱边

- Defined by / 定义来源: [Procedural Execution Failure](../papers/procedural-execution-llm-steps.md)
- Mitigated by / 缓解方式: [Constrained Agent Execution](../patterns/constrained-agent-execution.md)
- Example / 示例: [RunAgent Constraint-Guided Execution](../papers/runagent-constraint-guided-execution.md)
- Index / 首页: [AI Knowledge Graph Index](../index.md)
