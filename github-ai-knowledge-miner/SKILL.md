---
name: github-ai-knowledge-miner
description: Use when mining GitHub commits, pull requests, releases, issues, gists, arXiv papers, Semantic Scholar/OpenAlex metadata, or curated AI engineering notes into a searchable AI research and engineering wiki. Use for collecting agent architecture ideas, RAG patterns, eval methods, implementation changes, bug-fix lessons, paper-to-code links, and reusable knowledge cards.
---

# GitHub AI Knowledge Miner

## Purpose

Use this skill to turn AI research and GitHub activity into a durable knowledge graph that an agent can search, understand, and update. The goal is not to archive everything; it is to keep high-signal engineering knowledge with source links, reusable patterns, explicit read-state metadata, bilingual presentation when requested, and reciprocal links among concept nodes.

## Workflow

1. Define the collection scope: user goal, time window, watched repos, trusted people, keywords, target AI areas, and license/privacy constraints.
2. Search existing wiki pages by title, aliases, source URLs, paper ids, PR numbers, technologies, pain points, and patterns before adding anything.
3. Gather candidates from metadata first: titles, abstracts, repo metadata, PR titles, commit messages, changed paths, gist descriptions, and curator identity.
4. Score candidates before deep reading. Only promote sources that pass the collection threshold and add something new to the graph.
5. Deep-read accepted candidates: paper text, PR context, diff patches, issue discussion, release notes, or gist revisions.
6. Extract concise knowledge nodes with provenance, source links, confidence, `read_status`, bilingual English/Chinese presentation when requested, and explicit graph edges.
7. Update existing paper, repo, pattern, problem, benchmark, people, and digest pages before creating new pages. Maintain reciprocal Markdown links among related pages so the wiki forms a navigable local graph.
8. Preserve raw sources separately from synthesized wiki pages. Do not overwrite source facts with model inference.

## Collection Rules

Default threshold: write to the wiki only when a candidate scores at least 6/10.

Score dimensions:

- Relevance: 0-2, close to the user's agent or AI engineering interests.
- Novelty: 0-2, new architecture, method, implementation, benchmark, or failure lesson.
- Reusability: 0-2, can become a design pattern, checklist, code lead, or implementation plan.
- Verifiability: 0-2, has paper, code, PR, benchmark, issue, or maintainer evidence.
- Impact signal: 0-1, strong author, repo, citations, stars, adoption, or discussion quality.
- Information density: 0-1, contains judgment beyond a generic summary.

Do not promote low-signal items by keyword alone. Usually reject README-only changes, dependency lock updates, generated files, formatting-only commits, vague paper abstracts, unsourced claims, and large resource lists without curation.

For detailed collection policy, read `references/collection-policy.md`.

## Required Wiki Read State

Every wiki page must include a read-state tag in frontmatter:

```yaml
read_status: unread
read_by_user: false
read_at:
tags:
  - read:unread
```

Allowed `read_status` values:

- `unread`: created or updated but not read by the user.
- `skimmed`: user skimmed it but has not fully reviewed it.
- `read`: user has read it and the page is stable enough for later retrieval.
- `needs_reread`: page changed materially after the user previously read it.

When updating a page that was already `read`, change it to `needs_reread` if the update changes claims, recommendations, links, or conclusions. Minor typo or formatting edits may keep the previous state.

## Source Types

Use these ingest modes:

- `ingest_repo_update`: commits, PRs, releases, issues, reviews, diffs.
- `ingest_paper`: arXiv, Semantic Scholar, OpenAlex, PDFs, paper code links.
- `ingest_gist`: public gists, curator lists, idea files, reading notes.
- `ingest_query`: the user's own question, exploration, or synthesis that should be written back into the wiki.

## Output Pages

Prefer durable graph nodes over one-off summaries. When the user requests bilingual output, include Chinese in the frontmatter `title` and write pages with English/Chinese paired headings or paired sentences. Keep source facts unchanged and avoid introducing extra inference in the translation.

- `papers/`: one paper per page.
- `repos/`: repository profiles and implementation leads.
- `patterns/`: reusable designs such as memory compaction, tool retry loops, hybrid RAG, or eval harnesses.
- `problems/`: problem-first pages such as context drift, unreliable tool calls, citation faithfulness, or agent regression testing.
- `benchmarks/`: eval methods, datasets, metrics, and when to use them.
- `people/`: high-signal authors, maintainers, labs, or engineering teams.
- `weekly-digests/`: concise synthesis of what changed and what matters.

## Extraction

Use concise structured extraction. Do not summarize file-by-file unless the user asks. Prefer this node shape:

- `Knowledge Point / 知识点`: one sentence.
- `Pain Point / 痛点`: what problem exists.
- `Method / 方法`: what method solves it.
- `Technology / 技术`: frameworks, algorithms, runtime mechanisms, protocols, or tooling.
- `Solves / 解决了什么`: concrete solved problem or capability.
- `Graph Edges / 图谱边`: reciprocal links to source, pattern, problem, repo, paper, and digest nodes.

For bilingual pages, keep the same structure in both languages and translate the synthesis rather than the source facts.

For schemas and card templates, read `references/schemas.md`.

For GitHub, gist, paper, and query search playbooks, read `references/query-playbooks.md`.

For wiki update procedure, read-state maintenance, and reciprocal link graph rules, read `references/wiki-maintenance.md`.

## Answering From The Wiki

When answering the user, prioritize source-backed statements. Distinguish source facts from inference. Link to source URLs and wiki pages when available. If a page has `read_status: unread` or `needs_reread`, mention that it has not yet been user-reviewed when the distinction matters. If the user requested bilingual wiki pages, preserve the English/Chinese pairing in future updates unless they ask to switch back.
