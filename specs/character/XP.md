# XP Ledger Specification — V1

**Status:** Product specification draft
**Scope:** Vampire/MET
**Related:** docs/PRD.md, specs/character/VAMPIRE_SHEET.md, specs/character/CHANGE_REQUESTS.md

## 1. Purpose

Define XP as an auditable ledger integrated with Character Change Requests. XP is not merely a mutable number on the character.

## 2. Core Principles

1. **Ledger first.** Current/unspent and lifetime/earned totals are derived whenever possible.
2. **No silent mutation.** Every balance-affecting operation has actor, reason, timestamp and provenance.
3. **Spend follows approval.** A player proposal does not debit XP while pending.
4. **Atomic commit.** When an approved proposal purchases mechanical state, official sheet mutation and XP spend commit together.
5. **History survives corrections.** Do not destroy disputed/incorrect history to make totals look right.
6. **Rules determine cost.** The ledger stores the resolved cost and evaluation snapshot; it does not invent cost rules.
7. **Historical dates are legitimate.** IC/effective dates may be old; recorded/imported timestamps are separate concepts.

## 3. XP Concepts

### Available XP
Spendable balance according to accepted ledger entries and applicable rules.

### Lifetime Earned XP
Total qualifying XP earned. Exact formula must be explicit and must not be inferred from a legacy label alone.

### Spent XP
Accepted spend transactions.

### Adjustment
Audited correction that changes a balance without pretending the original event never existed.

### Opening / Imported State
A migration may establish an opening state when complete transaction history cannot be reconstructed. It must be visibly marked as imported/reconciled.

## 4. XP Transaction

Minimum fields:
- id
- character_id
- type
- amount
- effective_at
- recorded_at
- recorded_by
- chronicle_id
- reason
- status
- source_type/source_id
- change_request_id nullable
- correction_of_id nullable
- import_batch_id nullable
- raw_legacy_reference nullable
- metadata
- created_at

Types:
- EARN
- SPEND
- ADJUSTMENT
- OPENING_BALANCE
- MIGRATION_RECONCILIATION

Do not expose generic SET_EARNED/SET_UNSPENT as ordinary player/ST operations. Legacy equivalents may be imported as reconciliation records with provenance.

## 5. Earn XP

Authorized staff can create XP awards.

Award supports:
- one Character;
- bulk selection of Characters where policy permits;
- amount;
- effective date;
- reason/category;
- source/session;
- optional note.

Bulk award creates traceable per-character ledger effects even if initiated as one operation.

Rules/policies such as monthly caps must be configured/versioned before the system enforces them.

## 6. Spend XP

Normal player spend originates from a Change Request.

Before submission, UI may show:
- current available XP;
- estimated cost;
- cost breakdown;
- rule source;
- prerequisites;
- warnings;
- external requirements.

Estimated cost is not a debit.

At final approval:
1. re-evaluate authoritative state;
2. verify sufficient XP;
3. verify required approvals;
4. calculate/freeze accepted cost;
5. commit sheet changes;
6. create SPEND transaction(s);
7. record audit/provenance.

All steps are one atomic operation from product perspective.

## 7. Bundled Purchases

One Change Request may contain multiple requested changes.

Each line item has:
- target holding/change;
- quantity/from/to;
- individual calculated cost;
- rule evaluation;
- governance dependency;
- approval disposition.

Product must support:
- approve all;
- reject all;
- **partial approval only if explicitly handled as separate accepted/rejected line items with recalculated total and user-visible result.**

Never charge rejected line items.

## 8. Zero-Cost Changes

A mechanical change can have zero XP cost and still require:
- ST review;
- rule validation;
- approval/notification;
- provenance.

Zero cost does not bypass Change Request.

## 9. Corrections

Preferred behavior:
- retain original transaction;
- create an ADJUSTMENT or explicit correction event;
- link correction to original;
- require reason and authorized actor.

If technical/legal requirements demand editing a malformed imported record, preserve immutable audit of before/after.

## 10. Transfers and Chronicle Changes

A Chronicle transfer is not inherently XP earned/spent.

Migration/transfer metadata should preserve:
- prior chronicle;
- effective date;
- imported balances/history;
- reconciliation status;
- source.

Do not fabricate XP transactions merely to represent narrative/administrative transfer unless a balance reconciliation is actually required.

## 11. Migration

Import can result in:
- fully reconstructed transactions;
- partially reconstructed ledger;
- opening balance + preserved legacy log;
- unresolved entries requiring review.

Importer must compare:
- parsed lifetime/earned value;
- parsed unspent/current value;
- reconstructed transactions.

Mismatch produces reconciliation task, not silent balancing.

## 12. Permissions

Player:
- view own authorized XP;
- create spend proposals;
- cannot award/correct XP.

ST/HST:
- permissions defined by chronicle role matrix;
- award/correct/review as authorized.

System:
- calculate/validate;
- never create discretionary XP award autonomously.

## 13. UX

XP tab:
- Available XP
- Lifetime Earned
- Pending XP committed by submitted requests (informational reservation, not debit)
- ledger
- filters: Earn / Spend / Adjustment / Import
- linked purchases
- provenance/detail drawer

Pending requests must not misrepresent available official balance. UI may additionally show "available if all pending requests are approved."

## 14. Concurrency

Two pending requests may each be affordable independently but not together.

Therefore:
- do not permanently reserve XP merely on submission unless later explicitly designed;
- re-check balance at final approval;
- if balance became insufficient, approval must stop and return to review.

## 15. Acceptance Criteria

1. Every official XP change is traceable.
2. Pending purchase does not debit XP.
3. Approved purchase updates sheet and XP atomically.
4. Rejected purchase costs zero XP.
5. Bundles preserve per-line cost/provenance.
6. Corrections do not erase history.
7. Imported incomplete history can reconcile without invented transactions.
8. Old IC/effective dates are allowed.
9. Concurrent approvals cannot silently create invalid negative balance.
10. Cost shown to user identifies rule source/version when engine-calculated.
