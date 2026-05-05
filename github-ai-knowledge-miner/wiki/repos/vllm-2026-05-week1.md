---
title: vLLM Low-Precision KV Cache / vLLM 低精度 KV Cache
page_type: repo
read_status: unread
read_by_user: false
read_at:
created_at: 2026-05-05
updated_at: 2026-05-05
source_urls:
  - https://github.com/vllm-project/vllm/pull/40177
provenance:
  repo: vllm-project/vllm
  commit_sha: 947138b6c22f9a4751b63b9aa75a2bc4b42835e9
  pr_number: 40177
  issue_number:
license:
confidence: 0.9
status: accepted
tags:
  - read:unread
---

## Knowledge Point / 知识点

Low-precision KV cache support must update storage layout, attention backend, dtype handling, tests, and docs as one path. / 低精度 KV cache 支持必须把存储布局、attention 后端、dtype 处理、测试和文档作为一条路径一起更新。

## Pain Point / 痛点

Large-model serving is constrained by KV cache memory and backend support. Partial low-precision support often breaks only during real inference. / 大模型服务受 KV cache 内存和后端支持约束。低精度支持如果只做一半，常常到真实推理时才坏。

## Method / 方法

vLLM wired NVFP4 through FlashInfer, selected `trtllm-gen` where required, forwarded block scales, and handled FP8 output conversion. / vLLM 把 NVFP4 接入 FlashInfer，在需要时选择 `trtllm-gen`，转发 block scale，并处理 FP8 输出转换。

## Technology / 技术

NVFP4 KV cache, FlashInfer, TensorRT-LLM generated backend, FP8 output buffer, block-scale views, ModelOpt quantization metadata. / NVFP4 KV cache、FlashInfer、TensorRT-LLM 生成后端、FP8 输出缓冲、block-scale view、ModelOpt 量化元数据。

## Solves / 解决了什么

It enables end-to-end NVFP4 KV cache inference on supported Blackwell-class paths, reducing memory pressure for serving. / 它在支持的 Blackwell 级路径上实现端到端 NVFP4 KV cache 推理，降低服务端内存压力。

## Graph Edges / 图谱边

- Supports / 支撑: [KV Cache Memory Efficiency / KV Cache 内存效率](../patterns/kv-cache-memory-efficiency.md)
- Related / 相关: [LightKV Multimodal KV Compression / LightKV 多模态 KV 压缩](../papers/lightkv-lvlm-kv-cache.md)
- Weekly context / 周报上下文: [AI Weekly Digest - 2026-05-05 / AI 周报 - 2026-05-05](../weekly-digests/2026-05-05-ai-weekly.md)
- Index / 首页: [AI Knowledge Graph Index / AI 知识图谱索引](../index.md)

