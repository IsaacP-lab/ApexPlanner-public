---
id: ADR-PUB-002
title: SQLite with WAL Mode for Local State
status: Accepted
date: 2026-05-10
supersedes: null
superseded_by: null
---

# ADR-PUB-002 — SQLite with WAL Mode for Local State

## Context

The engine maintains a **Skeletal Circuit**: nodes (files / symbols), edges (imports and related relations), access telemetry, and optional intent locks. That state must survive process restarts, concurrent readers (MCP tool calls, CLI inspection), and bursty writes during full-workspace indexing.

Alternatives considered at a high level:

- **Embedded key-value stores** — fast, but weaker **relational integrity** for multi-table invariants (nodes vs edges vs logs).
- **Process-memory only** — low latency, no durability; unacceptable for audit and cold-start reuse.
- **Remote OLTP** — operational overhead and latency variance; conflicts with local-first sovereignty goals.

## Decision

**SQLite** is the system of record for local ApexPlanner state. The deployment uses **WAL (Write-Ahead Logging)** journal mode so that:

- **Readers** (tool calls) do not block the **writer** (indexer) as aggressively as rollback journaling.
- **Crash recovery** remains **ACID**-backed: committed transactions survive process termination within SQLite’s guarantees.
- **Filesystem-backed** deployment stays **single-binary** friendly without a separate database service.

Schema evolution is **migration-driven**: every change to persisted tables ships as a new numbered migration; historical migrations are immutable once merged.

## Consequences

### Positive

- **ACID transactions** align edges, nodes, and logs for a consistent graph view.
- **WAL** improves responsiveness under **heavy I/O** during large TypeScript workspace ingestion.
- **Portable** artifacts: one file (or predictable file set) per workspace database.

### Negative

- Very high **write** concurrency from multiple writers is not the sweet spot; the engine assumes a **single primary writer** with concurrent readers.
- NFS or exotic network filesystems may require operational care (documented at deployment time).

## Status

**Accepted.**

## Related (full ApexPlanner repository)

- Migration discipline: `docs/adr/0003-migration-tool.md`, `docs/adr/0012-deterministic-migrations.md`
- Schema reference: `docs/schema.md`
