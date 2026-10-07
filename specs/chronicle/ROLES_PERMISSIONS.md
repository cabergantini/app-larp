# Roles & Permissions — V1

**Status:** Product specification draft  
**Scope:** Authentication authorization and field-level visibility for Vampire/MET V1  
**Related:** docs/PERMISSIONS.md, docs/PRD.md, specs/character/VAMPIRE_SHEET.md, specs/approvals/APPROVAL_WORKFLOW.md

## 1. Purpose

Define who may discover, retrieve, view, create, propose, review, approve, administer and export data.

The platform contains mechanical, narrative, governance and potentially highly secret fictional information. Authorization must be enforced before retrieval, not only hidden in the UI.

## 2. Core Model

Authorization is the intersection of:

**User identity + Role assignment + Scope + Object relationship + Field visibility + Workflow state + Explicit grants/restrictions**

A role alone is never a universal bypass.

## 3. Visibility Vocabulary

Baseline classifications:

- **PUBLIC** — intentionally publishable without authenticated Chronicle membership.
- **PLAYER** — accessible to the owning/authorized Player in the appropriate context.
- **CHRONICLE_ST** — accessible to authorized Storyteller staff of the controlling/relevant Chronicle.
- **NETWORK_ST** — cross-Chronicle Storyteller visibility only where explicitly authorized/needed.
- **COORDINATOR** — accessible to the relevant Coordinator office/workflow scope.
- **RESTRICTED** — explicit allow-list/policy required; not implied by ordinary role.
- **SYSTEM** — internal operational/security data not normally exposed as content.

Visibility classification is metadata/policy, not merely a UI label.

## 4. Role Vocabulary

### Player
A participant controlling one or more PCs.

Typical capabilities:
- view own authorized Character data;
- create/edit drafts;
- propose post-creation mechanical changes;
- submit Actions/Downtimes;
- view own XP/history/requests;
- provide governance-request information;
- manage public identity content subject to publication workflow.

Does not:
- mutate official sheet directly;
- award XP;
- approve own mechanical changes;
- read ST secrets merely because they concern the Character.

### Storyteller (ST)
Chronicle-scoped narrative/operational staff.

Typical capabilities according to granted Chronicle permissions:
- view/manage relevant PCs and NPCs;
- review Change Requests;
- resolve Actions/Downtimes;
- record XP;
- create Chronicle narrative records;
- prepare governance requests;
- access Chronicle-level secrets needed for assigned function.

ST is scoped to Chronicle; it is not automatic global access.

### Head Storyteller (HST)
Chronicle-scoped lead Storyteller.

Typically inherits broad Chronicle storytelling authority and may:
- manage ST role assignments where policy permits;
- publish House Rules;
- make/record final local decisions;
- authorize Chronicle-level exports;
- control high-sensitivity Chronicle data.

Exact HST powers remain configurable to organizational/pilot policy.

### Chronicle Admin
Technical/operational administrator.

May:
- manage Chronicle configuration;
- invitations/accounts/technical membership;
- operational settings;
- integrations where authorized.

**Critical:** Chronicle Admin does not automatically receive ST narrative/mechanical secret access.

A user may hold both Admin and ST, but those are separate grants.

### Network ST
Cross-Chronicle storytelling role for a specific collaboration, event, shared plot, visiting Character or agreement.

Access is purpose/scope limited.

Network ST is not "all STs can see all Chronicles."

### Coordinator
Represents an authenticated holder/delegate of a governance Office.

May access:
- governance cases addressed to their Office;
- minimum Character/context data required for those cases;
- other Coordinator workspace data explicitly authorized by future specs.

Coordinator role does not imply blanket Chronicle database access.

### System
Service actor for deterministic operations, jobs, imports and integrations.

System actions:
- are auditable;
- execute under explicit service permissions;
- do not bypass content authorization for convenience;
- cannot make discretionary narrative/governance decisions.

## 5. Role Assignment

Role Assignment fields:
- user_id
- role
- scope_type
- scope_id
- effective_from
- effective_to
- status
- granted_by
- reason/source
- delegated_from nullable
- audit metadata

Scopes may include:
- Chronicle
- Network Agreement
- Event
- Governance Office
- specific object/case
- future organization scope

Avoid permanent global roles unless product governance explicitly requires them.

## 6. Character Ownership & Control

PC:
- controlled by Player account(s) according to Chronicle policy;
- governed mechanically by home/controlling Chronicle.

NPC:
- controlled by Chronicle or explicitly assigned narrative authority;
- may have delegated/shared access.

Character control is separate from visibility.

A Player controlling a PC does not necessarily see every field stored about that PC.

## 7. Field-Level Visibility

An object may contain fields with different visibility.

Example Character:
- public name: PUBLIC
- official mechanical sheet: PLAYER + relevant CHRONICLE_ST
- ST-only secret: CHRONICLE_ST
- restricted plot truth: RESTRICTED
- governance evidence: relevant CHRONICLE_ST/COORDINATOR according to workflow
- public biography: PUBLIC after publication

API/query authorization must filter fields before response serialization.

## 8. Notes

Every Note has:
- owner/author
- subject
- visibility
- explicit allowed scopes/users if RESTRICTED
- provenance
- timestamps

Suggested note types:
- PLAYER_NOTE
- ST_NOTE
- CHARACTER_NOTE
- GOVERNANCE_NOTE
- NARRATIVE_SECRET
- IMPORT_NOTE

A Player Note's exact staff visibility is an open product decision and must be explicit in UI before users rely on privacy expectations.

## 9. Mechanical Sheet

Player:
- reads official authorized sheet;
- reads own pending proposals;
- proposes changes.

Relevant Chronicle ST:
- reads official sheet;
- reviews proposals;
- may initiate staff-originated changes;
- sees governance/compliance details according to role.

Network ST:
- receives only fields explicitly shared for crossover/event purpose.

Coordinator:
- receives only relevant fields required for governance case unless separate authorization exists.

## 10. XP

Player:
- own balance/ledger subject to Chronicle display policy;
- cannot alter.

Authorized Chronicle ST/HST:
- award/correct/review according to permission.

Admin-only role:
- no XP modification by default.

Network ST/Coordinator:
- no general XP access unless a specific workflow requires a minimal relevant subset.

## 11. NPC Permissions

NPC is first-class and may be highly secret.

NPC record supports:
- controlling Chronicle
- controlling ST group/users
- visibility
- share grants
- shared narrative agreement

A Player may know an NPC IC without gaining access to the NPC's canonical/mechanical record.

Public NPC identity, Character Knowledge and Event Truth are separate concerns.

## 12. Narrative Data

Future narrative objects follow the same principle:

### Event Truth
Visible only to scopes authorized to know canonical truth.

### Character Knowledge
Visible to the controlling Player/ST according to policy and to specifically authorized narrative staff.

### Rumor
Visibility/distribution is part of the Rumor object and is not equivalent to Event Truth access.

No role may infer Truth access merely because it can see a Rumor.

## 13. Actions / Downtimes

Player:
- create/view own submissions and permitted responses.

Chronicle ST:
- review/resolve submissions within Chronicle assignment.

Network ST:
- access only when Action is explicitly shared/routed through a crossover agreement.

Coordinator:
- no access unless Action is attached as relevant evidence to a governance case, and then only the necessary authorized excerpt/data.

## 14. Governance Cases

Request package uses minimum necessary disclosure.

Player may see:
- status;
- nonrestricted requirements;
- requests for information;
- decision/result appropriate to their request.

Chronicle ST sees case operational details according to role.

Coordinator sees cases addressed to their Office.

Restricted internal notes/correspondence can remain separately protected.

Decision authority and data visibility are distinct permissions.

## 15. House Rules & Rules Corpus

Published rules:
- readable by relevant users;
- source/version visible.

Draft rule:
- restricted to authorized rules editors/reviewers.

Only authorized HST/rules role may publish Chronicle House Rules.

Technical Admin cannot publish a rule solely by being Admin.

## 16. Cross-Chronicle Sharing

No implicit global sharing.

Cross-Chronicle access requires a Share Grant / Narrative Agreement / Event participation or other explicit product relationship.

Share Grant fields:
- source Chronicle
- recipient Chronicle/user/role
- subject/object
- permitted fields/categories
- purpose
- effective period
- onward-sharing policy
- granted by
- revocation
- audit

Default: deny data not included in the grant.

## 17. Visiting Characters

A visiting Character does not become owned by host Chronicle.

Host access should be purpose-limited and may include:
- identity/public presentation;
- agreed mechanical subset needed to run game;
- relevant approvals/status;
- temporary event facts.

Home Chronicle retains authoritative ownership unless a formal transfer occurs.

## 18. Explicit Restricted Grants

RESTRICTED content uses explicit access policy.

Grant:
- principal (user/role/scoped group)
- object/field category
- permission
- purpose
- expiry
- granted by
- audit

Revocation affects future access, not historical audit of actions previously performed.

## 19. Permission Actions

Capabilities should be explicit, e.g.:
- DISCOVER
- READ
- CREATE
- EDIT_DRAFT
- SUBMIT
- REVIEW
- RESOLVE
- APPROVE_LOCAL
- AWARD_XP
- CORRECT_XP
- PUBLISH_RULE
- SHARE
- EXPORT
- MANAGE_MEMBERS
- MANAGE_ROLES
- MANAGE_INTEGRATIONS
- RECORD_EXTERNAL_DECISION

Avoid one generic EDIT permission for all domains.

## 20. Discovery vs Read

A user may lack permission even to know an object exists.

Therefore distinguish:
- DISCOVER: object can appear in search/list/reference picker.
- READ: content can be retrieved.

Secret NPC/plot names must not leak through search, autocomplete, counts, URLs, error messages or AI retrieval.

## 21. Exports

Export is a separate capability.

Export must:
- re-run authorization;
- include only permitted fields;
- record actor/time/purpose where sensitive;
- prevent hidden fields from leaking into PDF/CSV/JSON metadata.

Public/Wiki export uses Public Identity, not private sheet by default.

## 22. AI Authorization

Before any AI context assembly:
1. authenticate user;
2. resolve roles/scopes;
3. authorize requested objects;
4. filter fields;
5. retrieve allowed content;
6. only then provide context to AI.

Never retrieve everything and ask the model to ignore secrets.

AI-generated answer inherits source visibility and must not widen access.

## 23. Search & World Graph

Search results are authorization-aware.

Graph edges may themselves be secret.

User must not learn hidden relationships from:
- edge counts;
- suggested NPCs;
- "related characters";
- embeddings;
- autocomplete;
- graph topology.

Future World Graph queries must filter nodes and edges before presentation/retrieval.

## 24. Audit

Sensitive actions to audit include:
- role grant/revoke;
- restricted-data access where policy requires;
- sheet review/commit;
- XP award/correction;
- governance access/decision recording;
- export;
- share grant;
- House Rule publication;
- import;
- permission override.

Audit itself has restricted visibility.

## 25. Emergency / Break-Glass Access

Do not implement invisible superuser browsing.

If operationally necessary later, break-glass access must require:
- explicit capability;
- reason;
- short-lived access;
- prominent audit;
- post-access review/notification according to policy.

V1 may omit break-glass entirely.

## 26. Deny by Default

When policy cannot determine access:
**DENY / MANUAL AUTHORIZATION**

Do not infer access from:
- same sect;
- same clan;
- friendship;
- being an ST elsewhere;
- knowing a Character IC;
- receiving a Rumor;
- being technical Admin.

## 27. Initial Permission Matrix

| Domain | Player own | ST Chronicle | HST Chronicle | Admin-only | Network ST | Coordinator |
|---|---|---|---|---|---|---|
| Own PC official sheet | Read | Scoped read/review | Read/review | No default | Shared subset | Case subset |
| Other PC private sheet | No | Scoped | Scoped | No default | Shared subset | Case subset |
| XP modify | No | If granted | Yes/configurable | No | No | No |
| Change Request review | No | If granted | Yes | No | Shared case only | Governance decision only |
| NPC canonical data | No by default | Scoped | Scoped | No default | Explicit share | Case subset |
| ST secrets | No | Scoped | Scoped | No default | Explicit share | No default |
| House Rule publish | No | If specifically granted | Yes/configurable | No | No | No |
| Member administration | No | No default | Configurable | Yes | No | No |
| Governance case | Own visible subset | Scoped | Scoped | No default | If explicitly shared | Addressed Office |
| Cross-Chronicle data | No default | Explicit share | Explicit share | No default | Agreement scope | Case scope |
| Public Identity | Read/edit own draft | Review | Review/publish policy | Technical only | Public | Public |

This matrix is a baseline; final pilot role configuration must be specified before implementation.

## 28. Acceptance Criteria

1. Technical Admin does not automatically see narrative/mechanical secrets.
2. ST access is Chronicle-scoped.
3. Coordinator sees case-relevant data, not entire Character database.
4. Cross-Chronicle access is explicit.
5. NPC existence can itself be hidden.
6. Field-level authorization occurs before retrieval/AI context.
7. Discover and Read are separate.
8. Export cannot bypass field permissions.
9. Public Identity is separate from private sheet.
10. Character Knowledge/Rumor do not imply Event Truth access.
11. Role grants/revocations are effective-dated and audited.
12. Sensitive actions use explicit capabilities rather than generic edit.
13. Unknown authorization denies by default.
14. Search/autocomplete cannot leak secret entities.
15. System/service actor remains auditable.

## 29. Pilot Decisions Still Needed

Before implementation:
- exact ST vs HST division in pilot Chronicle;
- whether Assistant ST/subroles are needed;
- Player Notes privacy contract;
- who may award/correct XP;
- who may publish House Rules;
- who may export full sheets;
- whether Chronicle Admin is a separate person/role in pilot;
- minimal visiting-character data shared with host Chronicle;
- whether Coordinator authenticated access exists in V1 or external workflow only.
