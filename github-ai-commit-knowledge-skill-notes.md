# GitHub AI Commit Knowledge Skill Notes

调研日期：2026-05-04

目标：把 GitHub 上持续出现的 AI 相关代码提交、PR、讨论和实现思路，积累成一个可快速搜索、可追溯、可反复沉淀的知识库。知识库重点不只是“代码片段”，而是新架构思想、实现形式、调试经验、权衡取舍和解决问题的套路。

## 核心结论

不要把 GitHub Search 当作实时数据源。Search 适合发现候选仓库、候选 PR、关键词和主题，但有 1,000 条结果上限、独立限流、默认分支/文件大小/索引范围等限制。持续积累应采用：

1. 已选仓库：GitHub App webhook + commits/compare/PR files API。
2. 全网发现：GH Archive hourly events 或 BigQuery 公共数据集，筛出 PushEvent/PullRequestEvent 后再用 GitHub API 补全。
3. 知识抽取：保存原始事件和 patch，再用 LLM/规则抽取结构化卡片，进入全文索引 + 向量索引 + 关系图。

更完整的形态应该是 Karpathy `llm-wiki` 模式在 AI 研发上的具体化：原始来源不可变，LLM 维护中间层 wiki，schema/AGENTS.md 规定写法、索引、引用和更新流程。目标不是把所有资料丢进 RAG，而是把论文、代码变更、Gist、PR 讨论、实验结果编译成持续更新的工程知识地图。

## 收录策略

核心原则：只收“以后能帮 agent 做判断、找代码、借鉴架构、规避坑”的内容。不要按关键词全量收录；所有来源先进入候选池，过质量门槛后才生成 wiki 卡片。

### 应该抓取的内容

优先级 P0：直接可复用的工程知识

- 新的 agent 架构：planning、memory、tool use、multi-agent、workflow orchestration、state management、context engineering。
- RAG 和检索系统实现：chunking、reranking、hybrid search、graph RAG、query rewriting、citation、document ingestion、eval。
- 推理和 serving 优化：KV cache、speculative decoding、batching、routing、quantization、model server、latency/cost tradeoff。
- 评测和 benchmark：agent eval、LLM-as-judge、retrieval eval、regression harness、benchmark dataset、safety eval。
- 真实 bug fix：上下文丢失、工具调用失败、幻觉引用、并发状态错乱、token 预算、缓存失效、schema drift。
- 代码结构变化：抽象边界、模块拆分、pipeline 改造、插件系统、MCP/tool API 设计。

优先级 P1：能指导方向的论文和实验

- 有明确方法、实验和局限的论文，不只是标题热门。
- 带官方代码、第三方实现、benchmark 或复现实验的论文。
- 被多个高质量 repo 快速采用的论文。
- 解决当前 agent 系统痛点的论文：长期记忆、任务分解、代码生成、工具使用、反思/验证、自动测试、planner/executor。
- 论文更新版本：新增实验、修正方法、发布代码、撤回强 claim。

优先级 P2：高信号 curator 内容

- 研究者/工程师写的 Gist、reading list、idea file、实现笔记。
- 论文列表中带短评、分类、取舍判断、代码链接的内容。
- 热门 PR/release 下的 maintainer 解释。
- issue 中的设计讨论、故障复盘、性能数据。

默认不收或低优先级：

- 只有标题党关键词、无方法细节的论文。
- 只改 README、拼写、依赖锁、格式化、生成文件的 commit。
- 无来源、无链接、无可验证信息的观点。
- 大而全的资源列表，除非有清晰分类和人工判断。
- 重复实现，除非它明显更简单、更快、更可靠或更适合你的 agent。

### 每条内容要收录的信息

所有来源都必须保留：

- `source_type`: paper / repo / commit / pr / issue / gist / release / query。
- `source_url`: 原始链接。
- `captured_at`: 抓取时间。
- `source_date`: 发布、提交、更新或合并时间。
- `authors_or_actors`: 作者、maintainer、提交者、评论者。
- `provenance`: arXiv id、DOI、commit sha、PR number、gist revision 等不可变标识。
- `license_or_terms`: 代码 license、论文 license、gist 可见性。
- `confidence`: 高/中/低，以及原因。
- `status`: candidate / accepted / rejected / superseded / needs_review。

论文必须额外收录：

- 研究问题：它到底解决什么痛点。
- 核心方法：一句话能讲清的机制。
- 架构图谱：涉及哪些组件、数据流、训练/推理流程。
- 实验结论：在哪些 benchmark 上有效，提升多少，成本是什么。
- 局限和失败条件：什么场景不适合。
- 代码线索：official code、复现 repo、关键文件、依赖。
- 对 agent 的价值：能改进 memory、planning、tool use、eval、coding、retrieval 中哪一块。
- 关联内容：相关论文、相似 repo、相反结论、后续实现。

代码变更必须额外收录：

- 问题背景：为什么改。
- 改动摘要：不要逐文件流水账，只讲行为变化。
- 架构/设计思想：新增了什么抽象、边界或流程。
- 关键路径：重要文件、函数、类、API。
- 可复用模式：什么时候可以借鉴。
- 取舍：性能、复杂度、兼容性、安全、成本。
- 测试证据：新增/修改了哪些 eval、unit test、benchmark。
- 关联论文或 issue：这次实现是否来自某篇 paper 或讨论。

Gist/curator 内容必须额外收录：

- 作者可信度：为什么这个人/列表值得关注。
- curated claim：作者真正的判断是什么。
- 链接清单：论文、repo、demo、benchmark。
- 分类方式：作者如何组织这些内容。
- 可转化卡片：哪些条目应该升级为 paper card 或 repo card。
- 未验证观点：哪些只是观点，还没有代码或实验支撑。

### 质量评分

建议每个候选来源计算 0-10 分，6 分以上才写入 wiki：

- 问题相关性：0-2，是否贴近你的 agent/AI 工程方向。
- 新颖性：0-2，是否提供新架构、新方法或新 bug 经验。
- 可复用性：0-2，是否能转成实现、检查清单或设计模式。
- 可验证性：0-2，是否有代码、实验、PR、benchmark 或清晰来源。
- 影响信号：0-1，作者、repo、引用、stars、讨论质量。
- 信息密度：0-1，是否比普通摘要更有判断。

### Wiki 页面类型

最终不要只存“来源摘要”，而要拆成几种页面：

- `papers/`: 单篇论文卡片。
- `repos/`: 仓库画像、关键模块、可借鉴点。
- `patterns/`: 可复用架构模式，例如 `Agent Memory Compaction`, `Tool Call Retry Loop`, `Hybrid RAG Pipeline`。
- `problems/`: 问题导向页面，例如 `Long Context Drift`, `Unreliable Tool Calls`, `RAG Citation Faithfulness`。
- `benchmarks/`: eval 方法、数据集、指标、适用场景。
- `people/`: 高信号研究者/工程师/团队的来源地图。
- `weekly-digests/`: 每周综合，不只是列表，要写“本周有什么趋势变化”。

## GitHub 能力分工

### 发现候选仓库和主题

用 GitHub Repository Search / Code Search / PR Search 做低频探索：

- 仓库发现：`topic:llm`, `topic:rag`, `topic:agents`, `topic:inference`, `topic:diffusion`, `topic:vector-database`, `stars:>100`, `pushed:>2026-01-01`, `archived:false`。
- PR 发现：`is:pr is:merged merged:>=2026-04-01 (rag OR agent OR inference OR eval OR tokenizer)`。
- 代码发现：`"class Agent" language:python`, `"tool_call" language:typescript`, `"speculative decoding"`, `"vector store"`, `path:evals`, `path:benchmarks`, `path:examples`。

使用限制要写进 skill：

- REST Search 每次搜索最多返回 1,000 个结果。
- REST Search 有独立限流；GitHub 文档当前页对 code search 的数字不完全一致，按最保守的约 9 requests/minute 设计节流。
- Code Search 主要用于“找线索”，不是完整抓取：默认分支、文件大小、仓库索引状态、fork/archived repo 都会影响结果。

### 已知仓库的增量采集

对高价值仓库建立 watchlist，用以下链路：

1. `push` webhook 收到 `before`、`after`、`ref`、`commits`、`compare`。
2. 若 push 中 commits 超出 payload 上限，继续用 Commits API 拉取缺失提交。
3. 用 Compare API 获取文件级变更、status、rename、patch。
4. 用 commit-associated PR API 找到引入该 commit 的 PR，再抓 PR body、review comments、changed files、linked issues。
5. 对每个候选 patch 只取必要上下文，避免把整个仓库灌进知识库。

GitHub 文档要点：

- `push` webhook 表示分支或标签发生 push，需要 GitHub App 至少有 Contents read 权限；payload 的 `commits` 最多 2048 个，超出后要用 Commits API 补。
- List commits 支持 `sha`、`path`、`since`、`until`、`per_page`，适合按仓库做增量补偿。
- Compare API 会返回文件变更状态、rename 前文件名和 patch，适合知识抽取。
- Get commit 支持获取单个 commit 的文件变更；默认 JSON 下超过 300 个文件会分页，最多到 3000 文件。

### 全网事件流

如果目标是“实时看到一大波大家在提交什么”，首选 GH Archive：

- GH Archive 把公共 GitHub timeline 聚合成每小时一个 JSON gzip 文件。
- BigQuery 版本每小时更新，适合用 SQL 筛选近实时趋势。
- 先在事件层筛选 PushEvent / PullRequestEvent / WatchEvent / ReleaseEvent，再对高分候选调用 GitHub API 补 patch 和 PR 语境。

建议筛选逻辑：

- repo topics / README / description 命中 AI 词表。
- push commit message 命中模型、agent、RAG、eval、inference、serving、quantization、scheduler、memory、retrieval 等词。
- changed path 命中 `agent`, `agents`, `llm`, `rag`, `retriever`, `vector`, `embedding`, `eval`, `benchmark`, `inference`, `serving`, `model`, `tokenizer`, `tool`, `memory`, `planner`。
- repo 热度分：stars、forks、近期 watch、PR 评论数、release 活跃度。
- 排除噪声：lockfile-only、vendor、generated、format-only、大规模迁移但无架构含量。

### 论文和 Gist 更新流

论文不要只靠 Gist 搜索。Gist 更适合发现“人的 curated list / idea file / reading notes”，论文元数据主干应来自 arXiv、Semantic Scholar、OpenAlex，Gist 作为补充信号：

1. arXiv：按 `cs.AI`, `cs.CL`, `cs.LG`, `cs.CV`, `stat.ML` 和关键词查询，按 `submittedDate` 或 `lastUpdatedDate` 增量拉取。
2. Semantic Scholar：用 paper search / recommendations 从 seed papers 扩展引用网络、相似论文和高影响论文。
3. OpenAlex：用 works search 搜标题、摘要、fulltext 子集，并补 citation、作者、机构、主题。
4. Gist：用 public gists 的 `since` 增量列表抓最近更新，按 description、filename、markdown content、arXiv URL、paper title、关键词过滤；对高价值 Gist 追踪 revision。
5. Curator list：维护可信作者/研究者/工程师的 GitHub users、Gists、repos、stars、forks，作为高信号入口。

Gist 的现实限制：

- Gist API 可以列出最近更新的 public gists，并支持 `since`，但它不是高质量论文搜索引擎。
- API 内容可能截断；大文件要用 `raw_url` 或 clone gist。
- public gists 噪声极大，必须用强过滤和 curator allowlist。
- Gist 可以被编辑，必须保存 revision/version 和抓取时间。

## 知识库数据模型

最小可用 schema：

- `source_event`: GitHub event id、event type、repo、actor、created_at、raw payload url/hash。
- `commit`: repo、sha、parent sha、author/committer date、message、html url、associated PR。
- `patch_file`: sha、path、previous_path、status、additions、deletions、patch、language。
- `paper`: title、authors、venue/source、arxiv id、doi、semantic scholar id、openalex id、published/updated、abstract、pdf url、code url。
- `gist_source`: gist id、owner、description、files、updated_at、revision、raw urls、matched keywords、paper links。
- `knowledge_card`: title、problem、solution_pattern、architecture_idea、implementation_notes、tradeoffs、failure_mode、tests_or_eval、tags、confidence、source_links。
- `entity`: repo、library、model、framework、pattern、API、file path、symbol。
- `embedding_chunk`: chunk text、embedding、source id、license/provenance。

搜索层建议：

- 全文：BM25/OpenSearch/Meilisearch/SQLite FTS，用于精确查函数名、文件名、错误信息。
- 向量：pgvector/Qdrant/LanceDB，用于查“类似设计思路”和“解决过类似问题的提交”。
- 图关系：轻量先用表表达边，后面再上 Neo4j/GraphRAG。边包括 `commit -> PR -> issue`, `patch -> symbol`, `card -> pattern`, `repo -> dependency`。

## Skill 工作流草案

skill 名称建议：`github-ai-knowledge-miner`

触发描述建议：

> Use when the user wants to mine GitHub commits, pull requests, diffs, or public GitHub activity for AI-related architecture ideas, implementation patterns, bug fixes, evaluations, or reusable engineering knowledge, and store the result in a searchable knowledge base.

SKILL.md 应只放高频流程，详细查询模板和 schema 放 `references/`：

1. 确认采集范围：watchlist repos、topic/keyword seed、时间窗、语言、是否只要 merged PR、许可和隐私边界。
2. 发现阶段：优先用 repo/PR/code search 找 seed；全网趋势用 GH Archive/BigQuery。
3. 增量阶段：对 watchlist 用 webhook/commits/compare，避免高频 polling。
4. 补全阶段：对高分候选拉 commit、PR、review comments、linked issues、release notes。
5. 过滤阶段：排除生成文件、依赖锁、格式化、低信号提交。
6. 抽取阶段：把 patch + PR 语境转成 `knowledge_card`，必须保留 source link、repo、sha、license/provenance。
7. 入库阶段：原始数据不可丢；结构化卡片、全文索引、向量索引分层存。
8. 回答阶段：所有结论带来源链接；区分“源码事实”和“模型推断”。

扩展后的 skill 可以支持四种 ingest：

- `ingest_repo_update`: commit/PR/release/diff -> 工程实现卡片。
- `ingest_paper`: arXiv/Semantic Scholar/OpenAlex -> 论文卡片、方法卡片、可复现线索。
- `ingest_gist`: idea file/reading list/code snippet -> curator note、相关论文、相关 repo。
- `ingest_query`: 用户的一次问题或探索 -> 把高价值综合答案写回 wiki。

## 抽取模板

每个高价值提交生成一张卡片：

```yaml
title:
source:
repo:
commit_sha:
pr:
date:
paths:
problem:
solution_pattern:
architecture_idea:
implementation_notes:
tradeoffs:
failure_modes:
tests_or_eval:
reusable_snippet_summary:
tags:
confidence:
why_it_matters:
```

抽取提示词核心：

> Read the patch and surrounding PR context. Extract only reusable engineering knowledge. Do not summarize file-by-file. Identify the problem solved, the architecture or implementation pattern introduced, important tradeoffs, tests/evals, and when this pattern should or should not be reused. Keep exact source links.

论文抽取模板：

```yaml
title:
source:
paper_ids:
date:
authors:
problem:
core_idea:
method:
architecture:
experiments:
limitations:
code_or_reproduction:
related_repos:
related_cards:
tags:
why_it_matters_for_agents:
confidence:
```

Gist/curator 抽取模板：

```yaml
title:
source_gist:
owner:
revision:
date:
curator_claim:
linked_papers:
linked_repos:
implementation_hints:
open_questions:
cards_to_update:
tags:
confidence:
```

## 推荐实施顺序

第一阶段：本地原型

- 维护 `watchlist.yaml`：20-50 个 AI 相关仓库和关键词。
- 每小时跑一次增量：GitHub commits API + compare API。
- 存 SQLite/DuckDB + 本地 markdown cards。
- 用 ripgrep/SQLite FTS 做关键词搜索，先证明抽取质量。

第二阶段：近实时

- 做 GitHub App，订阅 watchlist 的 push/pull_request/pull_request_review_comment/release。
- 加队列和去重：`repo + sha + path`。
- 接入向量库，做 hybrid search。

第三阶段：全网趋势

- 接 GH Archive hourly gzip 或 BigQuery。
- 先事件层筛选，再 GitHub API hydration。
- 做每日/每周 digest：新架构、新 bug fix、新 benchmark、新工具链变化。

第四阶段：AI research wiki

- 每天抓 arXiv/Semantic Scholar/OpenAlex 的新论文和更新论文。
- 每天抓 public Gist 增量，但只保留高分内容。
- 对论文卡片和代码卡片做双向链接：`paper -> official code -> third-party implementation -> benchmark/eval -> bug fixes`。
- 每周做一次 synthesis：本周值得跟进的 paper、对应代码、可借鉴架构、可直接复用 repo、仍未解决的问题。

## 风险和边界

- 版权和许可：保存 patch 和代码片段前记录 license；对外回答优先摘要，不输出长段原文。
- API 限流：Search 只用于发现；增量采集靠 webhook 和低频补偿任务。
- 噪声巨大：必须做路径、文件类型、diff 类型、PR 文本、测试变化的多层评分。
- 语义误判：knowledge card 里必须标 `confidence`，并区分源码事实与推断。
- 供应链安全：不要自动执行抓到的代码，只做静态分析和摘要。

## 资料来源

- GitHub REST Search API: https://docs.github.com/en/rest/search/search
- GitHub Code Search syntax: https://docs.github.com/en/search-github/github-code-search/understanding-github-code-search-syntax
- GitHub legacy code search limitations: https://docs.github.com/en/search-github/searching-on-github/searching-code
- GitHub commit search qualifiers: https://docs.github.com/en/search-github/searching-on-github/searching-commits
- GitHub webhook push payload: https://docs.github.com/en/webhooks/webhook-events-and-payloads
- GitHub REST Commits API: https://docs.github.com/en/enterprise-cloud@latest/rest/commits/commits
- GitHub REST Pull Requests API: https://docs.github.com/en/rest/pulls/pulls
- GitHub REST Gists API: https://docs.github.com/en/rest/gists/gists
- GitHub GraphQL pagination and limits: https://docs.github.com/en/graphql/guides/using-pagination-in-the-graphql-api
- GitHub GraphQL rate/query limits: https://docs.github.com/en/graphql/overview/rate-limits-and-query-limits-for-the-graphql-api
- GH Archive: https://www.gharchive.org/
- BigQuery public datasets: https://docs.cloud.google.com/bigquery/public-data
- arXiv API user manual: https://info.arxiv.org/help/api/user-manual.html
- Semantic Scholar Academic Graph API: https://www.semanticscholar.org/product/api
- OpenAlex API search: https://developers.openalex.org/guides/searching
- Karpathy LLM Wiki gist: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
