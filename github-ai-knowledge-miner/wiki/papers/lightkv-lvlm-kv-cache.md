---
title: Make Your LVLM KV Cache More Lightweight
page_type: paper
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://arxiv.org/abs/2605.00789v1
provenance:
  arxiv_id: 2605.00789
  doi:
  semantic_scholar_id:
  openalex_id:
authors:
  - Xihao Chen
  - Yangyang Guo
  - Roger Zimmermann
published_at: 2026-05-01
updated_source_at: 2026-05-01
confidence: 0.88
status: accepted
tags:
  - read:unread
---

## Problem / 问题

LVLM inference pays a large KV-cache tax because vision tokens inflate prefill memory usage. / LVLM 推理会为 KV cache 支付很高的代价，因为视觉 token 会把 prefill 的内存用量顶上去。

## Core Idea / 核心想法

LightKV compresses vision-token KV state using prompt-guided cross-modality message passing, aiming to keep general-purpose quality while shrinking cache size. / LightKV 通过 prompt 引导的跨模态消息传递压缩视觉 token 的 KV 状态，目标是在缩小 cache 的同时保住通用能力。

## Method / 方法

The method progressively compresses vision tokens during prefill, guided by text prompts and cross-modal aggregation. / 这个方法在 prefill 期间逐步压缩视觉 token，并由文本 prompt 和跨模态聚合来引导。

## Architecture / 架构

- Prompt-aware compression
- Cross-modality message passing
- Vision-token redundancy reduction

## Experiments / 实验

The source summary says it was evaluated across eight open-source LVLMs and eight public benchmark datasets, with reported reductions in KV size and compute while preserving performance. / 来源摘要显示，它在 8 个开源 LVLM 和 8 个公共 benchmark 数据集上做了评估，并报告了在保住性能的同时减少 KV 大小和计算量。

## Limitations / 局限

The exact compression schedule and failure cases need a deeper read before I would treat this as a production recipe. / 具体的压缩节奏和失败案例还需要更深的阅读，我才会把它当作生产方案。

## Code Or Reproduction / 代码或复现

No code link surfaced in the source result I collected. / 我收集到的来源结果里没有看到代码链接。

## Agent Relevance / 与 agent 的相关性

Moderate to high. It is not an agent paper, but it matters for multimodal serving and context-budget management. / 中到高。它不是 agent 论文，但对多模态服务和上下文预算管理很重要。

## Related Papers / 相关论文

- [vLLM NVFP4 KV cache support](../repos/vllm-2026-05-week1.md)

## Related Repos / 相关仓库

- [vLLM](../repos/vllm-2026-05-week1.md)

## Local Links / 本地索引

- [Wiki index / Wiki 首页](../index.md)
- [Weekly digest / 周报](../weekly-digests/2026-05-05-ai-weekly.md)
- Related repos / 相关仓库: [vLLM](../repos/vllm-2026-05-week1.md)

## Open Questions / 开放问题

- How stable is the compression under harder multimodal reasoning? / 在更难的多模态推理下，这种压缩有多稳定？
- Does prompt guidance introduce brittle behavior on out-of-distribution vision prompts? / prompt 引导会不会让分布外的视觉输入更脆弱？
