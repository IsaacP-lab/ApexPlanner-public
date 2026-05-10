---
id: ADR-PUB-001
title: Deterministic Skeletal Graph vs Generic Vector RAG
status: Accepted
date: 2026-05-10
supersedes: null
superseded_by: null
---

# ADR-PUB-001 — Deterministic Skeletal Graph vs Generic Vector RAG

## Context

Agent workflows often attach a **vector database** or hosted “code search” service to retrieve chunks of source by embedding similarity. That pattern optimizes for **approximate recall** over heterogeneous corpora. It does not, by itself, guarantee:

- **Exact** import / export relationships between compilation units
- **Stable** identity for symbols across refactors when chunk boundaries shift
- **Reproducible** answers for the same repository state (embedding models, chunkers, and indexes change)

For **architectural evidence**—what depends on what, what is public API vs internal, what changed relative to a locked intent baseline—**precision** and **auditability** dominate **fuzzy relevance**.

## Decision

ApexPlanner uses a **deterministic, SQLite-backed skeletal graph** derived from parse trees and explicit workspace rules. Structural nodes and dependency edges are materialized in relational form. **BLAKE3** hashes of **normalized AST projections** anchor drift and locking semantics: the same normalized skeleton yields the same hash; intentional refactors are distinguishable from accidental skew.

**Vector RAG** is **out of scope** for the primary truth plane. If semantic similarity is introduced later, it must be **downstream** of the deterministic graph (e.g., labeling or ranking), never a replacement for enumerated edges.

## Consequences

### Positive

- **100% precision** for dependency mapping as modeled (imports / declared edges)—no “nearby text” substitutes for a missing edge.
- **Deterministic** replays: identical inputs and normalization rules reproduce hashes and graph snapshots.
- **Local-first**: no mandatory network dependency for indexing or retrieval.

### Negative

- Up-front engineering cost for parsers, normalization policy, and migrations.
- “Fuzzy” questions (“something like this pattern”) are not first-class without an auxiliary retrieval layer.

## Status

**Accepted.**

## Related (full ApexPlanner repository)

- Normalization and hashing: `docs/adr/0002-blake3-normalization.md`
- MCP contract (tool surfaces): `docs/mcp-contract.md`
