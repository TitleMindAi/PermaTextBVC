# Perma-Text: Bounded Virtual Context (BVC)

**Bounded prompt. Unbounded memory. Auditable facts. Verifiable truth.**

Perma-Text is an evidence-backed memory protocol that gives AI agents infinite effective context while keeping inference cost flat. Every fact traces to its source. Every session resumes cold with zero state loss. Every claim is cryptographically chained, oracle-verifiable, and zero-knowledge provable.

## The Problem

AI context windows are finite. Long workflows — research chains, multi-session code refactors, title investigations, deep analysis — hit the wall and lose everything. You start over. You repeat yourself. You lose state.

## The Solution: Three-Layer Memory

| Layer | What | Size | Purpose |
|---|---|---|---|
| **LWS** (Live Working Set) | Current conversation | Bounded | Active work zone |
| **CSL** (Canonical State Ledger) | Hard-capped structured truth | ≤1500 tokens | Always injected on resume |
| **EV** (Evidence Vault) | Unlimited claims + citations | Unbounded | Retrieved selectively |

## Five Advanced Modules

| Module | What It Does |
|--------|-------------|
| **Provenance Chain** | SHA-256 Merkle tree + append-only hash log. Every claim and CSL snapshot is chained into an immutable, tamper-evident ledger. Enables third-party audits and legal-grade evidence. |
| **Oracle Validation** | Auto-query trusted external sources (county records API, blockchain land registries, USGS, SEC EDGAR) to verify and upgrade claim confidence. Passive memory becomes an active truth engine. |
| **Multi-Agent Consensus** | Shared CSL across multiple agents/sessions with conflict resolution via weighted voting and staking. Enables collaborative research without state drift. |
| **Self-Evolving Normalization** | RL-inspired utility scoring optimizes what stays in the CSL. Fields are promoted/demoted based on access frequency, outcome success, and temporal recency. The CSL adapts per domain and user. |
| **Zero-Knowledge Proofs** | Hash-based commitment scheme with selective field disclosure. Prove a claim exists, meets a confidence threshold, or reveal specific fields — all without exposing the full Evidence Vault. Critical for sensitive title data. |

## Two Non-Negotiable Rules

1. **CSL is always small.** Hard cap enforced. If it grows, it normalizes — never expands.
2. **No fact without provenance.** Every claim links to `source_doc` + `source_ref` + `confidence`.

## What It Does

- **Auto-compacts** at ~50% context consumption — no manual intervention
- **Detects conflicts** when new evidence contradicts existing claims
- **Resumes cold** — any new session loads CSL + latest handoff and picks up exactly where you left off
- **Tracks confidence** — claims carry 0.0-1.0 confidence scores
- **Supersedes cleanly** — old facts get marked `superseded`, not deleted. Full audit trail.
- **Rebuilds from wreckage** — if state is lost, reconstructs from handoff history
- **Chains every fact** — immutable Merkle tree provides tamper-evident audit trail
- **Validates against oracles** — external sources auto-verify claim accuracy
- **Coordinates agents** — multi-agent consensus prevents state drift
- **Self-optimizes** — RL-based normalization keeps the right facts in CSL
- **Proves without revealing** — ZKP selective disclosure for sensitive data

## Architecture

```
/home/claude/perma-text/
├── csl.json                 <- Canonical State Ledger (the truth, hard-capped)
├── claims.jsonl             <- Evidence Vault (atomic facts with provenance)
├── chain.jsonl              <- Provenance chain (immutable hash log)
├── merkle_roots.jsonl       <- Merkle tree root snapshots
├── commitments.jsonl        <- ZKP claim commitments
├── proofs.jsonl             <- ZKP proof audit log
├── oracle_registry.json     <- Registered oracle sources
├── validations.jsonl        <- Oracle validation audit trail
├── field_stats.json         <- Normalization utility scores
├── access_log.jsonl         <- Field access events (RL training)
├── session_state.json       <- Session metadata
├── handoffs/
│   └── handoff_NNN.md       <- Compaction snapshots with provenance
├── consensus/
│   ├── shared_csl.json      <- Multi-agent consensus CSL
│   ├── agents.json          <- Agent registry
│   ├── proposals.jsonl      <- CSL patch proposals
│   ├── votes.jsonl          <- Voting records
│   └── consensus_log.jsonl  <- Resolution history
├── scripts/
│   ├── session_init.py      <- Cold-start session loader
│   ├── compact.py           <- BVC compaction engine
│   ├── claims.py            <- Evidence Vault manager
│   ├── provenance_chain.py  <- Merkle tree + hash chain
│   ├── oracle.py            <- External oracle validation
│   ├── consensus.py         <- Multi-agent consensus
│   ├── normalization.py     <- Self-evolving CSL optimization
│   └── zkp.py               <- Zero-knowledge proofs
└── references/
    └── context_estimation.md <- Token budget heuristics
```

## Installation

Upload `perma-text.skill` to Claude.ai via **Settings → Profile → Skills**.

The skill auto-triggers on long-running workflows or when you say "perma-text" or "context management."

## How Claims Work

Every extracted fact becomes an atomic record:

```json
{
  "subject": "Section 12 T1S R1E",
  "fact": "Current owner: Smith Family Trust per deed 2024-001234",
  "source_doc": "county_deed_records",
  "source_ref": "Book 1234, Page 567",
  "confidence": 0.95,
  "status": "active",
  "timestamp": "2025-02-07T03:04:00Z",
  "handoff": 1
}
```

When a new claim contradicts an existing one, the system flags it:

```
CONFLICTS DETECTED: 1
  Subject: Section 12 T1S R1E
    Old: Smith Family Trust (from Book 1234, Page 567)
    New: Johnson LLC (from Book 1301, Page 112)
```

You decide which wins. The loser gets `superseded`, not deleted.

## Provenance Chain Verification

Every claim is hashed into an append-only chain. At any point, generate a Merkle root that serves as a single tamper-evident fingerprint for the entire Evidence Vault:

```
python scripts/provenance_chain.py audit

  PROVENANCE CHAIN AUDIT REPORT
  Chain length     : 47 entries
  Chain integrity  : VALID
  Merkle snapshots : 3
  Latest root      : a7f3b2c1e9d4...
```

## Zero-Knowledge Selective Disclosure

Prove a claim's subject and source without revealing the fact itself:

```
python scripts/claims.py disclose "Section 12" subject source_doc

  SELECTIVE DISCLOSURE PROOF
  Fingerprint : 8f3a...
  Verified    : True
  Revealed    : subject, source_doc
  Hidden      : 6 fields

    subject: Section 12 T1S R1E
    source_doc: county_deed_records

  Hidden field commitments (verifiable without content):
    fact: c4a9e2f1...
    confidence: 7b3d...
```

## Who This Is For

- Land professionals running chain-of-title investigations with legal-grade audit trails
- Researchers who need auditable, oracle-verified fact chains
- Multi-agent teams doing collaborative research without state drift
- Developers doing extended code refactoring across sessions
- Anyone handling sensitive data who needs privacy-preserving verification
- Anyone tired of AI forgetting what you told it 20 minutes ago

## Credits

Built by [TitleMind.AI](https://titlemind.ai) — where we make AI remember what matters.

BVC architecture inspired by bounded virtual context patterns in evidence-backed memory systems.

---

*Free your mind. Let the machine remember — and prove it.*
