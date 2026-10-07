# Conceptual Data Model — V1

**Status:** Architecture/product bridge draft  
**Scope:** Vampire/MET V1 with foundations for Narrative Engine and OWBN network growth  
**Purpose:** Define domain entities, ownership, relationships, invariants and boundaries before technical schema design.

> This is not the final SQL schema. Storage technology, indexes, ORM models and physical normalization belong to the approved technical plan.

---

## 1. Architectural Invariants

The implementation must preserve these distinctions:

1. **Character != Sheet**
2. **PC and NPC are both Characters**
3. **Catalog Definition != Character Holding/Acquisition**
4. **Proposed State != Official State**
5. **XP Balance != Mutable Counter**
6. **Rule Source != Rule Interpretation != Rule Evaluation != Human Decision**
7. **Controlled Item != Permanent Boolean on Catalog Item**
8. **Authority Office != Current Office Holder**
9. **Governance Requirement != Governance Request != Governance Decision**
10. **Action Intent != ST Resolution != Event Truth**
11. **Event Truth != Character Knowledge != Rumor**
12. **Public Identity != Private Character Record**
13. **Technical Administration != Narrative Authority**
14. **Visibility != UI Hiding**
15. **Imported Claim != Verified Canon**
16. **Historical/effective date != system recorded timestamp**

---

## 2. Domain Map

### Identity & Access
- User
- Chronicle
- ChronicleMembership
- RoleAssignment
- PermissionGrant / ShareGrant
- GovernanceOffice
- OfficeHolder / DirectoryEntry

### Character
- Character
- CharacterControl
- CharacterIdentity/Profile
- CharacterPosition
- CharacterHolding
- specialized holding metadata
- CharacterNote
- PublicIdentity

### Rules
- RuleSource
- Rule
- RuleVersion / effective versioning
- RuleCondition
- RuleEffect
- RuleEvaluation
- ControlledItemRegulation

### Change & XP
- ChangeRequest
- ChangeRequestItem
- XPTransaction

### Governance
- GovernanceRequirement
- GovernanceRequest
- GovernanceEvent
- GovernanceEvidence
- GovernanceDecision

### Chronicle Operations
- Action
- ActionParticipant
- ActionReference
- ActionResource
- ActionResolution
- ActionOutputProposal
- ActionTemplate
- ActionCycle

### Narrative Foundation / Future
- Event
- CharacterKnowledge
- Rumor
- Relationship
- Promise/Hook
- Plot
- Organization
- Location

### Provenance & Operations
- SourceReference
- ImportBatch
- ImportRecord
- AuditEvent
- Attachment

---

## 3. User

Represents authenticated human identity.

Key fields:
- id
- account identity
- display/profile data
- status
- created_at
- last_active_at

A User has no broad content authority merely by existing.

Relationships:
- User 1..* ChronicleMembership
- User 1..* RoleAssignment
- User may control 0..* PCs
- User may hold 0..* Governance Offices through effective assignments

Authentication details should remain separated from game-domain profile where practical.

---

## 4. Chronicle

Primary operational/narrative sovereignty boundary.

Fields:
- id
- name
- slug/code
- location/domain metadata
- status
- rules configuration reference
- locale/timezone
- created_at

Relationships:
- has Members/Roles
- controls Characters/NPCs
- owns House Rules
- owns Actions/Templates/Cycles
- participates in Share Grants/Narrative Agreements
- handles governance requests

A Chronicle is not equivalent to an OWBN Domain in every possible future organizational sense; avoid premature conflation.

---

## 5. Chronicle Membership

Represents a User's membership relationship with Chronicle.

Fields:
- user_id
- chronicle_id
- status
- joined_at
- left_at
- membership metadata

Membership does not itself grant ST powers.

---

## 6. Role Assignment

Effective-dated authorization assignment.

Fields:
- user_id
- role
- scope_type/id
- effective_from/to
- status
- granted_by
- source/reason

Examples:
- PLAYER in Chronicle A
- ST in Chronicle A
- HST in Chronicle A
- CHRONICLE_ADMIN in Chronicle A
- NETWORK_ST for Agreement X
- COORDINATOR for GovernanceOffice Y

Role and permission are related but not identical; capabilities may be configured/granted separately.

---

## 7. Governance Office & Directory

### GovernanceOffice
Stable organizational authority identity.

Examples conceptually:
- a specific Clan Coordinator office
- Genre Coordinator office
- other governing office/body

Fields:
- id
- office_type
- name
- scope
- active state

### OfficeHolder / DirectoryEntry
Effective-dated person/contact resolution.

Fields:
- office_id
- holder identity/reference
- contact endpoint
- effective_from/to
- source
- last_verified_at
- status

Rules point to GovernanceOffice. Operational requests snapshot the resolved holder/contact at send time.

---

## 8. Character

Durable fictional entity.

Fields:
- id
- type: PC | NPC
- canonical_name
- lifecycle
- home_chronicle_id
- controlling_chronicle_id
- start/effective metadata
- public_identity_id nullable
- provenance

Character persists even when:
- sheet changes;
- player changes;
- Chronicle transfer occurs;
- public identity changes;
- character becomes inactive/dead/retired.

Do not use Character row as a giant sheet.

---

## 9. Character Control

Represents who controls/manages a Character.

Fields:
- character_id
- controller_type: USER | CHRONICLE | scoped group
- controller_id
- control_type
- effective_from/to
- provenance

PC usually links to Player control.
NPC usually links to Chronicle/ST authority.

Control does not automatically equal permission to every hidden field.

---

## 10. Character Identity / Profile

Relatively stable Vampire identity fields that are not generic holdings.

Candidate fields:
- clan/bloodline references
- sect
- faction
- coterie/pack
- nature
- demeanor
- concept
- morality/path configuration
- generation where model choice determines it belongs here vs holding

Final physical schema may move rule-sensitive fields into holdings/configuration; conceptual distinction is more important than table choice.

---

## 11. Character Position

Political/organizational office held by Character.

Fields:
- character_id
- title/position catalog or label
- organization/domain
- effective_from/to
- active state
- grant/source
- visibility
- provenance

Do not store all titles in one string on Character.

---

## 12. Catalog Definitions

Rules-aware definitions for selectable mechanics.

Possible catalogs:
- TraitCatalog
- AbilityCatalog
- LoreCatalog
- DisciplineCatalog
- PowerCatalog
- BackgroundCatalog
- InfluenceCatalog
- MeritFlawCatalog
- RitualCatalog
- RiteCatalog
- StandingCatalog
- DerangementCatalog
- ItemCatalog
- Morality/Path catalogs

Shared conceptual fields:
- stable id
- canonical name
- category/type
- source
- ruleset/edition
- effective/active status
- custom/local marker
- provenance

A catalog entry describes **what a mechanic is**, not whether a Character possesses it.

Avoid premature one-table-vs-many-table decision; technical plan should choose based on query/rule needs.

---

## 13. Character Holding

Represents official possession/state of a mechanical concept.

Generic conceptual fields:
- id
- character_id
- holding_type
- catalog_reference
- value/rating/quantity
- descriptor
- effective_from/to
- official status
- acquired_via_change_request_id
- xp_transaction linkage
- provenance
- visibility

Specialized metadata can model:
- teacher for powers
- linked NPC for Ally
- influence specialization
- merit/flaw descriptor
- Standing grantor
- item mechanics
- blood bond target
- etc.

Important: do not force all specialized semantics into one JSON blob merely to keep one table.

---

## 14. Acquisition / Provenance

For mechanically meaningful holdings, provenance may include:
- source Change Request
- XP spend
- teacher/grantor
- Action/Scene
- rule evaluation
- governance evidence
- import source
- effective date

Whether implemented as explicit Acquisition entity or structured relations on specialized holdings is a technical design decision.

The capability must exist regardless.

---

## 15. Character Note

Fields:
- id
- character_id/subject reference
- note_type
- author
- content
- visibility
- explicit grants if restricted
- created/updated
- source

Note is not a replacement for structured mechanical/narrative entities.

---

## 16. Public Identity

Separate publishable projection of Character.

Fields:
- character_id
- public/display name
- approved public biography
- public image/media references
- public titles/status where permitted
- publication state
- visibility
- approved/published by
- timestamps

Private Character Sheet is never the implicit source payload for Wiki/publication.

---

## 17. XP Transaction

Immutable/auditable ledger event.

Fields:
- character_id
- type: EARN | SPEND | ADJUSTMENT | OPENING_BALANCE | MIGRATION_RECONCILIATION
- amount
- effective_at
- recorded_at
- actor
- chronicle
- reason
- source reference
- change_request_id nullable
- correction_of nullable
- import_batch nullable
- status/provenance

Derived projections:
- available XP
- lifetime earned
- spent
- pending proposed spend (not ledger debit)

Do not make current XP a freely editable Character field.

---

## 18. Change Request

Proposal container.

Fields:
- character_id
- requested_by
- chronicle_id
- status
- submitted/reviewed timestamps
- justification
- source Action/Scene nullable
- rule evaluation snapshot/reference
- estimated/accepted total cost
- decision metadata
- concurrency/version reference

Relationships:
- 1..* ChangeRequestItem
- 0..* GovernanceRequirement
- on approval → Holding changes + XPTransaction(s)

---

## 19. Change Request Item

Atomic proposed delta.

Fields:
- request_id
- domain/holding type
- target catalog/holding
- operation
- before state
- proposed state
- quantity
- estimated/accepted cost
- acquisition context
- rule result
- disposition

Per-item disposition allows partial approval without charging rejected items.

---

## 20. Rule Source

Authoritative/reference source metadata.

Fields:
- type
- title
- authority/publisher
- reference/URL/document
- edition/version
- effective dates
- last verified
- status
- provenance

May point to a source without storing copyrighted full text.

---

## 21. Rule

Normalized rule interpretation tied to source.

Fields:
- source_id
- stable key
- section/reference
- type
- scope
- normalized interpretation
- authority layer
- effective dates
- machine-evaluable flag
- supersession
- provenance

Rule has typed Conditions and Effects.

---

## 22. Rule Condition & Effect

### Condition
Typed predicates such as:
- PC/NPC
- clan
- item/power
- level
- existing holdings
- acquisition context
- Chronicle
- effective date

### Effect
- cost
- prerequisite
- limit
- prohibition
- warning
- manual review
- governance requirement
- evidence requirement

Physical implementation may use a constrained expression format, normalized relational rows or hybrid design. Avoid arbitrary executable code stored as rules.

---

## 23. Rule Evaluation

Immutable decision-support snapshot.

Fields:
- subject/request/item
- Character snapshot/version
- evaluated_at
- applicable source/rule versions
- result: CLEAR | REVIEW | BLOCKED | MANUAL_REVIEW
- matched rules
- cost breakdown
- prerequisites
- governance outputs
- explanation
- evaluation version/hash

Human decision is not stored as RuleEvaluation.

---

## 24. Controlled Item Regulation

Contextual regulatory rule/mapping.

Fields:
- catalog concept
- source Rule
- effective dates
- condition set
- PC requirement
- NPC requirement
- authority office(s)
- evidence requirements
- machine evaluability
- provenance

Do not store a permanent `requires_approval` boolean on Power/Item.

---

## 25. Governance Requirement

Structured obligation emitted by Rules Engine.

Fields:
- requirement type
- source rule/version
- subject/change item
- character/chronicle
- authority office(s)
- threshold/logic
- blocking behavior
- required evidence
- status

Requirement says **what must happen**.

---

## 26. Governance Request

Operational case used to satisfy Requirement.

Fields:
- requirement_id
- request type
- requesting Chronicle/User
- authority offices
- resolved holder/contact snapshot
- lifecycle status
- send/acknowledge/resolve timestamps
- external reference
- decision/result metadata
- audit

Request says **what operationally happened**.

---

## 27. Governance Decision & Evidence

### Decision
- request
- result
- decision authority
- decision date
- recorded_by
- recorded_at
- scope/conditions
- reason

### Evidence
- request/decision
- evidence type
- attachment/message/reference
- source
- visibility
- verification state

Decision maker and platform recorder must remain distinct.

---

## 28. Action

Player/ST declared activity.

Fields:
- character
- submitted_by
- home/handling Chronicle
- type/category
- title
- intent
- method
- fictional/effective period
- visibility
- status
- assigned ST
- timestamps/provenance

Action intent is never automatically Event Truth.

---

## 29. Action Participant / Reference / Resource

### Participant
- Action
- Character/NPC
- role: actor | participant | target | subject
- visibility

### Reference
- Action
- referenced entity type/id
- relationship/purpose
- unresolved raw text fallback

### Resource
- Action
- Holding/reference
- declared by Player
- validation state
- ST adjudication metadata

Possession does not imply effect/success.

---

## 30. Action Resolution

Separate resolution layers.

Fields:
- action_id
- internal/ST resolution
- player-facing resolution
- outcome classification
- resolved_by
- resolved_at
- visibility
- version/history

Do not overwrite original Action intent.

---

## 31. Action Output Proposal

Bridge to Narrative Engine.

Fields:
- action/resolution source
- output_type:
  - EVENT
  - KNOWLEDGE
  - RUMOR
  - RELATIONSHIP
  - PROMISE
  - CHANGE_REQUEST
  - XP
  - ITEM
  - STANDING
  - NPC_CHANGE
  - LOCATION_CHANGE
  - PLOT_HOOK
- proposed payload
- source excerpt/reference
- AI/human proposer
- confidence where AI
- visibility
- status: PROPOSED | CONFIRMED | EDITED | REJECTED
- confirmed_by/at

V1 can implement only outputs whose target modules exist while preserving extensibility.

---

## 32. Event — Narrative Foundation

Canonical occurrence.

Conceptual fields:
- id
- event type
- title/summary
- occurred_from/to
- location(s)
- participants
- canonical facts
- source(s)
- controlling Chronicle
- visibility
- canon state
- confirmed_by

Event is created/confirmed by authorized humans.

A Scene may later be a specialized grouping/context around Events; do not require every Event to equal a session scene.

---

## 33. Character Knowledge — Narrative Foundation

Represents what a Character knows/believes.

Fields:
- knower_character_id
- proposition/fact reference
- knowledge type
- confidence/certainty
- learned_at
- source Event/Action/Scene
- visibility
- state

Knowledge can be false/incomplete without changing Event Truth.

---

## 34. Rumor — Narrative Foundation

Circulating information.

Fields:
- content
- origin reference if known
- created/effective time
- distribution scope
- visibility
- propagation records
- restricted truth relationship
- source/provenance

Rumor existence/content does not expose underlying Event Truth.

---

## 35. Relationship — Narrative Foundation

Typed edge between entities.

Fields:
- source entity
- target entity
- relationship type
- direction
- canonical/perceived layer
- strength/state where useful
- effective dates
- source/provenance
- visibility

Examples:
- ally
- enemy
- mentor/student
- sire/childe
- ghoul/domitor
- employer/agent
- trusts/distrusts
- owes
- custom

Mechanical bonds may require specialized holding data while linking to Relationship.

---

## 36. Promise / Hook — Narrative Foundation

Unresolved obligation or future story seed.

Fields:
- source/owed by
- target/owed to
- content
- condition/deadline
- state: OPEN | FULFILLED | BROKEN | CANCELLED | UNKNOWN
- source Event/Action
- visibility
- provenance

Not every narrative promise is a mechanical Boon.

---

## 37. Organization & Location — Narrative Foundation

First-class entities to avoid burying world references in prose.

Organization:
- name/type
- controlling Chronicle
- visibility
- relationships
- provenance

Location:
- name/type
- parent/geographic context
- controlling Chronicle
- visibility
- provenance

Public real-world location data and fictional secret location data require different visibility treatment.

---

## 38. Share Grant / Narrative Agreement

Explicit cross-Chronicle authorization.

ShareGrant:
- source Chronicle
- recipient principal/scope
- subject/object
- permitted fields/categories/actions
- purpose
- effective dates
- onward sharing
- granted/revoked by
- audit

Narrative Agreement may later add story-specific authority/consent semantics on top of ShareGrant.

Default cross-Chronicle access is deny.

---

## 39. Source Reference

Reusable provenance pointer.

Fields:
- source type
- title/label
- external/internal reference
- source entity/file/message
- effective date
- recorded date
- author/authority if known
- verification state
- visibility

Can support:
- rule citation
- imported sheet
- email approval
- scene log
- Action
- document
- manual historical assertion

SourceReference is evidence/provenance, not automatically canonical truth.

---

## 40. Import Batch

Tracks one migration operation.

Fields:
- id
- source system/type
- source artifact
- uploaded/started by
- timestamps
- parser/mapping version
- status
- summary counts
- provenance

## 41. Import Record

Per imported concept/row/field mapping.

Fields:
- batch
- source locator/raw value
- proposed target entity/field
- parsed value
- mapping status
- validation status
- review decision
- reviewer
- created entity reference
- warnings

Statuses support unresolved/ambiguous data without discarding it.

---

## 42. Attachment

Metadata for files/evidence.

Fields:
- owner/subject
- storage reference
- filename/type/size
- uploaded_by/at
- visibility
- purpose
- integrity/hash
- provenance

Authorization precedes retrieval/download/AI processing.

---

## 43. Audit Event

Append-oriented security/domain audit.

Fields:
- actor
- actor type
- action
- subject type/id
- scope
- timestamp
- before/after metadata where appropriate
- request/session correlation
- reason
- security context

Audit is not the same as narrative history.

Sensitive audit access is restricted.

---

## 44. Temporal Model

Where meaningful distinguish:
- **effective_at / occurred_at** — when something is true/happened in game/history
- **recorded_at** — when platform recorded it
- **created_at/updated_at** — technical persistence timestamps
- **valid_from/to** — policy/rule/role validity

Historical Vampire dates may be centuries old and are valid.

Do not use record creation timestamp as fictional chronology.

---

## 45. Official State & Versioning

Official Character state should support optimistic concurrency/version checks.

Change Request captures:
- relevant state/version at submission.

Before approval:
- compare with current state;
- re-evaluate conflicts/rules/XP.

Official changes produce:
- new holdings/state version;
- linked provenance;
- audit;
- XP transaction where applicable.

Avoid mutable state without history for regulated/mechanical fields.

---

## 46. Transaction Boundaries

Critical atomic transaction:

**Approve Change Request Item(s)**
- verify current Character version
- verify reviewer authority
- verify governance satisfied
- verify XP
- persist accepted Holding changes
- persist XP SPEND
- persist acquisition/provenance
- finalize request item status
- append audit

Failure rolls back the canonical mutation.

Governance communication and external waiting are not held inside database transactions.

---

## 47. Derived Read Models

For performance/UX, system may maintain derived projections:
- current Character Sheet
- XP balances
- pending purchases
- compliance summary
- Action inbox
- governance inbox
- public profile
- Chronicle dashboard

Read models/caches are not authoritative sources if they conflict with ledger/holdings/source records.

---

## 48. Authorization Boundary

Every repository/query service must receive authorization context.

Conceptual flow:
1. authenticate User;
2. resolve RoleAssignments/Grants;
3. authorize DISCOVER/READ/action;
4. constrain query;
5. filter fields;
6. return data;
7. only then assemble AI/export context.

Do not fetch all secrets and redact only in the client.

---

## 49. AI Boundary

AI interacts with authorized projections and creates proposals.

AI may create:
- draft text
- extraction candidates
- mapping suggestions
- rule interpretation flags
- Action output proposals

AI may not directly create official:
- Character Holdings
- XP discretionary awards
- Governance Decisions
- Event Truth
- Rules
without required human confirmation/workflow.

AI proposal records should identify model/process version where useful for audit/reproducibility.

---

## 50. Deletion & Historical Integrity

Prefer lifecycle states/retirement over destructive deletion for:
- Characters
- official holdings
- XP transactions
- decisions
- approvals
- Rules
- resolved Actions
- audit records

Privacy/legal deletion requirements may require specialized redaction/anonymization processes; do not implement arbitrary cascade deletes that destroy governance/history.

---

## 51. MVP Entity Priority

### Foundation / P0
- User
- Chronicle
- Membership
- RoleAssignment
- Character
- CharacterControl
- Catalogs
- CharacterHolding
- XPTransaction
- ChangeRequest / Item
- RuleSource / Rule / Condition / Effect
- RuleEvaluation
- ControlledItemRegulation
- GovernanceOffice / Directory
- GovernanceRequirement / Request / Decision / Evidence
- SourceReference
- AuditEvent
- ImportBatch / ImportRecord

### Chronicle Operations / P1
- Action
- ActionParticipant/Reference/Resource
- ActionResolution
- ActionTemplate/Cycle
- ActionOutputProposal
- CharacterNote
- Attachment

### Narrative Expansion
- Event
- CharacterKnowledge
- Rumor
- Relationship
- Promise/Hook
- Organization
- Location
- Narrative Agreement
- Public Identity publishing workflow

Some narrative entities may receive minimal foundation tables earlier if the technical plan finds that cleaner, but V1 should not prematurely build full World Graph features.

---

## 52. Key Relationship Map

Conceptually:

```text
User
 ├─ ChronicleMembership ── Chronicle
 ├─ RoleAssignment ──────── Scope
 └─ CharacterControl ────── Character

Character
 ├─ CharacterHolding ────── Catalog Definition
 ├─ XPTransaction
 ├─ ChangeRequest
 │   └─ ChangeRequestItem
 │       ├─ RuleEvaluation ── Rule ── RuleSource
 │       └─ GovernanceRequirement
 │           └─ GovernanceRequest
 │               ├─ GovernanceDecision
 │               └─ GovernanceEvidence
 ├─ Action
 │   ├─ ActionResolution
 │   └─ ActionOutputProposal
 └─ PublicIdentity

Rule
 ├─ RuleCondition
 ├─ RuleEffect
 └─ ControlledItemRegulation
      └─ GovernanceOffice
           └─ OfficeHolder/DirectoryEntry

Future:
Action/Event ──> Event Truth
Event/Action ──> CharacterKnowledge
Event/Action ──> Rumor
Entities ──────> Relationship
Events/Actions ─> Promise/Hook
```

---

## 53. Anti-Patterns to Reject in Technical Plan

Reject designs that:
- put the entire sheet in one opaque JSON column as authoritative state;
- put NPCs in a notes table;
- store XP only as `character.current_xp`;
- mutate approved holdings directly from Player form submission;
- encode Controlled Item as a boolean on Power;
- hardcode Coordinator emails;
- use one `role=admin` superuser for all permissions;
- treat all STs as global;
- merge Action submission and resolution into one editable text;
- use one knowledge table for truth/rumor/character knowledge;
- send unrestricted Character objects to AI then rely on prompting for secrecy;
- overwrite Rules when bylaws change;
- delete old approvals when replaced;
- infer approval from free-text import;
- treat old historical dates as invalid by age alone;
- build full multi-genre abstractions before Vampire V1 needs them.

---

## 54. Technical Plan Questions Claude Must Answer

Before implementation Claude should propose:
1. relational database choice and rationale;
2. identity/auth strategy;
3. physical schema mapping from this conceptual model;
4. catalog/holding modeling strategy;
5. typed rules expression strategy;
6. versioning/effective-date strategy;
7. authorization enforcement layer;
8. transaction strategy for Change Request approval;
9. audit architecture;
10. import staging architecture;
11. attachment/evidence storage;
12. search strategy with authorization;
13. AI proposal storage/context boundary;
14. testing strategy for rules/permissions/concurrency;
15. migration path for future Narrative Engine without premature implementation.

Claude must identify any place where technical constraints would force a product tradeoff rather than silently choosing one.

---

## 55. Definition of Ready for Implementation

The data architecture is ready to move into technical planning when:
- product accepts this entity/boundary model;
- pilot role decisions are sufficiently defined;
- initial Vampire sheet catalogs/source strategy is decided;
- initial OWBN rules corpus/source ingestion strategy is decided;
- import source formats are investigated;
- open V1 scope decisions are resolved or explicitly deferred;
- Claude's technical plan maps every P0 entity/workflow without violating invariants.

Implementation must not begin merely because tables can be created.
