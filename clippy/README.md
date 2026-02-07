# Perma-Text: Bounded Virtual Context (BVC)

**Bounded prompt. Unbounded memory. Auditable facts.**

Perma-Text is an evidence-backed memory protocol that gives AI agents infinite effective context while keeping inference cost flat. Every fact traces to its source. Every session resumes cold with zero state loss.

## The Problem

AI context windows are finite. Long workflows — research chains, multi-session code refactors, title investigations, deep analysis — hit the wall and lose everything. You start over. You repeat yourself. You lose state.

## The Solution: Three-Layer Memory

| Layer | What | Size | Purpose |
|---|---|---|---|
| **LWS** (Live Working Set) | Current conversation | Bounded | Active work zone |
| **CSL** (Canonical State Ledger) | Hard-capped structured truth | ≤1500 tokens | Always injected on resume |
| **EV** (Evidence Vault) | Unlimited claims + citations | Unbounded | Retrieved selectively |

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

## Architecture

```
/home/claude/perma-text/
├── csl.json                 ← Canonical State Ledger (the truth, hard-capped)
├── claims.jsonl             ← Evidence Vault (atomic facts with provenance)
├── session_state.json       ← Session metadata
├── handoffs/
│   └── handoff_NNN.md       ← Compaction snapshots with provenance
├── scripts/
│   ├── session_init.py      ← Cold-start session loader
│   ├── compact.py           ← BVC compaction engine
│   └── claims.py            ← Evidence Vault manager
└── references/
    └── context_estimation.md ← Token budget heuristics
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
⚠ CONFLICTS DETECTED: 1
  Subject: Section 12 T1S R1E
    Old: Smith Family Trust (from Book 1234, Page 567)
    New: Johnson LLC (from Book 1301, Page 112)
```

You decide which wins. The loser gets `superseded`, not deleted.

## Who This Is For

- Anyone running multi-session AI workflows
- Researchers who need auditable fact chains
- Developers doing extended code refactoring across sessions
- Land professionals running chain-of-title investigations
- Anyone tired of AI forgetting what you told it 20 minutes ago

## Credits

Built by [TitleMind.AI](https://titlemind.ai) — where we make AI remember what matters.

BVC architecture inspired by bounded virtual context patterns in evidence-backed memory systems.

---

*Free your mind. Let the machine remember.*
