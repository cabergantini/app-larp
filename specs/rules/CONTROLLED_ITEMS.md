# Controlled Items — V1

**Status:** Product specification draft  
**Scope:** Vampire/MET regulatory classification and governance dependency generation  
**Normative content:** OWBN source data must be separately ingested/versioned and verified.

## 1. Purpose
Model OWBN Controlled Items as contextual regulations rather than permanent booleans on powers/traits.

A Controlled Item regulation answers:
**For this item, this character type/context, at this effective date, what regulatory action is required and which authority controls it?**

## 2. Observed OWBN Regulatory Levels
Current Character Bylaws define categories including:
- Disallowed
- 2/3 Majority Vote
- Majority Vote
- Coordinator Approval
- Coordinator Notify

Listings can explicitly distinguish PC and NPC treatment and identify controlling Coordinator(s).

The data model must also support:
- Unregulated / no special Controlled Item requirement
- Storyteller/local review where sourced
- multiple authorities
- conditional/varied requirements
- MANUAL_REVIEW where a listing cannot be safely normalized.

These labels are versioned source data, not immutable enums of organizational policy forever.

## 3. Regulation Record
Fields:
- id
- controlled_item_key/catalog reference
- display label
- item category
- rule_id/source
- effective_from/to
- condition set
- pc_requirement
- npc_requirement
- authority_office_ids[]
- secondary/joint authorities[]
- evidence/context requirements
- notes/normalized interpretation
- machine_evaluable
- provenance
- supersession

## 4. Why Regulation Is Contextual
The same general mechanic/category can have different regulation depending on:
- PC vs NPC
- clan/bloodline
- level
- whether character belongs to that clan/type
- creation vs acquisition
- benefiting/learning/possessing
- sect
- multiple Coordinators
- effective rule version.

Therefore catalog item must not simply store `requires_coordinator_approval=true`.

## 5. Governance Requirement Output
When a Change Request matches a Controlled Item regulation, emit one or more Governance Requirements:
- type: NOTIFY | COORDINATOR_APPROVAL | COUNCIL_MAJORITY | COUNCIL_TWO_THIRDS | DISALLOWED | ST_REVIEW | MANUAL_REVIEW
- authority office(s)
- source rule/version
- applies-to item/request
- blocking behavior
- required evidence/context
- resolution state.

## 6. Blocking Semantics
- DISALLOWED: requested item cannot finalize through ordinary approval flow; any exceptional process must be explicitly supported by authoritative rules and separate workflow.
- COORDINATOR_APPROVAL: blocks official acquisition until approved.
- COORDINATOR_NOTIFY: workflow must ensure required notification is completed/recorded according to source rules; do not equate Notify with discretionary Coordinator approval.
- COUNCIL vote: blocks until valid vote result/evidence is recorded.
- MANUAL_REVIEW: blocks automated finalization until authorized human resolves interpretation.
- UNREGULATED: no Controlled Item dependency, though other rules may still apply.

## 7. Multiple Requirements
One item may require more than one authority/action.

Requirements form an AND set by default only when the source explicitly requires all listed authorities. Do not infer AND/OR semantics from punctuation during automated import.

## 8. PC vs NPC
PC and NPC regulation are separate fields/conditions.

The Character Bylaws contain examples where the same item is more restrictive for a PC than an NPC and vice versa. The evaluator selects the applicable branch from Character type and context.

## 9. Controlled Item Catalog Mapping
A regulation must map to a stable catalog concept where possible:
- clan/bloodline/creature configuration
- discipline/power
- combination power
- merit/flaw
- background
- ritual/blood magic
- item
- society/membership
- generation
- custom content
- other regulated concept.

If mapping is ambiguous, retain raw source reference and require review rather than attaching regulation to the wrong catalog item.

## 10. Rule Changes & Grandfathering
Do not encode a universal grandfathering assumption.

When regulation changes:
- preserve prior version;
- identify holdings/requests potentially affected;
- surface the source's transition/grandfathering instructions;
- create review tasks where needed;
- do not automatically revoke/approve holdings.

Current OWBN bylaws may specify transition windows for particular regulatory changes; these belong to the relevant versioned rule.

## 11. Existing Holdings
A Character can possess a regulated item with historical approval/evidence.

Holding should link to:
- acquisition/change request
- applicable regulation at acquisition
- Governance Request(s)
- decision/evidence
- grandfathering/exception evidence where applicable.

Rules Engine may flag missing provenance as REVIEW; absence of imported evidence is not proof that approval never existed.

## 12. Custom Content
Custom content needs explicit catalog identity and governance provenance.

Never make a custom item globally available because one Character received approval.

Approval scope must state what was approved:
- this Character only
- a Chronicle/local rule
- broader published/organizational content
according to actual evidence.

## 13. Data Ingestion
Controlled Item source ingestion should support:
1. capture source/version/effective date;
2. parse candidate entries;
3. map candidate to catalog concepts;
4. human review;
5. publish normalized regulation;
6. run regression tests against known examples.

No scraped/parser output becomes enforcement logic without review.

## 14. Test Fixtures
Rules corpus should include verified fixtures covering:
- PC approval / NPC notify
- PC disallowed / NPC approval
- same requirement for PC/NPC
- joint Coordinators
- level threshold
- clan/non-clan condition
- unregulated branch
- Council vote
- custom/conditional listing.

Fixtures reference source version and expected Governance Requirement output.

## 15. Acceptance Criteria
1. Regulation can differ PC vs NPC.
2. Multiple controlling authorities are supported.
3. Regulation changes are effective-dated.
4. Notify is not treated as Approval.
5. Council thresholds are distinct.
6. Disallowed does not masquerade as ordinary Coordinator denial.
7. Existing approval evidence links to holdings.
8. Missing evidence is not automatically interpreted as no historical approval.
9. Ambiguous source mapping cannot become machine enforcement.
10. A Change Request receives structured governance dependencies from the Rules Engine.
