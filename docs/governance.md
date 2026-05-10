# Apex Core Governance Standard — R1–R8 (Summary)

**Normative detail:** the complete standard is `docs/governance-protocol.md` in the **full ApexPlanner repository** (document id `DOC-GOVERNANCE-PROTOCOL`).

This document is a **concise, public-facing** summary of the **R1–R8** operating rules. It exists so external readers can assess **Human-in-the-loop (HITL)** rigor and **Hermetic IP Boundary (HIPB)** posture without parsing the full procedural text.

## Purpose

The standard ties **strategic intent** (goals), **architectural decisions** (ADRs), and **operational evidence** (tasks, recaps, telemetry) into one traceable chain. It is not legal advice and not a regulatory submission format; it is a **disciplined engineering record**.

## R1 — Plan-First

Non-trivial work begins with a durable plan before execution. Triggers include load-bearing surface changes (schema, MCP wire, shared types, normalization policy), dependency changes, new or superseded ADRs, multi-crate refactors, and new **HITL-class** behaviors.

## R2 — Evidence-First

Work items marked complete carry **observable acceptance criteria**, **evidence** (commits, tests, artifacts), **references** (ADRs, doc IDs), **risks**, and **redaction** counts where applicable.

## R3 — Intent–Execution Commit Format

Commits link **Goal ID** and **Task ID** to the change set so history remains navigable by intent, not only by diff shape.

## R4 — Secrecy Pass (Two Marker Namespaces)

Outputs are scrubbed with explicit markers:

- Runtime tool payloads: `[REDACTED:<KIND>]` (category disclosed).
- Governance and recap exports: `[REDACTED_BY_APEX]` (human-facing, category collapsed).

Absolute paths, volatile process metadata, and blocked path classes are handled per the full standard.

## R5 — Token Report

Substantive recaps include token-density reporting against an agreed encoding baseline so efficiency claims remain **measurable**, not anecdotal.

## R6 — Session Recap

Substantive sessions produce dated recaps with decisions, portfolio-ready value statements, artifact linkage, and follow-ups—forming a **human-auditable** narrative.

## R7 — Path-Block List

Certain credential and secret paths are **never** read, parsed, or persisted. This is **defense in depth** alongside output redaction.

## R8 — HITL Policy

Tooling is classified by **Human-in-the-loop** requirements (read-only vs consent-gated mutation vs forbidden-in-MVP externals). Mutating classes record **consent_method** to the access log per **ADR-0008** (`docs/adr/0008-local-hitl-policy.md` in the full repository). **No silent bypass** of approved consent text for higher classes.

---

## Hermetic IP Boundary (HIPB)

**HIPB** means: **intellectual-property-bearing and implementation detail remain under the operator’s control**; the automation boundary exports **structured, intentional artifacts** (skeletons, graphs, hashes, schemas, governance metadata)—not wholesale corpus upload to opaque third-party indexes.

Practical implications:

- **Local-first** persistence and indexing are the default posture.
- **ADRs** and **goal linkage** document **meaningful human control** over load-bearing design choices.
- **Bootstrap and project metadata** for governance live in-repo per dedicated ADRs (full repository: `docs/adr/0009-hermetic-state-boundary.md`, `docs/adr/0016-hipb-bootstrap-project-toml.md`).

HIPB is complementary to **security**: it addresses **IP sovereignty and auditability**; it does not, by itself, certify confidentiality against a compromised host or a malicious MCP peer. Scope: full repository `docs/security-model.md`.

---

## Reading Order (full repository)

1. `docs/governance-protocol.md` — full R1–R8 text and quality gates  
2. `docs/adr/0000-system-genesis.md` — IP and collaboration baseline  
3. `docs/adr/0008-local-hitl-policy.md` — HITL classes and consent  
4. `AGENTS.md` — contributor hard rules  
