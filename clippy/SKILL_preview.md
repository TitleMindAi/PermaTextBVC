---
name: perma-text
description: >
  Bounded Virtual Context (BVC) protocol for infinite session continuity.
  Maintains a hard-capped Canonical State Ledger (CSL) with provenance-linked
  claims across arbitrarily long workflows. Every fact traces to source evidence.
  Now with verifiable provenance chains, oracle validation, multi-agent consensus,
  self-evolving normalization, and zero-knowledge proofs.
  Bounded prompt. Unbounded memory. Auditable facts. Verifiable truth.
  Triggers: (1) long-running tasks requiring session splits, (2) user mentions
  "perma-text", "context management", or "BVC", (3) context threshold detection
  at ~50% consumption, (4) session handoff/resume operations, (5) any multi-session
  research, chain-of-title investigation, extended refactoring, or deep workflow.
---

# Perma-Text: Bounded Virtual Context Protocol

## Three-Layer Memory Stack

1. **LWS (Live Working Set)**: Current conversation + instructions + CSL header. Bounded by context window.
2. **CSL (Canonical State Ledger)**: Structured, hard-capped truth (~1500 token max). Always injected on resume. Every field links to evidence.
3. **EV (Evidence Vault)**: Unlimited claims + citations + handoff archives. Retrieved selectively.

## Five Advanced Modules

| Module | Purpose | Script |
|--------|---------|--------|
| **Provenance Chain** | Immutable Merkle tree + SHA-256 hash chain for tamper-evident audit trails | `provenance_chain.py` |
| **Oracle Validation** | Auto-query trusted sources (county records, blockchain registries) to verify claims | `oracle.py` |
| **Multi-Agent Consensus** | Shared CSL across agents with conflict resolution via weighted voting | `consensus.py` |
| **Self-Evolving Normalization** | RL-inspired optimization of CSL field retention based on access patterns | `normalization.py` |
| **Zero-Knowledge Proofs** | Prove claim validity without revealing full EV — selective field disclosure | `zkp.py` |

## Protocol

### 1. SESSION INIT

Run `scripts/session_init.py` on every new session. It will:
- Load CSL and latest handoff into context
- Display session number, resume point, and active claims count
- Initialize fresh state if no prior session exists
- **Verify provenance chain integrity** (tamper detection)
- **Run normalization session tick** (apply decay to unused fields)
- **Register agent for consensus** (multi-agent coordination)
- **Initialize ZKP secret** (commitment readiness)

### 2. CONTEXT TRACKING

Estimate consumption heuristically. See `references/context_estimation.md` for the decision matrix.

**Soft trigger**: ~20 substantial assistant turns OR >50KB cumulative tool results -> begin monitoring.
**Hard trigger**: ~25 turns OR degraded early-conversation recall -> IMMEDIATE compaction.

### 3. COMPACTION

When threshold is reached, run `scripts/compact.py` with session data. The script:

1. Generates a provenance-linked handoff document
2. Patches the CSL (enforcing hard cap via normalize/merge)
3. Appends new atomic claims to the Evidence Vault
4. Runs conflict detection against existing claims
5. Writes all state to disk
6. **Chains claims + CSL into provenance hash log** (immutable audit trail)
7. **Creates ZKP commitments** for new claims (privacy-preserving verification)
8. **Runs self-evolving normalization** (optimize field retention)
9. **Syncs to multi-agent consensus CSL** (collaborative state)

**Do NOT ask permission. Do NOT pause work.** Compact and continue.

Supply these arguments to `compact.py`:
- `summary`: 2-3 sentence overview of accomplishments
- `active_threads`: List of in-progress work items with status
- `critical_state`: Dict of key-value pairs that MUST persist (IDs, paths, decisions)
- `pending_actions`: Ordered list of next steps with enough detail to resume cold
- `new_claims`: List of dicts with `fact`, `source_doc`, `source_ref`, `confidence` (0.0-1.0)
- `decisions`: Key choices made this session
- `files_modified`: Paths and descriptions of output files

### 4. POST-COMPACTION

Continue working. CSL is updated in memory. Full context freed for new work.

### 5. SESSION BOUNDARY

If context reaches ~85% even after compaction:
- Run final compaction
- Inform user: "Session boundary reached. Next message starts fresh with full state preserved."
- Next conversation runs `session_init.py` and resumes seamlessly

## Two Rules (Non-Negotiable)

1. **CSL is always small.** Hard cap ~1500 tokens. If it grows, normalize/merge — never expand.
2. **No fact without provenance.** Every CSL field and every claim links to source: `source_doc` + `source_ref` + `confidence`.

## Emergency Recovery

If state is corrupted or missing:
1. Read `journal.txt` for session index
2. Load most recent transcript from `/mnt/transcripts/`
3. Scan last 20% for state reconstruction
4. Run `scripts/claims.py --rebuild` to regenerate claims from handoff history
5. Run `scripts/provenance_chain.py verify` to check chain integrity
6. Resume from reconstructed state
