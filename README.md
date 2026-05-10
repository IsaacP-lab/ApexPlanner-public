# ApexPlanner: Deterministic Context Engineering for the AI Era

**Bundle:** This directory is a **self-contained export** of public architectural evidence (overview, benchmarks, governance summary, two ADRs, sample MCP tool schema). Zip or copy **this folder only** for portfolio, audit, or partner review.

ApexPlanner is a **local-first** engine that compresses repository context into a **token-bounded architectural skeleton** and a **deterministic dependency graph**, delivered to agents over the Model Context Protocol (MCP). The design prioritizes repeatable structure, ACID-backed state, and measurable latency—not probabilistic retrieval.

## The Problem: Context Rot

Large language models operating on raw trees of source files exhibit **context rot**: token budgets fill with redundant or unstable text, dependency relationships are implied rather than enumerated, and small refactors produce outsized diffs in what the model “remembers.” Vector retrieval compounds the issue by trading **precision** for **recall**: similar text is not the same as a correct import edge or a stable public API surface.

ApexPlanner addresses rot by emitting **normalized, deterministic projections** (AST-level skeletons and explicit edges) so agents consume **architecture**, not a sample of files.

## The Solution: Local-First Rust Engine

The runtime is a **Rust** workspace with a single `apex` binary. Indexing uses **tree-sitter**; persistence uses **SQLite** with migrations and WAL mode. Skeletons and hashes are produced under fixed normalization rules so the same inputs yield the same stored artifacts—supporting audit, drift detection, and reproducible tooling without a network round-trip to a hosted index.

- **Transport:** stdio JSON-RPC 2.0 MCP (normative wire and tool catalog: full repository `docs/mcp-contract.md`).
- **State:** relational store for nodes, edges, access telemetry, and intent baselines (full repository `docs/schema.md`).
- **Evidence:** architectural trade-offs as ADRs; this bundle includes two public summaries under [`docs/adr/`](docs/adr/).

## Key Benchmarks

Published envelopes and measurement notes: [`BENCHMARKS.md`](BENCHMARKS.md). Summary:

| Envelope | Target |
| -------- | ------ |
| Cold boot to ready | \< 100 ms (p95, local SSD, warm OS cache) |
| Resident set (idle indexer) | \< 30 MB |
| AST normalization throughput | ≥ 5,000 TypeScript files / minute (single workspace, bounded file size) |

## The “Sovereign” Stance: HIPB Compliance

ApexPlanner is engineered around a **Hermetic IP Boundary (HIPB)**: proprietary or sensitive implementation detail stays on disk under your control; the MCP surface exposes **schemas, skeletons, hashes, and governance metadata** suitable for agents and auditors—not a dump of raw corpus into a third-party vector store. Combined with path-block rules and redaction of secret-shaped tokens in tool output, the stance aligns with **human-in-the-loop (HITL)** operation and local auditability rather than **send-it-all** ingestion patterns.

HIPB and HITL requirements are summarized in [`docs/governance.md`](docs/governance.md). The normative R1–R8 standard in full is in the ApexPlanner repository at `docs/governance-protocol.md`.

## Architectural Integrity

Load-bearing decisions are **why-first** documents: **Architecture Decision Records (ADRs)**. This bundle includes:

- [`docs/adr/ADR-001-deterministic-graphing-vs-vector-rag.md`](docs/adr/ADR-001-deterministic-graphing-vs-vector-rag.md) — deterministic graph vs vector RAG  
- [`docs/adr/ADR-002-sqlite-wal-mode-persistence.md`](docs/adr/ADR-002-sqlite-wal-mode-persistence.md) — SQLite + WAL for local state  

The full ADR index, IP baseline (`0000-system-genesis`), and implementation ADRs live in the complete ApexPlanner checkout under `docs/adr/`.

## MCP Handshake (Sample)

Sample tool descriptors (JSON Schema with embedded example): [`interface/mcp-schema.json`](interface/mcp-schema.json).

## Full Product Checkout

Build instructions, MCP wiring examples, contributor rules (`AGENTS.md`), and the complete doc set are **not** duplicated here; clone or obtain the full **ApexPlanner** repository for those artifacts.

## License

MIT OR Apache-2.0 (see full repository license files when present).
