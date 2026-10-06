# PRD V1 — OWBN Narrative Platform

**Status:** Draft for product approval  
**Scope:** Vampire: The Masquerade / Mind's Eye Theatre (Revised / 3rd Edition)  
**Pilot:** one real Brazilian OWBN chronicle; no pilot-specific hardcoding  
**Product principle:** Vampire-first, genre-extensible  
**North Star:** A plataforma não existe para guardar fichas. Ela existe para fazer histórias continuarem.

---

## 1. Product Summary

A web platform for OWBN Vampire chronicles combining character administration, mechanical governance, XP, controlled-item workflows, approvals, Actions/Downtimes and narrative continuity.

The V1 must be capable of replacing the essential day-to-day administrative loop of a character-management tool for a pilot chronicle while establishing the foundations for two differentiated engines:

1. **Rules & Governance** — the platform can explain why a proposed mechanical change may require ST action, Coordinator notification/approval or other review, with source and version.
2. **Narrative Memory** — play records can later become structured, permission-aware continuity instead of isolated notes.

The system is advisory where organizational interpretation or human judgment is required. It does not claim autonomous authority over OWBN rulings.

---

## 2. Goals

### G1 — Operate a Vampire chronicle
Allow a chronicle to manage users, memberships, PCs, NPCs, sheets, XP, requests and downtime without requiring a second character-management system for normal V1 operations.

### G2 — Player-driven proposals with authoritative review
Players may propose purchases/changes. Official character state changes only through the appropriate workflow and authorization.

### G3 — Explainable rules compliance
When possible, evaluate proposed changes against versioned base/OWBN/house rules and show the applicable source, requirement and authority.

### G4 — Preserve provenance
Mechanical and governance-relevant state must be traceable to actor, source, time and decision.

### G5 — Support migration
Reduce adoption friction by importing supported legacy character data and requiring human reconciliation before it becomes official.

### G6 — Prepare narrative continuity
Actions/Downtimes and character records must be structured so Scene Intelligence, Character Knowledge, Rumor and Chronicle Memory can be added without redesigning the core model.

---

## 3. Non-Goals for V1

- Garou, Mage, Changeling, Wraith or other genres.
- autonomous AI changes to canonical/mechanical state;
- automatic final interpretation of ambiguous OWBN rules;
- full cross-chronicle social/discovery network;
- automatic direct publishing to Camarilla Wiki;
- Coordinator institutional dashboard;
- automatic WhatsApp/Discord scene ingestion;
- complete semantic World Graph;
- plot matchmaking;
- replacing official OWBN Council voting infrastructure;
- reproducing every HallerGames feature before pilot launch.

---

## 4. Primary Actors

### Player
Owns/plays PCs, views authorized data, proposes sheet changes and XP spends, submits Actions/Downtimes, responds to requests for clarification.

### Storyteller (ST)
Reviews character proposals, manages chronicle characters/NPCs, awards XP, adjudicates local requirements, handles downtime, reviews imports and maintains official character state.

### Head Storyteller (HST)
ST capabilities plus chronicle-level configuration/authority and staff management where policy allows.

### Chronicle Administrator
Technical/configuration role. Administrative access must not automatically imply unrestricted IC secret access unless explicitly granted.

### Coordinator / External Authority
V1 models the authority and approval/notification workflow. Full Coordinator workspace is post-V1. External decisions/evidence can be recorded by authorized staff.

### System
Evaluates deterministic rules where configured, produces warnings/requirements and preserves audit records. System recommendations are not organizational rulings.

---

## 5. Core Domain Principles

### 5.1 Character is not Sheet
Character is the central entity. Sheet, XP ledger, approvals, notes, actions and later narrative memory attach to Character.

### 5.2 PC and NPC are first-class
Both use Character. Type and control/ownership differ. NPC must not be represented as free text embedded in another character.

### 5.3 Proposed state vs official state
Player edits that affect authoritative mechanical state create proposals. UI must make clear whether data is:
- Draft
- Pending review
- Approved/Official
- Rejected
- Superseded/Withdrawn where applicable

### 5.4 Rule source and rule decision are separate
The platform stores what a source says/configures and records how a particular proposal was evaluated. Historical decisions remain reproducible after rules change.

### 5.5 Authorization before retrieval
Secret data must not be fetched and then merely hidden in UI or prompt instructions.

### 5.6 AI is proposal-only
Any AI extraction/classification creates a reviewable proposal with provenance.

---

## 6. V1 Modules

### 6.1 Authentication & Account
Requirements:
- secure account creation/sign-in;
- user profile;
- membership in one or more chronicles;
- role assignments scoped to chronicle;
- account deactivation without destroying historical audit records.

Acceptance:
- a user can be Player in one chronicle and staff in another without global role leakage;
- unauthorized chronicle data is not returned by backend APIs.

### 6.2 Chronicle
Fields/configuration:
- name;
- slug/id;
- OWBN affiliation metadata;
- geographic/online description;
- active/inactive;
- official game timezone;
- staff/memberships;
- links/references to current House Rules and relevant policies;
- configuration needed by supported workflows.

Capabilities:
- invite/add members;
- assign scoped roles;
- manage chronicle settings;
- maintain House Rule versions.

No pilot-specific behavior may be hardcoded.

### 6.3 Character
Minimum:
- PC/NPC;
- name;
- player for PC;
- controlling chronicle / Home Chronicle as applicable;
- lifecycle/status;
- clan/bloodline where applicable;
- sect;
- concept;
- generation and other supported Vampire identity fields;
- public/private metadata;
- created/updated/provenance metadata.

Lifecycle should support at minimum Draft, Active, Shelved/Inactive, Retired and Dead, with final terminology validated in UX/spec.

### 6.4 Vampire/MET Mechanical Sheet
The exact trait catalog will be specified from authoritative supported rules sources.

Architectural requirement:
- do not build one giant unversioned JSON blob as the only authoritative representation;
- mechanical entries need stable identity/type and provenance;
- support custom/local entries without pretending they are globally valid;
- distinguish current official value from pending proposal.

Candidate domains:
- attributes/traits;
- abilities;
- backgrounds;
- disciplines/powers;
- merits/flaws;
- morality/path where applicable;
- willpower;
- blood;
- health;
- influence/resource-style systems as applicable;
- lores;
- rituals/blood magic where applicable;
- equipment/item cards;
- status/positions where in scope.

Each category requires a dedicated functional spec before implementation.

### 6.5 Change Request / Character Proposal
Player can propose:
- acquire/increase/remove supported trait;
- spend XP;
- add supporting source/teacher/scene/note when relevant;
- request custom/manual item review.

Workflow baseline:
DRAFT → SUBMITTED → UNDER_REVIEW → APPROVED / REJECTED
Optional transitions: NEEDS_INFO, WITHDRAWN.

Rules evaluation runs before or during review and may attach requirements.

Approval does:
- validate authorization;
- write official state transactionally;
- create XP ledger entry if applicable;
- retain proposal and decision history;
- retain rule evaluation snapshot/provenance.

Rejection never mutates official sheet.

### 6.6 XP Ledger
XP must be ledger-based, not only a mutable total.

Entry types include:
- award;
- spend;
- adjustment/correction;
- import opening balance/history where available.

Requirements:
- immutable/auditable historical entries; corrections via compensating entry or controlled correction history;
- running balance;
- actor/reason/source/date;
- association to approved change request where applicable;
- ST award tools;
- player-visible balance/history subject to policy;
- no spend can silently create negative balance unless an explicit supported rule/policy allows it.

### 6.7 Rules Registry
Rule sources are versioned.

Source categories:
- Base MET/White Wolf supported rules reference;
- OWBN Charter/Bylaws;
- binding/approved OWBN material where applicable;
- Chronicle House Rules.

Minimum Rule metadata:
- source;
- source version/effective dates;
- section/reference;
- scope;
- applies-to criteria;
- requirement/effect;
- authority;
- severity/type;
- human-readable explanation;
- provenance/link/reference metadata.

The Rules Engine must support deterministic checks and MANUAL_REVIEW for ambiguity.

### 6.8 Rule Precedence
V1 policy model:
- OWBN binding limits cannot be loosened by a Chronicle House Rule;
- a chronicle may configure stricter local restrictions where permitted;
- no generic algorithm may invent precedence for an ambiguous case;
- conflicts/unknowns route to manual review with sources displayed.

### 6.9 Controlled Items
The system must be able to model regulatory levels/actions including, where supported by current authoritative data:
- no special action;
- ST review/approval;
- Coordinator Notify;
- Coordinator Approval;
- Council vote categories;
- disallowed;
- manual review.

Requirement may vary by PC/NPC, item/context, version and authority.

When a proposed sheet change includes a controlled item:
1. evaluate applicable rule version;
2. show requirement and source;
3. prevent finalization when a required external action is unresolved;
4. create/link governance workflow;
5. retain evidence of resolution.

### 6.10 Approval & Notification Engine
Core entity: Governance Request.

Fields:
- character;
- proposed item/change;
- requesting chronicle/user;
- requirement type;
- authority/office;
- status;
- sent/requested timestamps;
- deadlines when derived from an explicit versioned rule;
- evidence/source correspondence;
- comments;
- decision;
- rule snapshot/version.

States baseline:
DRAFT → READY → SENT → WAITING → NEEDS_INFO → APPROVED / DENIED
Additional: CANCELLED; EXPIRED only when semantically supported.

The platform must never assume an email recipient from hardcoded personal data. Authority resolution belongs to Directory/configuration.

V1 may record/send-preparation without full email automation; exact integration is technical-plan scope.

### 6.11 Directory Foundation
Store offices/roles separately from rules:
- office;
- authority type;
- official contact method;
- current holder/contact if available;
- validity/last verified;
- related official source.

A rule points to an office/authority, not a person's hardcoded email.

### 6.12 House Rules
Chronicle staff can maintain versioned local rules.

Requirements:
- published/current version;
- effective dates;
- history;
- source document/link;
- structured rules where used by engine;
- local rules may be stricter but cannot silently override binding OWBN constraints;
- players should be able to access the applicable current House Rules.

### 6.13 Actions / Downtimes
Player can submit an Action/Downtime for a Character.

Fields:
- title;
- character;
- chronicle;
- IC/OOC date/window;
- body;
- intended outcome;
- related traits/resources optionally;
- visibility;
- attachments/references if supported;
- status;
- ST response;
- provenance.

States:
DRAFT → SUBMITTED → IN_REVIEW → RESOLVED
plus NEEDS_INFO, WITHDRAWN when useful.

Design constraint:
Action results must later be linkable to Events, Knowledge, Relationships, Rumors, mechanical proposals and plots without rewriting the Action model.

### 6.14 Notes & Visibility
Support notes with explicit audience/visibility. Avoid a single universal "notes" text field for all secrecy classes.

Initial visibility vocabulary:
PUBLIC, PLAYER, CHRONICLE_ST, NETWORK_ST, COORDINATOR, RESTRICTED.

The permissions spec must define exact semantics before coding sensitive access.

### 6.15 Audit & Provenance
Audit required for:
- official sheet changes;
- XP entries;
- approval/notification status;
- rules changes;
- House Rule versions;
- role/permission changes;
- imports;
- sensitive governance decisions.

Audit record must identify actor, action, target, timestamp and relevant before/after or event payload safely.

### 6.16 Migration / Import
MVP requirement, exact adapters after format investigation.

Flow:
UPLOAD/INGEST → PARSE → MAP → VALIDATE → REVIEW → IMPORT

Requirements:
- imported data first enters staging;
- show unmapped/ambiguous values;
- never silently discard unknown traits;
- human confirms before official creation/update;
- preserve source file/reference and import report;
- support dry run;
- avoid duplicate character creation through explicit reconciliation.

Priority investigation:
1. HallerGames exports available to chronicle/user;
2. Grapevine XML compatibility;
3. CSV/manual structured fallback.

### 6.17 Basic Search & Operational Views
Player:
- My Characters;
- pending proposals;
- XP;
- Actions/Downtimes;
- approvals requiring player input.

ST:
- chronicle characters;
- pending character changes;
- XP operations;
- pending Actions;
- governance requests;
- import review;
- compliance/manual-review queue.

No global secret search in V1.

---

## 7. Permissions Model — V1 Requirements

Role alone is insufficient.

Authorization decision may depend on:
- user;
- chronicle membership;
- role;
- Character ownership/player;
- Character Home/controlling chronicle;
- visibility;
- explicit grant;
- governance authority where represented.

Must support:
- Player cannot edit official state directly;
- ST authority is chronicle-scoped;
- Chronicle Admin technical role does not automatically imply narrative omniscience;
- restricted notes require explicit authorization;
- imported source files inherit strict access;
- audit access is itself permission-controlled.

Detailed policy matrix belongs in `docs/PERMISSIONS.md` / specs before implementation.

---

## 8. Compliance UX

Never label a character "illegal".

Suggested statuses:
- CLEAR — no configured issue detected;
- REVIEW — warning/manual review/pending requirement;
- BLOCKED — a known required workflow prevents requested finalization.

Copy should say:
"Potential compliance issue detected" or equivalent.

Every compliance result should expose:
- detected condition;
- consequence/action;
- source;
- version/effective date;
- authority;
- next step.

---

## 9. Narrative Foundation in V1

Full Narrative Engine is not required for launch, but V1 must preserve inputs.

### V1 includes
- structured Actions/Downtimes;
- source/provenance;
- Character/NPC entities;
- visibility model;
- relationship-ready identifiers;
- timestamps;
- ability to attach a mechanical proposal to a narrative source/reference.

### Immediately post-core / P1
- Scene upload;
- AI Scene Intelligence proposals;
- Event Truth;
- Character Knowledge;
- Rumor;
- Relationship;
- Promise/Obligation;
- Chronicle Memory;
- Travel Brief/Return Report.

Hard constraint: do not store narrative data in ways that make the three-layer truth model impossible later.

---

## 10. AI Requirements

AI is not required for the first operational character sheet loop, but architecture must support it.

When enabled:
- input is permission-filtered before model call;
- output is stored as proposal, not canonical fact;
- source text/provenance is retained or referenced;
- user can accept/edit/reject;
- no approval is inferred as granted from natural language without human verification;
- sensitive prompts/outputs follow retention/security policy defined before production.

---

## 11. Data Integrity

- Use stable IDs.
- Official mechanical changes should be transactional.
- Historical rules/decisions must remain interpretable after updates.
- Avoid destructive overwrites of governance history.
- Timestamps stored consistently; display in relevant timezone.
- Soft deletion/archive where historical integrity requires preservation.
- Concurrency conflicts must not silently overwrite newer official character state.

---

## 12. Pilot Success Criteria

Pilot is successful when a real chronicle can, without routine dependence on another character manager:

1. onboard staff and players;
2. create/import PCs;
3. maintain Vampire/MET sheets;
4. award and spend XP through traceable workflows;
5. let players propose changes;
6. review/approve/reject those changes;
7. detect configured controlled-item requirements;
8. track approval/notification evidence;
9. maintain versioned House Rules;
10. submit/respond to Actions/Downtimes;
11. protect restricted data according to role/relationship;
12. retrieve a complete audit trail for a disputed mechanical change.

Quality target:
No critical unauthorized-data exposure, no silent loss of imported data, and no official mechanical mutation outside authorized workflow.

---

## 13. MVP Release Gates

### Gate A — Product specification
- PRD approved;
- permission matrix approved;
- Vampire sheet spec approved;
- XP spec approved;
- Rules/Controlled Items spec approved;
- Approval spec approved;
- Action/Downtime spec approved;
- migration formats investigated.

### Gate B — Technical plan
Claude/engineering plan must define:
- stack;
- schema;
- auth;
- authorization/RLS strategy;
- migrations;
- testing;
- audit;
- file storage;
- rules versioning;
- deployment;
- observability;
- backup/recovery;
- security boundaries.

No implementation before plan review.

### Gate C — Core build
Auth → Chronicle → Character → Sheet → XP → Change Requests.

### Gate D — Governance
Rules → Controlled Items → Approvals/Notifications → House Rules → audit.

### Gate E — Operations
Actions/Downtimes → import → operational dashboards.

### Gate F — Pilot
Seed/configure pilot chronicle, migrate controlled sample, acceptance testing, then broader migration.

---

## 14. Open Product Decisions Before Technical Implementation

These are not permission to guess; they require explicit resolution/specification:

1. Exact Vampire/MET sheet schema and supported books/material for launch.
2. Exact Brazilian pilot chronicle and staff roles.
3. Which Haller/Grapevine export formats we can legally/technically ingest.
4. Whether V1 sends email or only prepares/tracks external approval communication.
5. Exact notification channels inside the platform.
6. Whether NPC mechanical sheets in pilot need full parity with PCs at launch.
7. Item card scope in V1.
8. Status/Prestation scope in V1.
9. Whether visitor/travel workflow is V1 launch or first P1 release.
10. Portuguese-only launch vs bilingual data/UI foundation.

---

## 15. Required Specs Next

Before Claude writes product code, create:
1. `specs/character/VAMPIRE_SHEET.md`
2. `specs/character/XP.md`
3. `specs/character/CHANGE_REQUESTS.md`
4. `specs/rules/RULES_REGISTRY.md`
5. `specs/rules/CONTROLLED_ITEMS.md`
6. `specs/approvals/APPROVAL_WORKFLOW.md`
7. `specs/chronicle/ROLES_PERMISSIONS.md`
8. `specs/scenes/ACTIONS_DOWNTIMES.md`
9. `docs/DATA_MODEL.md`
10. migration investigation document.

---

## 16. Product Positioning

Haller-like tools prove the need for digital chronicle administration. This product differentiates by joining mechanical administration with explainable governance and narrative continuity.

**Short positioning:**
> A plataforma que entende não apenas o que está na ficha, mas por que está ali, quem autorizou, de onde veio e o que isso significa para a história.
