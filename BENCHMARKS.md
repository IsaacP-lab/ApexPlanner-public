# ApexPlanner — Performance Envelopes

This document states **published performance envelopes** for the local engine. Figures are **targets or measured baselines** under stated conditions; they are not a substitute for profiling on your hardware.

## Methodology (normative for comparisons)

| Parameter | Value |
| --------- | ----- |
| Platform reference | x86-64 or ARM64, local NVMe SSD, default OS power profile |
| Cold boot | Process start until MCP **ready** (stdio handshake complete, redaction probe passed, DB migration check finished) |
| Memory | Resident set **after** idle settle post-index (no active full-workspace scan) |
| AST normalization | Wall-clock time to ingest **TypeScript / TSX** files into normalized skeleton projections for hashing; excludes first-time compiler download; files within documented size caps |

Reported percentiles use **p95** over ≥ 30 consecutive cold starts unless noted.

## Published envelopes

| Metric | Envelope | Notes |
| ------ | -------- | ----- |
| Cold boot latency | **\< 100 ms** (p95) | Local disk; excludes deliberate debug logging and antivirus cold-scan interference |
| Memory usage (idle) | **\< 30 MB** RSS | Single workspace; typical medium monorepo index resident |
| AST normalization throughput | **≥ 5,000 files / min** | TypeScript family; bounded per-file size per operations doc (full repo: `docs/operations.md`) |

## Drift and tail latency

Full-workspace cold scans and Git-adjacent operations are **tail-sensitive** (p95 / p99 grow with file count, depth, and concurrent readers). For SLA-style guarantees inside an organization, measure against a **fixed golden workspace** and pin toolchain versions.

## Related documentation (full ApexPlanner repository)

- Operations limits and honest scope: `docs/operations.md`
- MCP contract: `docs/mcp-contract.md`
- Governance token reporting: `docs/governance-protocol.md` (R5)
