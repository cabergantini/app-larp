# Vampire/MET Character Sheet Specification — V1

**Status:** Product specification draft  
**Scope:** Vampire: The Masquerade / MET only  
**Reference artifact:** HallerGames character sheet PDF supplied from a real OWBN character  
**Related:** docs/PRD.md, DECISIONS.md, docs/RULES_ENGINE.md, docs/PERMISSIONS.md

## 1. Purpose

Define the functional shape of the V1 Vampire/MET character record and mechanical sheet.

This specification intentionally distinguishes:
- **Observed** — present in the supplied Haller sheet.
- **Product decision** — how this platform should model it.
- **Rules-dependent** — exact catalog, costs, limits or legality must come from approved rules sources and must not be invented here.

The goal is familiarity without reproducing Haller's storage model.

## 2. Core Principle

**Character != Sheet.**

Character is the durable entity. The mechanical sheet is one authoritative, versioned aspect of Character.

Character also owns/relates to:
- identity;
- XP ledger;
- change requests;
- governance requests;
- notes;
- Actions/Downtimes;
- equipment/items;
- status/prestation;
- later: relationships, knowledge, scenes, rumors and narrative memory.

## 3. Classification Vocabulary

Every field/entity should be classified as one or more of:

- **IDENTITY** — identifies/describes the character.
- **MECHANICAL** — participates in game mechanics.
- **DERIVED** — calculated from other authoritative data.
- **NARRATIVE** — fiction/continuity information.
- **REGULATED** — may trigger OWBN/Chronicle governance.
- **HISTORICAL** — event/ledger/version data.
- **GOVERNANCE** — approval, authority, evidence, rule source.
- **OPERATIONAL** — workflow/admin data.

## 4. Character Header

### Observed in reference
- Character name
- Chronicle/location
- Player
- Status
- Current XP
- Total XP
- Date Printed
- Last Modified
- Starting Date
- Clan
- Sect
- Title
- Faction
- Coterie/Pack
- Nature
- Demeanor
- Path
- Path Rating
- Generation
- Conscience/Conviction
- Self-Control/Instinct
- Courage
- Willpower
- Blood

### Product model

#### Character identity
- id
- type: PC | NPC
- canonical_name
- player_id nullable for NPC
- home_chronicle_id / controlling_chronicle_id according to type/workflow
- lifecycle
- start_date
- clan_id or approved custom reference
- bloodline_id nullable
- sect_id
- faction_id nullable
- coterie_id nullable
- nature
- demeanor
- concept
- visibility/public identity references

#### Political/office data
Do not store all titles as one free-text field.
A Character Position/Office record should support:
- title/position
- organization/sect/domain
- effective_from
- effective_to
- source/provenance
- visibility
- active state

#### Morality/Virtues
Model Path/Morality and virtue configuration structurally:
- morality/path type
- rating
- virtue types and ratings
- ruleset/source where required

Do not assume every Vampire configuration uses exactly Humanity + Conscience + Self-Control. Catalog/rules define valid combinations.

#### Resources
Willpower and Blood require:
- maximum/capacity where applicable
- current/transient value where product supports live resource tracking
- source of derived maximum if derived
- audit rules for permanent changes

**Open decision:** whether V1 sheet is primarily permanent character state or also live session resource tracker.

## 5. Attributes / Traits

### Observed
Three positive trait categories:
- Physical
- Social
- Mental

Each contains named traits with repeated quantity, e.g. Agile x3, Dexterous x2.

Negative Physical/Social/Mental are separate sections.

### Product decision
Do not encode repeated trait names as display strings.

Trait Holding:
- character_id
- trait_catalog_id
- category
- polarity: POSITIVE | NEGATIVE
- quantity
- source/provenance
- acquired/effective metadata
- official state

Trait Catalog:
- stable id
- name
- category
- polarity
- source/ruleset/version
- active/deprecated
- custom/local marker

UI may render dots/counts for MET familiarity.

Rules-dependent:
- valid trait catalog;
- duplicate limits;
- category totals;
- creation rules;
- XP cost;
- exceptions.

## 6. Abilities

### Observed
Abilities have:
- name
- rating/count
- optional specialization/descriptor in parentheses
Examples in reference include Crafts specializations, Brawl, Empathy, Etiquette, Expression and Flight descriptors.

### Product decision
Ability Holding:
- ability_catalog_id
- rating
- specialization(s) / descriptor(s), structured where possible
- source/provenance
- official state

Ability Catalog:
- name
- source/version
- default limits/rules references
- custom/local flag

Do not bake specialization into the ability's canonical name.

Rules-dependent:
- legal catalog;
- rating caps;
- specialization behavior;
- XP costs;
- controlled/custom abilities.

## 7. Lores

### Observed
Lores appear among Abilities, e.g. Lore: Camarilla, Lore: Clan: Brujah, Lore: Kindred.

### Product decision
For UX, Lore may appear in the Abilities area if that matches rules/user expectation, but data should distinguish Lore subtype because it can carry special regulation, secrecy and approval requirements.

Lore Holding:
- lore_catalog_id
- rating
- scope/topic
- provenance
- governance state where applicable
- visibility

This prepares later Character Knowledge without treating mechanical Lore and narrative knowledge as the same thing.

## 8. Disciplines & Powers

### Observed
The reference mixes:
- Discipline paths/power names;
- levels represented by x2/x3/etc.;
- clan-specific powers;
- combination disciplines;
- free-text acquisition notes such as "Learned with Dawn";
- book/source shorthand in text.

### Product decision
This must be normalized.

Discipline Catalog:
- discipline
- power
- level/tier/order where applicable
- family/type: standard | clan-specific | combination | blood magic | other supported
- source publication
- rules version/reference
- regulation metadata linkage

Character Power Acquisition:
- character_id
- power_catalog_id
- acquired_at
- acquisition_type
- teacher_character_id / teacher_npc_id nullable
- teacher_free_text only as migration fallback
- narrative_source_id nullable
- XP transaction/change_request
- rule evaluation
- governance request(s)
- approval evidence
- notes
- official state

**Critical:** "Learned with Dawn" is not part of a power name.

Rules-dependent:
- discipline progression;
- in-clan/out-of-clan classification;
- prerequisites;
- costs;
- teacher requirements;
- controlled items;
- combination discipline requirements.

## 9. Backgrounds

### Observed
Named backgrounds with ratings, including Allies, Alternate Identity, Contacts and Resources.

### Product decision
Background Holding:
- background_catalog_id
- rating
- descriptor/name where background instance needs identity
- linked entity where appropriate
- source/provenance
- regulation/governance state

Important future behavior:
A background such as Ally should be capable of linking to a first-class NPC/organization rather than existing only as text.

Rules-dependent:
- catalog;
- rating/cost;
- creation vs XP behavior;
- special approvals/restrictions.

## 10. Influences

### Observed
Influences are a separate rated section (Finance, Health, High Society, Legal, Media, Military, Police, Politics, Transportation, Underworld).

### Product decision
Influence Holding:
- influence_catalog_id
- rating
- descriptor/specialization if applicable
- geographic/chronicle scope if rules require
- provenance
- official state

Do not infer the definitive influence catalog from this single character sheet.

## 11. Rituals and Rites

### Observed
Separate Rituals and Rites sections; both empty in reference.

### Product decision
Keep separate conceptual categories in the sheet specification until rules research proves they can share a generic implementation safely.

Each acquisition should support:
- catalog item
- category
- level/type
- source
- teacher/source of learning
- XP/change request
- rule evaluation
- approval/governance
- provenance

## 12. Merits & Flaws

### Observed
Merits and Flaws list names with point values in parentheses. One flaw includes an additional free-text descriptor.

### Product decision
Merit/Flaw Catalog:
- name
- kind: MERIT | FLAW
- base value/range if rules define it
- source/version
- regulation metadata

Character Merit/Flaw:
- catalog id
- selected value where variable
- descriptor
- acquisition provenance
- change request
- governance
- official state

Do not treat parenthetical text indiscriminately as one field: numeric value and narrative descriptor are semantically different.

## 13. Standing / Negative Standing

### Observed
Standing is represented as a list. Entries may include:
- standing name;
- who granted it;
- sometimes date/context.
Negative Standing has its own section.

### Product decision
Standing is not plain text.

Standing Record:
- type/catalog
- polarity: POSITIVE | NEGATIVE
- character
- granting_character/entity
- granting_title/domain at time
- granted_at
- effective_from/to
- active/revoked state
- reason/context
- source/provenance
- visibility

This model prepares Prestation/Status history and political continuity.

Exact OWBN Status mechanics require a dedicated spec; do not infer them from this PDF alone.

## 14. Derangements

### Observed
Separate Derangements section.

### Product decision
Derangement Holding:
- catalog/custom reference
- descriptor
- mechanical metadata
- visibility classification
- source/provenance
- governance if applicable

Sensitive narrative/character information must follow field-level authorization.

## 15. Health

### Observed
Health Levels are grouped as Healthy, Bruised, Wounded, Incapacitated and Torpor with quantities.

### Product decision
Separate:
- health track definition/capacity (sheet state)
- current damage/session state, if live tracking is included.

Do not assume counts from the sample are universal.

Rules-dependent:
- track configuration;
- modifiers from merits/powers/etc.;
- damage semantics.

## 16. Equipment & Items

### Observed
Equipment mixes mundane objects, tools, weapon, cellphone, clothing and vehicle in free-form/display-oriented entries.

### Product decision
Equipment should support:
- item instance
- item catalog/type
- name
- mechanical stats where relevant
- mundane/special/custom
- owner/controller
- source/acquisition
- item card/evidence
- regulation/governance
- visibility
- notes

A weapon's damage/stat block should not be embedded solely in its display name.

Mundane items may use a lighter-weight entry.

## 17. Miscellaneous

### Observed
A generic Miscellaneous section exists.

### Product decision
Do not make Miscellaneous the escape hatch for mechanically meaningful data.

Allow general notes, but anything affecting mechanics/governance should be represented by an appropriate typed entity or explicit Custom Mechanical Entry routed to review.

## 18. Boons / Prestation

### Observed
A Boons section exists, empty in reference.

### Product decision
Boons must eventually be relational records, not notes:
- creditor
- debtor
- boon type
- status
- date
- witnesses/Harpy where applicable
- transfer/history
- source
- visibility

Exact Prestation behavior is a separate product/rules spec. V1 launch inclusion remains an open PRD decision.

## 19. Blood Bonds & Vinculum

### Observed
Separate Blood Bonds and Vinculum sections.

### Product decision
These are relationship/mechanical records, not plain text.

Relationship Bond:
- source character/entity
- target character/entity
- bond type
- rating/stage where applicable
- timestamps
- source/provenance
- visibility
- mechanical/rule metadata

They should later integrate with Relationships while preserving strict secrecy.

## 20. Biography, Notes, Player Notes

### Observed
Biography, Notes and Player Notes are distinct display sections.

### Product decision
Separate content by purpose and authorization.

- Biography: narrative profile/content; may have public/private variants.
- Character Notes: typed notes with visibility.
- Player Notes: clarify ownership/visibility before implementation.
- ST Notes: not observed on this printout but required by broader product needs.

No universal text blob should expose all categories to the same audience.

## 21. XP Summary & Ledger

### Observed
Header shows Current XP and Total XP. XP Log contains historical operations such as:
- Earn
- Spend
- Set Earned
- Set Unspent
- Other
- transfers
- actor/logger
- description
- amount
- earned total
- unspent total

Historical data also demonstrates malformed/legacy dates and bulk textual operations.

### Product decision
XP totals are DERIVED from an auditable ledger whenever feasible.

XP Transaction:
- id
- character
- transaction_type
- amount
- occurred_at
- recorded_at
- actor
- chronicle
- reason
- linked change request
- linked source/event/action
- import provenance
- correction relation
- metadata

Suggested transaction types:
EARN, SPEND, ADJUSTMENT, OPENING_BALANCE, TRANSFER/MIGRATION_METADATA.

Avoid "Set balance" as ordinary operation. Administrative corrections should retain before/after and reason, preferably as auditable adjustment rather than destructive rewrite.

### Legacy import
A suspicious legacy date (e.g. epoch-era 1969 values observed in the sample) must:
- preserve original raw value;
- be flagged;
- allow human reconciliation;
- never be silently normalized into a fabricated date.

Bulk legacy spend text must be parsed as proposal/mapping, then reviewed.

## 22. Sheet Change UX

The sheet is not edited by directly mutating official holdings.

Player action:
1. Select section/item.
2. Choose proposed change.
3. System previews cost/rule implications where known.
4. Player adds required context (teacher/source/etc.).
5. Submit.
6. Rules Engine evaluates configured requirements.
7. ST/external governance workflows resolve.
8. Authorized approval commits official holdings + XP transaction atomically.

ST may have privileged proposal creation, but official mutation remains audited.

UI should show pending changes adjacent to official state without confusing them.

## 23. Provenance Standard

For every mechanically meaningful holding, answer when applicable:
- What is it?
- When did it become official?
- How was it acquired?
- What XP transaction paid for it?
- Which rule/source governed it?
- Who approved it?
- Was external approval/notification required?
- Who/what taught or granted it?
- What scene/action/source supports it?
- Was it imported?
- What was the original legacy representation?

Not every field requires every answer, but the model must not make these impossible.

## 24. Migration Mapping from Haller-like PDF/Export

### Direct/simple candidates
- name
- player
- chronicle
- status/lifecycle candidate
- start date
- clan
- sect
- nature
- demeanor
- path/rating
- generation
- virtue ratings
- willpower
- blood
- trait quantities
- ability ratings
- background ratings
- influence ratings

### Structured parsing + review
- Discipline notes/teachers/sources
- ability specializations
- Merit/Flaw values/descriptors
- Standing grantor/date
- equipment stats
- biography/notes
- XP bulk operations
- transfers
- legacy dates

### Never silently infer
- missing approvals;
- legality;
- teacher identity from ambiguous text;
- rule source;
- whether a historical entry was correct;
- true event date from malformed legacy date;
- visibility/secrecy of imported notes.

## 25. Proposed V1 Sheet Navigation

Character header remains compact and familiar.

Tabs:
1. **FICHA**
   - Identity summary
   - Attributes
   - Abilities
   - Backgrounds
   - Influences
   - Disciplines/Powers
   - Merits/Flaws
   - Morality/Virtues
   - Health/resources
   - Equipment
2. **HISTÓRICO**
   - official change history
   - transfers/lifecycle
3. **XP**
   - balance
   - ledger
   - pending spends
4. **AÇÕES**
   - Actions/Downtimes
5. **APROVAÇÕES**
   - governance requirements/evidence
6. **NOTAS**
   - authorized narrative/admin notes
7. **PUBLIC IDENTITY / WIKI** — post-core or disabled until publishing spec

Standing/Prestation may receive a dedicated political tab if its final scope justifies it.

## 26. What We Keep from Haller

- recognizable MET category organization;
- compact current-state summary;
- visible current/total XP concept;
- distinct positive/negative traits;
- familiar grouping of Abilities, Backgrounds, Influences, Disciplines, Merits/Flaws;
- historical XP ledger;
- printable/exportable character snapshot as a future/operational requirement.

## 27. What We Improve

Haller-like display → Platform model:
- "Power (Learned with X)" → Power Acquisition + Teacher + provenance.
- "Standing (By Prince X - date)" → Standing Record + grantor + office/domain + date.
- "Knife (+2...)" → Item Instance + mechanical profile.
- XP free-text spend → transaction linked to approved mechanical changes.
- Notes blobs → typed, permission-aware notes.
- static current sheet → official state + pending proposals + historical provenance.
- rule-neutral storage → explainable rule/governance evaluation.
- legacy import → staging/reconciliation instead of silent acceptance.

## 28. Rules Research Still Required

This PDF is a character instance, not an authoritative rules catalog.

Before implementing catalogs/cost engines, we still need approved sources for:
- base MET trait catalogs and creation rules;
- XP costs;
- discipline trees/progression;
- clan disciplines and out-of-clan behavior;
- ability/background/influence limits;
- merits/flaws;
- morality/virtue variants;
- health/resource derivation;
- equipment mechanics;
- OWBN controlled items;
- Status/Prestation;
- blood magic/rituals/rites;
- relevant House Rules for pilot.

All catalog/rule data must carry source/version/provenance.

## 29. Acceptance Criteria for Sheet Foundation

The sheet foundation is ready when:
1. a Vampire PC can be represented without using a single opaque JSON/text blob as the only source of truth;
2. repeated traits/ratings are structured;
3. powers are distinct from acquisition provenance;
4. pending proposals cannot masquerade as official holdings;
5. XP spend can link to the mechanical change it purchased;
6. regulated items can link to Rules/Approval workflows;
7. sensitive fields obey authorization;
8. imported ambiguous data can remain unresolved without being lost;
9. historical state is auditable;
10. adding Narrative Engine later does not require replacing Character identity or provenance foundations.

## 30. Open Decisions

- live Blood/Willpower/Health tracking in V1 vs permanent maxima only;
- exact launch scope of Status/Prestation;
- full NPC sheet parity at launch;
- equipment/item-card depth;
- exact treatment of Player Notes;
- whether political positions are displayed within Ficha or a dedicated domain/politics view;
- initial rules/source corpus and licensing/data-entry strategy.
