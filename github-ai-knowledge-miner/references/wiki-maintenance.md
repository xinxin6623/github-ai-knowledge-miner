# Wiki Maintenance

## Directory Layout

Use this structure unless the user has an existing wiki layout:

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

Raw sources are immutable snapshots or source metadata. Wiki pages are synthesized and can be updated.

## Knowledge Graph Style

The wiki should grow as a graph of concise knowledge points, not as long reading notes. Each accepted page should answer:

- What pain point exists?
- What method solves it?
- What technology or framework is used?
- What concrete problem or capability is solved?
- Which existing nodes should this connect to?

Use this default node shape:

```text
## Knowledge Point / 知识点
## Pain Point / 痛点
## Method / 方法
## Technology / 技术
## Solves / 解决了什么
## Graph Edges / 图谱边
```

## Bilingual Page Style

When the user requests bilingual output, write wiki pages in English/Chinese pairs:

- Include Chinese in the frontmatter `title`.
- Use paired headings such as `## Top Changes / 本周重点变化`.
- Keep source facts, URLs, and provenance unchanged.
- Translate the synthesis, not the citation metadata.
- Prefer one concise bilingual line over duplicated long paragraphs when space matters.

## Read-State Rules

Every wiki page must include:

```yaml
read_status: unread
read_by_user: false
read_at:
tags:
  - read:unread
```

If the user says they have read a page:

- Set `read_status: read`.
- Set `read_by_user: true`.
- Set `read_at` to the current date or timestamp.
- Replace the read tag with `read:read`.

If the user skimmed it:

- Set `read_status: skimmed`.
- Set `read_by_user: false` unless the user explicitly treats skimmed as read.
- Replace the read tag with `read:skimmed`.

If a previously read page receives a meaningful update:

- Set `read_status: needs_reread`.
- Keep `read_by_user: true`.
- Keep old `read_at`.
- Replace the read tag with `read:needs-reread`.

Meaningful updates include changed claims, new caveats, new code links, new benchmark evidence, new conclusions, or contradictions. Minor typo, formatting, and link-cleanup edits do not require reread.

## Updating Pages

Before creating a new page:

1. Search existing pages by title, aliases, paper id, repo, PR, commit, and tags.
2. Search by pain point, method, technology, and graph-neighbor concepts.
3. Update an existing page when the new source strengthens or corrects it.
4. Add a new source edge to an existing concept node when the idea is not truly new.
5. Create a new page only when it introduces a distinct paper, repo, pattern, problem, benchmark, person, or reusable query.

When updating an existing page:

- Preserve source links and provenance.
- Add new evidence with dates.
- Separate facts from model inference.
- Update related pages.
- Mark read state according to the rules above.

## Link Graph Rules

Every accepted wiki page should participate in the local link graph:

- Link back to `../index.md` or `../../index.md` depending on page depth.
- Link to the weekly digest that introduced or updated the page.
- Convert related papers, repos, patterns, problems, and benchmarks into Markdown links, not plain text.
- Maintain reciprocal links when two pages materially reference each other.
- Prefer relative Markdown links so the wiki works in GitHub and local Markdown tools.
- Prefer connecting a source node to pattern/problem nodes over creating isolated source-only pages.

## Index Rules

Keep `wiki/index.md` concise. It should point to high-value pages rather than duplicate them.

Suggested sections:

- Current Focus
- Recently Added
- Needs Reread
- High-Value Patterns
- Open Problems
- Best Repos To Borrow From
- Papers Worth Reading Next

## Digest Rules

Weekly digests should answer:

- What changed this week?
- Which papers are worth reading?
- Which code is worth borrowing?
- Which patterns are emerging?
- What is overhyped or weakly supported?
- Which pages now need rereading?

Do not include low-score candidates in digest unless they explain a trend or caution.
