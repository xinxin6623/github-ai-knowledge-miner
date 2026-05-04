# Collection Policy

## What To Collect

P0: directly reusable engineering knowledge.

- Agent architectures: planning, memory, tool use, multi-agent orchestration, workflow engines, state management, context engineering.
- RAG and retrieval systems: chunking, reranking, hybrid search, graph RAG, query rewriting, citation, ingestion, eval.
- Inference and serving: KV cache, speculative decoding, batching, routing, quantization, model serving, latency/cost tradeoffs.
- Evaluation: agent eval, LLM-as-judge, retrieval eval, regression harnesses, benchmark datasets, safety eval.
- Real bug fixes: context loss, failed tool calls, hallucinated citations, concurrent state errors, token budget failures, cache invalidation, schema drift.
- Architectural code changes: module boundaries, abstractions, plugin systems, MCP/tool API design, pipelines.

P1: papers and experiments that guide direction.

- Papers with clear methods, experiments, limitations, and code or reproduction leads.
- Papers adopted quickly by high-quality repos.
- Work that improves agent memory, planning, tool use, coding, retrieval, eval, or verification.
- Updated paper versions that add experiments, fix claims, release code, or change conclusions.

P2: high-signal curator material.

- Gists, reading lists, idea files, and implementation notes from trusted researchers or engineers.
- Maintainer explanations in PRs, issues, releases, or design discussions.
- Curated lists with classification, comments, tradeoffs, or code links.

## What To Reject

- Keyword-only AI content without method or implementation detail.
- README-only, spelling, formatting, dependency lock, vendored, or generated-file changes.
- Unsourced opinions with no links, code, data, or verification path.
- Duplicate implementations unless simpler, faster, safer, or better aligned with the user's agent goals.
- Giant resource lists without useful classification or judgment.

## Candidate Scoring

Score 0-10:

- Relevance: 0-2.
- Novelty: 0-2.
- Reusability: 0-2.
- Verifiability: 0-2.
- Impact signal: 0-1.
- Information density: 0-1.

Default action:

- `0-3`: reject.
- `4-5`: keep as candidate only.
- `6-7`: create or update one wiki page.
- `8-10`: create page, update related pattern/problem pages, and include in digest.

## Token Budget Discipline

Use a funnel:

1. Metadata screen: no LLM or minimal LLM.
2. Light scoring: title, abstract, commit message, paths, diffstat, repo metadata.
3. Deep read: only for accepted or near-threshold candidates.
4. Wiki update: load only related pages, not the entire wiki.

Prefer reading less but linking better. The wiki should improve search and reasoning, not become a dump of raw text.
