---
title: When LLMs Stop Following Steps - A Diagnostic Study of Procedural Execution in Language Models
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

## Problem / 问题

Final-answer accuracy can hide whether a model actually executed the procedure it was given. / 最终答案准确率会掩盖一个事实：模型是否真的按给定流程执行了过程。

## Core Idea / 核心想法

The paper diagnoses procedural fidelity using step-wise arithmetic tasks with growing length and look-back dependencies. / 这篇论文用逐步算术任务来诊断过程忠实度，并逐渐增加长度和回看依赖。

## Method / 方法

Models receive an algorithm plus two numeric inputs and must return the final computed value. The benchmark increases difficulty by extending the procedure and making later steps depend on earlier intermediate values. / 模型收到一个算法和两个数值输入，必须返回最终计算结果。benchmark 通过延长流程、让后续步骤依赖早期中间值来提高难度。

## Architecture / 架构

- Controlled procedural benchmark
- 14-model evaluation
- 55 datasets
- Generation-level failure analysis

## Experiments / 实验

Average first-answer accuracy drops from 61% on 5-step procedures to 20% on 95-step procedures, and the authors identify missing answers, premature answers, self-correction, under-execution, and hallucinated extra steps. / 平均首答准确率从 5 步流程的 61% 掉到 95 步流程的 20%；作者还识别出了漏答、过早回答、自我修正、执行不足和幻觉额外步骤等失败模式。

## Limitations / 局限

This is a diagnostic benchmark, so it tells us a lot about failure modes but less about how to fix them. / 这是一个诊断型 benchmark，所以它更擅长告诉我们失败模式，而不是直接告诉我们怎么修。

## Code Or Reproduction / 代码或复现

No code link surfaced in the source snippet I collected. / 我收集到的来源片段里没有看到代码链接。

## Agent Relevance / 与 agent 的相关性

High. This is a clean warning that agentic workflows can degrade as traces lengthen, even if the model looks competent on short tasks. / 相关性很高。这清楚地提醒我们：即使模型在短任务上看起来很强，随着 trace 变长，agent 流程也会退化。

## Related Papers / 相关论文

- RunAgent
- Other procedural execution and planning-control work

## Related Repos / 相关仓库

- Agent runtimes with explicit step validation and constrained execution loops

## Open Questions / 开放问题

- Which part of the failure is longest-trace decay versus instruction drift? / 这些失败里，多少来自长 trace 退化，多少来自指令漂移？
- What control strategies recover procedural fidelity most effectively? / 哪些控制策略最能恢复过程忠实度？
