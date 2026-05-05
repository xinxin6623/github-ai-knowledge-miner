---
title: Procedural Execution Failure / 过程执行失效
page_type: paper
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://arxiv.org/abs/2605.00817v1
provenance:
  arxiv_id: 2605.00817
  doi:
  semantic_scholar_id:
  openalex_id:
authors:
  - Sailesh Panda
  - Pritam Kadasi
  - Abhishek Upperwal
  - Mayank Singh
published_at: 2026-05-01
updated_source_at: 2026-05-01
confidence: 0.9
status: accepted
tags:
  - read:unread
---

## Knowledge Point / 知识点

Final-answer accuracy hides whether a model actually followed the requested procedure. / 最终答案准确率会掩盖模型是否真的按流程执行。

## Pain Point / 痛点

Agent workflows can look fine on short tasks but drift or under-execute as step count grows. / agent 流程在短任务上看起来正常，但步骤变长后会漂移或执行不足。

## Method / 方法

The paper diagnoses models with controlled arithmetic procedures whose length and look-back dependencies increase. / 论文用受控算术流程诊断模型，并逐步增加流程长度和回看依赖。

## Technology / 技术

Procedural benchmark, generation-level failure taxonomy, long-step arithmetic control tasks. / 过程 benchmark、生成级失败分类、长步骤算术控制任务。

## Solves / 解决了什么

It provides a measurement frame for detecting long-horizon procedural drift before deploying agent workflows. / 它提供了一套测量框架，在部署 agent 流程前识别长链路过程漂移。

## Graph Edges / 图谱边

- Defines / 定义: [Long-Horizon Procedural Drift / 长链路过程漂移](../problems/long-horizon-procedural-drift.md)
- Motivates / 推动: [Constrained Agent Execution / 受约束 Agent 执行](../patterns/constrained-agent-execution.md)
- Related / 相关: [RunAgent Constraint-Guided Execution / RunAgent 约束引导执行](runagent-constraint-guided-execution.md)
- Related implementation / 相关实现: [LangChain Tool State Ownership / LangChain 工具状态所有权](../repos/langchain-2026-05-week1.md), [Phoenix Server-Owned Agent Prompts / Phoenix 服务端 Agent Prompt](../repos/phoenix-2026-05-week1.md)
- Weekly context / 周报上下文: [AI Weekly Digest - 2026-05-05 / AI 周报 - 2026-05-05](../weekly-digests/2026-05-05-ai-weekly.md)
- Index / 首页: [AI Knowledge Graph Index / AI 知识图谱索引](../index.md)

