# Actions & Downtimes — V1

**Status:** Product specification draft  
**Scope:** Vampire/MET Chronicle operations and narrative-continuity foundation  
**Related:** docs/PRD.md, docs/NARRATIVE_ENGINE.md, specs/character/CHANGE_REQUESTS.md, specs/chronicle/ROLES_PERMISSIONS.md

## 1. Purpose

Define a structured workflow for player Actions/Downtimes that is immediately useful for Chronicle operations and can later feed the Narrative Engine without redesign.

An Action/Downtime captures:
1. what a Character intends/attempts;
2. what resources/context are committed;
3. what the Storyteller resolves;
4. what becomes canonical;
5. what the Character learns;
6. what mechanical/governance consequences are proposed;
7. what future story hooks emerge.

These layers must not collapse into one text field.

## 2. Core Narrative Separation

### Player Intent
What the player says the Character is attempting.

This is not automatically true in the fiction.

### ST Resolution
The Storyteller's adjudication of the attempt.

This may contain private/canonical information the Player is not allowed to see.

### Player-Facing Outcome
What the Character perceives/is told as the result.

This is not necessarily the complete Event Truth.

### Canonical Consequences
Structured facts/events/relationships/changes that the ST confirms as canon.

### Character Knowledge
Information the Character legitimately knows after resolution.

### Rumor
Information circulating as rumor, which may be true, false, partial or distorted.

## 3. Terminology

**Action** is the generic system entity.

"Downtime" is an Action type or presentation mode used for between-session activity.

Other future Action types may include:
- investigation;
- influence action;
- training;
- research;
- social/political action;
- crafting;
- travel;
- feeding/hunting where Chronicle uses it;
- project;
- personal objective;
- crossover request;
- other Chronicle-defined categories.

V1 should not hardcode every possible Vampire activity as a separate database entity.

## 4. Action Record

Minimum fields:
- id
- character_id
- player_id / submitted_by
- home_chronicle_id
- handling_chronicle_id
- type/category
- title
- intent
- method/approach
- target references
- location references
- involved character/NPC/org references
- declared resources
- supporting notes
- visibility
- status
- submitted_at
- effective/fictional period
- assigned ST(s)
- resolution metadata
- created_at/updated_at
- provenance

Typed references are preferred over names embedded only in prose, while prose remains available for expressive roleplay.

## 5. Lifecycle

Baseline:

DRAFT → SUBMITTED → IN_REVIEW → RESOLVED

Additional:
- NEEDS_INFO
- WITHDRAWN
- CANCELLED
- ROUTED
- BLOCKED_DEPENDENCY
- SUPERSEDED

An Action may be resolved with:
- SUCCESS
- PARTIAL_SUCCESS
- FAILURE
- INCONCLUSIVE
- NARRATIVE_RESULT
- other Chronicle-defined outcome vocabulary

Mechanical systems must not assume every Action is a binary test.

## 6. Player Submission

Player can provide:
- objective/intention;
- description of approach;
- relevant traits/abilities/powers they intend to use;
- resources/influences/backgrounds/equipment they commit;
- target/location;
- NPCs/Characters involved;
- desired fictional timing;
- attachments/references where permitted;
- private note to ST where appropriate.

Selecting a mechanical trait does not guarantee it is applicable. It is a declaration for ST/rules review.

## 7. ST Review

ST view should show:
- Character identity;
- submitted intent;
- relevant authorized sheet subset;
- declared resources;
- linked Characters/NPCs/locations;
- prior related Actions where authorized;
- pending dependencies;
- rules/compliance hints where configured;
- Chronicle context/notes;
- resolution editor.

ST may:
- request more information;
- reassign/route;
- collaborate with another authorized ST;
- resolve;
- create structured consequences;
- attach secret canonical notes;
- create player-facing response.

## 8. Resolution Model

Resolution contains distinct layers.

### Internal Resolution
ST-only or restricted:
- actual adjudication;
- hidden opposition;
- secret facts;
- rolls/tests/results if recorded;
- rationale;
- consequences not yet revealed.

### Player-Facing Resolution
What the Player/Character receives:
- result narrative;
- perceived consequences;
- information discovered;
- next options/hooks visible to them.

### Structured Outputs
Proposed or confirmed links to:
- Event(s)
- Character Knowledge
- Relationship changes
- Rumors
- Change Requests
- XP awards/spends where policy allows
- equipment/items
- Standing/Boons/other political records
- governance requirements
- future Actions
- Plot/Story hooks
- NPC changes
- Location changes

V1 does not need all target modules implemented to preserve typed output proposals/references.

## 9. Event Truth Integration

Resolving an Action does not mean the player's submitted description becomes Event Truth.

Example:
Player: "I follow NPC X and discover that he works for Organization Y."

Possible truth:
- Character attempted surveillance;
- NPC X went to Location Z;
- no canonical evidence connects X to Organization Y.

Player-facing knowledge may still include an inference/suspicion, but it must not be stored as confirmed Event Truth.

Future Narrative Engine creates/links Event records only from ST-confirmed output.

## 10. Character Knowledge Integration

A resolved Action can grant Knowledge:
- observed fact;
- statement heard;
- document found;
- inference;
- identity learned;
- location learned;
- secret learned;
- uncertain clue.

Knowledge should support:
- knower Character;
- proposition/reference;
- confidence/type;
- source Action/Event;
- learned_at;
- visibility;
- whether confirmed IC vs suspected.

Do not make all ST Resolution content automatically known to the Character.

## 11. Rumor Integration

ST may generate a Rumor from an Action or its consequences.

Rumor must remain separate from:
- actual Event Truth;
- Character Knowledge.

Rumor can record:
- content;
- origin if known;
- distribution scope;
- propagation history;
- truth relationship restricted to authorized STs.

## 12. Mechanical Consequences

An Action may justify/propose a mechanical acquisition, but it does not directly mutate the sheet.

Example:
Training Action resolves successfully and supports learning a Discipline.

Output:
- acquisition evidence/context;
- teacher;
- effective date;
- source Action;
- proposed Change Request.

Then normal flow applies:
Change Request → Rules → Governance → Approval → Sheet + XP.

This preserves the narrative reason without bypassing mechanical governance.

## 13. XP Consequences

Chronicle policy may award XP for sessions/actions/etc.

Action can be linked as source of an XP EARN transaction, but Action resolution itself does not autonomously award XP unless an explicitly configured deterministic policy authorizes it.

No AI-created XP awards.

## 14. Resources & Commitments

Declared resources can reference:
- Backgrounds
- Influences
- Abilities
- Disciplines
- Equipment
- NPC/Allies/Contacts
- other approved holdings.

V1 distinction:
- **declared**: Player says they use/commit it;
- **validated**: system confirms Character currently possesses it;
- **adjudicated**: ST decides its effect in this Action.

Do not conflate possession with automatic success.

## 15. Targets and References

Action may target/reference:
- Character
- NPC
- Organization
- Location
- Item
- Chronicle
- Plot
- prior Action/Event
- free-text unresolved entity

Free-text unresolved references should be convertible later to canonical entities without rewriting the historical submission.

## 16. NPC Actions

NPCs are first-class.

Authorized ST can create Actions for NPCs.

NPC Action:
- has no Player submission requirement;
- may be entirely restricted;
- can generate Events/Knowledge/Rumors/relationships just like PC Actions;
- remains controlled by NPC permissions.

This is essential for Chronicle Memory.

## 17. Collaborative / Multi-Character Actions

An Action may involve multiple Characters.

V1 should distinguish:
- primary acting Character;
- participants;
- targets/subjects.

Participant visibility is explicit. Joining an Action does not automatically expose every participant's private submission or sheet.

Future enhancement may support joint player submission; V1 can model linked Actions if simpler.

## 18. Cross-Chronicle Actions

If an Action affects another Chronicle:
- home Chronicle retains Character authority;
- handling/affected Chronicle can be invited through explicit routing/agreement;
- shared fields are purpose-limited;
- each Chronicle can retain restricted notes;
- final shared facts require appropriate narrative agreement where needed.

No Chronicle gets blanket access to the other's data.

## 19. Routing

Action can be routed based on:
- assigned ST;
- category;
- NPC owner;
- plot owner;
- affected Chronicle;
- governance dependency.

Routing is operational, not narrative truth.

Route history is audited.

## 20. Deadlines / Cycles

Chronicle may configure Action cycles:
- submission opens;
- deadline;
- review period;
- expected response period.

V1 should support optional cycles without requiring every Chronicle to use them.

Cycle:
- name
- chronicle
- open/close dates
- categories allowed
- per-character limits if policy exists
- status

Limits are Chronicle policy/rule data, not universal assumptions.

## 21. Templates

Chronicle may create Action Templates.

Template fields:
- name/category;
- player guidance;
- required questions;
- optional structured fields;
- visibility;
- rules/source if mechanically governed;
- active version.

Templates improve consistency but do not replace freeform narrative.

## 22. Attachments / Sources

Action may reference/upload supporting material where storage supports it.

Attachment metadata:
- owner/source;
- visibility;
- file/reference;
- purpose;
- uploaded_at;
- provenance.

Future Scene Intelligence can analyze authorized sources only.

## 23. AI Assistance

Allowed for Player:
- help structure an Action from player's own authorized context;
- improve clarity without changing intent;
- suggest missing questions.

Allowed for ST:
- summarize submissions;
- propose relevant prior continuity;
- propose resolution consequences;
- extract candidate Events/Knowledge/Relationships/Rumors;
- flag possible mechanical/governance follow-up.

AI outputs are proposals.

Not allowed:
- resolve an Action canonically without ST confirmation;
- expose hidden opposition/secrets to Player;
- mutate sheet;
- award XP discretionarily;
- create Event Truth automatically;
- infer secret data outside authorized retrieval.

## 24. Narrative Extraction

After ST writes/resolves an Action, future Narrative Intelligence may propose:

**Candidate Event**
"Character A met NPC B at Location C."

**Candidate Knowledge**
"Character A learned that B uses alias D."

**Candidate Relationship**
"A distrusts B."

**Candidate Promise**
"B promised A access to E."

**Candidate Mechanical Acquisition**
"A trained Power F with B."

**Candidate Rumor**
"Members of the domain are saying G."

Each candidate:
- source Action;
- extracted text span/reference where possible;
- confidence;
- visibility;
- proposed structured entities;
- status: PROPOSED | CONFIRMED | EDITED | REJECTED;
- confirmed_by.

No candidate becomes canon solely because AI extracted it.

## 25. Promises & Unresolved Hooks

Actions often create future obligations without mechanical change.

Preserve candidate:
- promise/commitment;
- owed by/to;
- expected condition/date;
- source;
- resolved/open state;
- visibility.

This becomes part of future Chronicle Memory.

## 26. Relationship Consequences

Action can propose:
- knows;
- trusts/distrusts;
- owes;
- allied;
- hostile;
- mentor/student;
- employer/agent;
- blood/family/ghoul relationship;
- custom relation.

Relationship Truth and a Character's perception of relationship may differ; future Relationship spec must preserve this distinction.

## 27. Search & Chronicle Memory

Authorized ST should eventually be able to ask:
- What unresolved Actions involve NPC X?
- What did Character Y learn about Organization Z?
- Which promises remain open?
- Which Actions created pending mechanical acquisitions?
- What happened in this location recently?

V1 data shape must make these queries possible even if advanced UI arrives later.

## 28. Audit & Immutability

Preserve:
- original submitted version;
- edits/version history after submission;
- requests for information;
- ST resolution versions;
- final resolution;
- structured outputs;
- actor/timestamps.

Do not overwrite Player's original intent with ST interpretation.

## 29. Permissions

Player:
- DRAFT/EDIT/SUBMIT own Action;
- READ own authorized player-facing content;
- answer NEEDS_INFO;
- WITHDRAW when policy allows.

ST:
- DISCOVER/READ according to Chronicle assignment;
- REVIEW/ROUTE/RESOLVE according to capabilities;
- create restricted internal content.

HST:
- broad Chronicle oversight according to role policy.

Network ST:
- only explicitly routed/shared Action subset.

Admin-only:
- no content access by default.

## 30. V1 UI

### Player
**AÇÕES**
- New Action
- Drafts
- Submitted
- Needs Info
- Resolved

Action detail:
- intent
- approach
- references/resources
- status/timeline
- ST questions
- player-facing resolution
- visible consequences/follow-ups

### ST
**ACTION INBOX**
- Submitted
- Assigned to me
- Needs Info
- Cross-Chronicle
- Blocked
- Resolved

Review workspace:
- submission
- relevant context
- internal notes
- resolution
- structured consequence proposals
- routing/dependencies

## 31. V1 vs Future

### V1 required
- Action entity
- PC/NPC origin
- drafts/submission/review/resolution
- categories/templates foundation
- ST assignment/routing
- internal vs player-facing resolution
- typed references/resources
- audit/history
- permission enforcement
- ability to link Change Request/XP/governance
- structured output/proposal foundation

### Future expansion
- Scene Intelligence extraction
- automatic candidate graph links
- full Event Truth UI
- Character Knowledge UI
- Rumor propagation
- Plot management
- promise tracking dashboard
- relationship graph
- advanced cross-Chronicle narrative agreements
- WhatsApp/Discord ingestion

## 32. Acceptance Criteria

1. Player intent is preserved separately from ST resolution.
2. ST secret resolution can differ from player-facing outcome.
3. Action resolution cannot directly bypass Change Request for sheet changes.
4. NPC Actions are supported.
5. Typed Character/NPC/location/etc. references are possible.
6. Unresolved free-text references are preserved.
7. Declared resources are distinct from validated possession and adjudicated effect.
8. Cross-Chronicle access is explicit and scoped.
9. Original submission and resolution history are auditable.
10. Future Event/Knowledge/Rumor extraction can link back to source Action.
11. AI outputs remain proposals.
12. Action data does not collapse Event Truth, Character Knowledge and Rumor.
13. Mechanical acquisitions can retain narrative provenance.
14. Player cannot see hidden ST context through search/AI/metadata.
15. V1 remains useful even before full Narrative Engine exists.

## 33. Pilot Decisions Still Needed

- terminology shown to Brazilian players: "Ações", "Downtimes" or both;
- whether Chronicle uses submission cycles/deadlines;
- per-cycle Action limits, if any;
- initial Action categories/templates;
- whether Players may edit after SUBMITTED before review;
- whether multi-character joint Actions ship in V1 or as linked individual Actions;
- whether ST resolution records dice/tests in structured form;
- what player-facing history remains visible indefinitely;
- whether Action attachments are required in pilot.
