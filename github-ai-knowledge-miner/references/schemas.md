# Schemas

## Required Frontmatter For Every Wiki Page

```yaml
title:
page_type:
read_status: unread
read_by_user: false
read_at:
created_at:
updated_at:
source_urls: []
provenance: []
confidence:
status: accepted
tags:
  - read:unread
```

Allowed `page_type` values:

- `paper`
- `repo`
- `pattern`
- `problem`
- `benchmark`
- `person`
- `digest`
- `query`

Allowed `read_status` values:

- `unread`
- `skimmed`
- `read`
- `needs_reread`

When `read_status` changes, keep `tags` synchronized:

- `unread` -> `read:unread`
- `skimmed` -> `read:skimmed`
- `read` -> `read:read`
- `needs_reread` -> `read:needs-reread`

## Paper Card

```yaml
title:
page_type: paper
read_status: unread
read_by_user: false
read_at:
created_at:
updated_at:
source_urls: []
provenance:
  arxiv_id:
  doi:
  semantic_scholar_id:
  openalex_id:
authors: []
published_at:
updated_source_at:
confidence:
status: accepted
tags:
  - read:unread
```

Body sections:

- Problem
- Core Idea
- Method
- Architecture
- Experiments
- Limitations
- Code Or Reproduction
- Agent Relevance
- Related Papers
- Related Repos
- Open Questions

## Repo Or Code Change Card

```yaml
title:
page_type: repo
read_status: unread
read_by_user: false
read_at:
created_at:
updated_at:
source_urls: []
provenance:
  repo:
  commit_sha:
  pr_number:
  issue_number:
license:
confidence:
status: accepted
tags:
  - read:unread
```

Body sections:

- Problem Background
- Change Summary
- Architecture Idea
- Key Files And Symbols
- Reusable Pattern
- Tradeoffs
- Tests Or Eval Evidence
- Related Papers Or Issues
- When To Reuse
- When Not To Reuse

## Gist Or Curator Card

```yaml
title:
page_type: query
read_status: unread
read_by_user: false
read_at:
created_at:
updated_at:
source_urls: []
provenance:
  gist_id:
  gist_revision:
owner:
confidence:
status: accepted
tags:
  - read:unread
```

Body sections:

- Curator Claim
- Why This Source Matters
- Linked Papers
- Linked Repos
- Implementation Hints
- Claims To Verify
- Cards To Create Or Update

## Pattern Page

```yaml
title:
page_type: pattern
read_status: unread
read_by_user: false
read_at:
created_at:
updated_at:
source_urls: []
confidence:
status: accepted
tags:
  - read:unread
```

Body sections:

- Pattern
- Problem It Solves
- Known Implementations
- Papers Behind It
- Design Tradeoffs
- Failure Modes
- Minimal Implementation Sketch
- Evaluation Checklist
- Open Questions

## Digest Page

Digest pages should be short and judgment-heavy. They must not be a raw list.

Required sections:

- Top Changes
- New Papers Worth Reading
- Code Worth Borrowing
- Patterns Emerging
- Risks Or Contradictions
- Pages Marked Needs Reread
- Next Watchlist Updates
