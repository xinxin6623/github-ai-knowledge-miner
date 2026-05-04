# Query Playbooks

## GitHub Repository Discovery

Use repository search for seed discovery:

```text
topic:llm topic:agents stars:>100 pushed:>2026-01-01 archived:false
topic:rag stars:>100 pushed:>2026-01-01 archived:false
topic:inference stars:>100 pushed:>2026-01-01 archived:false
topic:evals stars:>50 pushed:>2026-01-01 archived:false
```

Useful topics:

- `llm`
- `agents`
- `rag`
- `inference`
- `evals`
- `vector-database`
- `mcp`
- `tool-use`
- `benchmarks`

## GitHub PR And Commit Discovery

Search for high-signal changes:

```text
is:pr is:merged merged:>=YYYY-MM-DD (agent OR memory OR planner OR eval OR retrieval OR rerank)
is:pr is:merged merged:>=YYYY-MM-DD (inference OR cache OR batching OR quantization OR serving)
```

When inspecting a candidate PR, gather:

- PR title and body.
- Changed files and diffstat.
- Important patches only.
- Review comments and maintainer explanation.
- Linked issues.
- Tests, evals, and benchmark changes.

Ignore generated files, vendored code, lockfiles, and formatting-only patches unless the change is specifically about packaging or reproducibility.

## GitHub Incremental Watchlist

For watched repos:

1. Track default branch HEAD and last processed commit.
2. Use commits or compare API for changes since last run.
3. Score by commit message, changed path, diffstat, PR context, and test evidence.
4. Promote only high-score changes into cards.

High-signal changed paths:

```text
agent
agents
memory
planner
tool
tools
rag
retriever
rerank
embedding
eval
benchmark
inference
serving
model
tokenizer
mcp
```

## Paper Discovery

Use arXiv for new and updated paper metadata, then Semantic Scholar/OpenAlex for citation and related-work expansion.

High-signal categories:

- `cs.AI`
- `cs.CL`
- `cs.LG`
- `cs.CV`
- `stat.ML`

Useful search concepts:

```text
agent memory
tool use
planning
reflection
retrieval augmented generation
long context
code generation
automated software engineering
LLM evaluation
LLM judge
inference optimization
speculative decoding
```

Promote a paper when it has:

- Clear method.
- Evidence and limitations.
- Code or reproduction path.
- A direct relation to agent engineering.
- Links to existing or emerging implementations.

## Gist And Curator Discovery

Use Gist as a curator signal, not as a paper database.

Collect:

- Trusted users' public gists and gist revisions.
- Reading lists with comments and links.
- Idea files with implementation clues.
- Gists that link papers to code.

Screen by:

- Owner trust.
- Description and filenames.
- arXiv, DOI, GitHub repo, benchmark, or model links.
- Presence of short judgments or taxonomy.

Reject giant link dumps unless the author adds useful grouping or opinion.

## User Query Ingest

When the user explores an idea, treat high-value synthesis as a source:

1. Capture the question.
2. Link relevant paper, repo, pattern, and problem pages.
3. Record the decision, uncertainty, and next experiments.
4. Create or update a `query` page when the answer should be reusable later.
