---
title: vLLM - NVFP4 KV Cache Support
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

## Problem Background / 问题背景

NVFP4 support was already partly in place on the write path. The missing piece was the read and attention side of KV cache inference through FlashInfer on Blackwell-class hardware. / NVFP4 在写入路径上已经部分就绪，缺的主要是 Blackwell 级硬件上通过 FlashInfer 做 KV cache 推理时的读取和 attention 侧。

## Change Summary / 变更摘要

This PR finished end-to-end NVFP4 KV cache support by wiring `nvfp4` into supported cache dtypes, forcing the `trtllm-gen` backend where needed, forwarding block scales, and handling FP8 output buffers when the model dtype is not FP8. / 这个 PR 通过把 `nvfp4` 接入支持的 cache dtype、在需要时强制使用 `trtllm-gen` 后端、转发 block scale，并在模型 dtype 不是 FP8 时处理 FP8 输出缓冲，完成了端到端 NVFP4 KV cache 支持。

## Architecture Idea / 架构想法

The implementation treats KV cache quantization as a hardware-aware data path: packed cache layout, backend selection, output dtype handling, and model-opt recognition all move together. / 这个实现把 KV cache 量化当成一个感知硬件的完整数据路径：packed cache 布局、后端选择、输出 dtype 处理和 model-opt 识别一起推进。

## Key Files And Symbols / 关键文件与符号

- `supported_kv_cache_dtypes`
- `nvfp4_kv_cache_split_views`
- `get_dtype_for_flashinfer`
- `ModelOptKVCacheMethod`
- `KV_CACHE_QUANT_ALGOS`

## Reusable Pattern / 可复用模式

When adding a new cache dtype, update the data path, backend selection, tests, and docs together. Partial support tends to fail in the most annoying way: only after deployment. / 新增 cache dtype 时，要同时更新数据路径、后端选择、测试和文档。部分支持最容易在最麻烦的时候出问题：上线之后才暴露。

## Tradeoffs / 取舍

This is hardware-specific complexity, so the maintenance surface grows with each quantized cache variant. The upside is lower memory pressure and better throughput on supported systems. / 这属于硬件特化复杂度，所以每多一种量化 cache 变体，维护面就会变大。好处是在支持的系统上能换来更低的内存压力和更好的吞吐。

## Tests Or Eval Evidence / 测试与证据

The PR extended unit and integration coverage and reported local pass results. / 这个 PR 扩展了单测和集成测试覆盖，并给出了本地通过结果。

## Related Papers Or Issues / 相关论文或问题

- The week’s LightKV paper is conceptually adjacent, though it targets LVLM KV compression rather than low-precision serving kernels. / 本周的 LightKV 论文在概念上是相邻的，不过它针对的是 LVLM 的 KV 压缩，而不是低精度服务 kernel。

## When To Reuse / 何时复用

Reuse this implementation shape when introducing a new KV cache quantization format or backend-specific inference path. / 只要要引入新的 KV cache 量化格式或特定后端推理路径，就可以复用这个实现形态。

## When Not To Reuse / 何时不复用

Do not mirror this complexity unless the hardware and deployment constraints justify it. / 除非硬件和部署约束真的值得，否则不要照搬这么高的复杂度。
