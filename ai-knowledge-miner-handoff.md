# AI Knowledge Miner Handoff

日期：2026-05-05  
工作目录：`/Users/macmini/Documents/Codex`

## 这次工作的目标

把“定期搜索 GitHub 仓库更新、Gist、AI 前沿论文，并沉淀成 agent 可快速搜索和理解的知识库”包装成一个可迭代的 Codex skill。

最终方向不是做一个资料收藏夹，而是做一个持续更新的 AI 研发 wiki：

- 能从论文、PR、commit、issue、release、Gist、curator notes 中发现高价值内容。
- 能把内容抽取成 agent 可读的结构化知识卡片。
- 能把论文和代码双向链接：`paper -> official code -> third-party implementation -> benchmark/eval -> bug fixes`。
- 能支持后续按问题、架构模式、代码实现、论文依据快速搜索。
- 能标注每篇 wiki 是否被用户读过。

## 核心判断

### 1. 不要把 GitHub Search 当实时主数据源

GitHub Search 适合发现候选仓库、候选 PR、关键词和主题，但不适合作为实时抓取主链路。

推荐分工：

- 已知高价值仓库：GitHub App webhook + commits/compare/PR files API。
- 全网趋势：GH Archive hourly events 或 BigQuery 公共数据集。
- 论文主干：arXiv、Semantic Scholar、OpenAlex。
- Gist：用作 curator signal，适合发现人的 reading list、idea file、短评和实现笔记，不适合作为论文主数据库。

### 2. 知识库不应全量收录

只收以后能帮助 agent 做研发判断的内容：

- 可以借鉴的架构。
- 可以复用的实现模式。
- 可以参考的论文方法。
- 真实 bug fix 和故障复盘。
- eval、benchmark、测试方法。
- maintainer 或 researcher 的高信号解释。

默认拒绝：

- README-only、格式化、依赖锁、生成文件。
- 标题党论文或无方法细节内容。
- 无来源、无链接、无可验证信息的观点。
- 大而全但没有人工判断的资源列表。

### 3. 使用质量评分控制 token 和噪声

每个候选来源计算 0-10 分，默认 6 分以上才写入 wiki：

- 相关性：0-2。
- 新颖性：0-2。
- 可复用性：0-2。
- 可验证性：0-2。
- 影响信号：0-1。
- 信息密度：0-1。

## Token 成本判断

主要 token 消耗不在 API 搜索，而在深读和综合：

- 论文全文：一篇通常 8k-30k tokens。
- 大 PR/diff/review：常见 10k-100k tokens。
- Gist/curated list：取决于长度和逐项判断成本。
- 更新 wiki：需要读取相关旧页面，若没有索引会很贵。
- digest/趋势总结：贵在前置阅读量。

推荐四级漏斗：

1. 规则和元数据筛选，尽量不用 LLM。
2. 轻量 LLM 打分，只给标题、摘要、路径、diffstat、message。
3. 深读高分候选。
4. 只读取相关 wiki 页面并更新。

健康的每日任务预算大致：

- 轻筛 50-150 条：30k-100k tokens。
- 深读 5-15 条：80k-300k tokens。
- 生成/更新 5-20 张卡片：30k-100k tokens。
- digest：10k-50k tokens。

每日总量建议控制在 150k-500k tokens。小时级任务控制在 10k-50k tokens。

## Skill 包设计

已创建 skill：`github-ai-knowledge-miner`

触发用途：

> 当用户希望把 GitHub commits、PR、release、issue、gists、arXiv papers、Semantic Scholar/OpenAlex metadata 或 AI 工程笔记沉淀成可搜索的 AI research/engineering wiki 时使用。

支持四种 ingest：

- `ingest_repo_update`: commit / PR / release / diff -> 工程实现卡片。
- `ingest_paper`: arXiv / Semantic Scholar / OpenAlex -> 论文卡片、方法卡片、可复现线索。
- `ingest_gist`: idea file / reading list / code snippet -> curator note、相关论文、相关 repo。
- `ingest_query`: 用户自己的问题和探索 -> 写回 wiki，作为后续可复用知识。

## Wiki 页面类型

建议 wiki 结构：

```text
raw/
  papers/
  github/
  gists/
  web/
wiki/
  index.md
  papers/
  repos/
  patterns/
  problems/
  benchmarks/
  people/
  weekly-digests/
  queries/
```

页面类型：

- `papers/`: 单篇论文卡片。
- `repos/`: 仓库画像、关键模块、可借鉴点。
- `patterns/`: 可复用架构模式。
- `problems/`: 问题导向页面。
- `benchmarks/`: eval 方法、数据集、指标、适用场景。
- `people/`: 高信号研究者、工程师、团队。
- `weekly-digests/`: 每周综合判断。
- `queries/`: 用户问题和探索结果。

## 已读状态设计

用户要求 wiki 需要标注“有没有读过这篇 wiki”。已加入强制 frontmatter 规则：

```yaml
read_status: unread
read_by_user: false
read_at:
tags:
  - read:unread
```

允许状态：

- `unread`: 新建或更新后用户未读。
- `skimmed`: 用户略读过。
- `read`: 用户已读，当前页面稳定。
- `needs_reread`: 用户以前读过，但页面发生了实质更新。

维护规则：

- 用户说已读：设为 `read`，`read_by_user: true`，填 `read_at`。
- 用户说略读：设为 `skimmed`。
- 已读页面出现新结论、新证据、新代码链接、新风险或结论变化：设为 `needs_reread`。
- 仅修 typo 或格式，不必改状态。

## 当前已生成文件

### 交接文档

- `/Users/macmini/Documents/Codex/ai-knowledge-miner-handoff.md`

### 调研笔记

- `/Users/macmini/Documents/Codex/github-ai-commit-knowledge-skill-notes.md`

内容包含：

- GitHub API / Gist / arXiv / Semantic Scholar / OpenAlex 的采集策略。
- GitHub 增量更新和全网趋势方案。
- 论文、Gist、代码知识库 schema。
- 收录策略、质量评分、token 成本判断。
- 后续 skill 工作流草案。

### Codex Skill 包

- `/Users/macmini/Documents/Codex/github-ai-knowledge-miner/`

文件：

- `/Users/macmini/Documents/Codex/github-ai-knowledge-miner/SKILL.md`
- `/Users/macmini/Documents/Codex/github-ai-knowledge-miner/agents/openai.yaml`
- `/Users/macmini/Documents/Codex/github-ai-knowledge-miner/references/collection-policy.md`
- `/Users/macmini/Documents/Codex/github-ai-knowledge-miner/references/query-playbooks.md`
- `/Users/macmini/Documents/Codex/github-ai-knowledge-miner/references/schemas.md`
- `/Users/macmini/Documents/Codex/github-ai-knowledge-miner/references/wiki-maintenance.md`

### Git 仓库存档

- Git 仓库根目录：`/Users/macmini/Documents/Codex`
- Git 元数据目录：`/Users/macmini/Documents/Codex/.git`
- 当前分支：`main`
- 已有提交：`5ea6b18 Add GitHub AI knowledge miner skill`
- 忽略文件：`/Users/macmini/Documents/Codex/.gitignore`

当前 GitHub 同步状态：

- 本地仓库已初始化并完成一次提交。
- 还没有配置 `origin` remote。
- 之前 GitHub 连接器没有列出可访问仓库，本机也没有 `gh` CLI。
- 后续要同步到 GitHub，需要提供目标仓库，例如 `owner/repo` 或 remote URL。

## 后续开发建议

### 第一阶段：让 skill 真正可安装和可调用

- 把 `github-ai-knowledge-miner/` 放入目标 skills 仓库。
- 在 Codex 中安装或指向该 skill。
- 用几个真实问题测试触发，例如：
  - “用这个 skill 收集过去一周 agent memory 相关论文和 GitHub 实现。”
  - “帮我找最近 RAG citation faithfulness 的论文和实现。”
  - “把这个 PR 抽取成 wiki card。”

### 第二阶段：做本地 wiki 原型

- 建 `raw/` 和 `wiki/` 目录。
- 用 10-20 个手工 seed sources 测试 schema。
- 先用 markdown + ripgrep 搜索，不急着上数据库。
- 验证 `read_status` 和 `needs_reread` 是否好用。

### 第三阶段：做自动抓取

- `watchlist.yaml`: 维护重点 repos、people、keywords、paper queries。
- 每日 arXiv / Semantic Scholar / OpenAlex 论文候选抓取。
- 每日 trusted gists 抓取。
- 每小时或每日 GitHub repo 增量抓取。
- 只让 LLM 深读高分候选。

### 第四阶段：升级搜索

- 全文搜索：SQLite FTS / Meilisearch / OpenSearch。
- 向量搜索：pgvector / Qdrant / LanceDB。
- 关系索引：paper、repo、pattern、problem、benchmark、person 之间的 links。

## 重要设计原则

- Raw source 和 wiki synthesis 分开保存。
- 所有结论保留 source link 和 provenance。
- 区分源码事实、论文事实和模型推断。
- 不自动执行抓到的代码。
- 代码 license、论文 license、Gist revision 都要保留。
- 每次更新页面时维护 `read_status`。
- 不追求“抓全”，追求“以后能帮 agent 判断”。

