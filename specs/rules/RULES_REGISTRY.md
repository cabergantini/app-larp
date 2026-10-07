# Rules Registry & Evaluation — V1

**Status:** Product specification draft  
**Scope:** Vampire/MET rules metadata and deterministic/advisory evaluation  
**Authoritative content:** must be populated only from approved sources with provenance.

## 1. Purpose
Represent rules as versioned, attributable data so the platform can explain costs, prerequisites, restrictions and governance requirements without turning software into an unsourced rules authority.

## 2. Core Principles
- Rule text/source and system interpretation are distinct records.
- Rules are effective-dated and versioned.
- Historical evaluations retain the version used at decision time.
- Ambiguity resolves to MANUAL_REVIEW.
- No rule may identify a current human office-holder as permanent logic.
- Chronicle rules cannot silently relax binding OWBN restrictions.
- The engine evaluates configured rules; it does not invent missing rules.

## 3. Rule Source
Fields:
- id
- source_type: BASE_RULEBOOK | OWBN_CHARTER | OWBN_CHARACTER_BYLAW | OWBN_MECHANICS_BYLAW | OWBN_PACKET | CHRONICLE_HOUSE_RULE | OTHER_APPROVED
- title
- publisher/authority
- canonical reference/url/document id
- edition/version
- published/effective_from/effective_to
- last_verified_at
- status
- provenance

Source availability/licensing must be respected. A citation/reference may be stored without reproducing copyrighted text.

## 4. Rule
Fields:
- id
- source_id
- stable_key
- section/reference
- short_label
- human-readable normalized interpretation
- rule_type
- scope
- effective_from/to
- supersedes/superseded_by
- priority/authority layer
- machine_evaluable boolean
- review_required boolean
- provenance

Rule types include:
COST, PREREQUISITE, LIMIT, PROHIBITION, GOVERNANCE_REQUIREMENT, NOTIFICATION, APPROVAL, VOTE, CREATION, ACQUISITION, HOUSE_RESTRICTION, OTHER.

## 5. Conditions
Rules may evaluate typed facts:
- PC/NPC
- clan/bloodline
- sect
- generation
- item/power/category
- level
- in-clan/out-of-clan classification
- existing holdings
- chronicle
- acquisition context
- effective date
- other explicitly modeled facts.

Conditions must be declarative where possible, not scattered hardcoded application logic.

## 6. Effects
A matched rule can produce:
- calculated/modified cost
- prerequisite
- maximum/minimum
- warning
- prohibition
- manual review
- governance requirement
- notification
- approval
- vote threshold
- additional evidence/context requirement.

## 7. Evaluation
Input:
- character official snapshot/version
- proposed change
- effective date
- chronicle
- applicable ruleset/source versions
- relevant governance facts

Output:
- CLEAR | REVIEW | BLOCKED | MANUAL_REVIEW
- matched rules
- explanations
- cost breakdown
- prerequisites
- governance requirements
- conflicts/unknowns
- evaluation timestamp/version/hash

## 8. Precedence and Conflicts
The engine must not assume a simplistic "last rule wins."

Represent authority layers and explicit overrides/relationships. If two applicable rules cannot be deterministically reconciled from configured policy, return MANUAL_REVIEW with both sources.

Chronicle House Rules may impose stricter local constraints where allowed but may not be configured to silently permit something prohibited/restricted by binding OWBN rules.

## 9. Rule Versioning
Never overwrite historical meaning in place.

A change creates a new effective version or superseding rule. Existing official character holdings are not automatically mutated.

Future compliance-impact workflow can identify potentially affected characters, but human review determines grandfathering/action according to the actual rule change.

## 10. Cost Engine
Cost is a rules output, not a static property of every catalog item.

Support:
- base cost
- context modifiers
- progression/rank
- zero-cost acquisition
- manual cost
- House Rule stricter/different cost only when valid under applicable governance/rules.

Every calculated cost shown in a Change Request should identify its source/version.

## 11. Explainability
User-facing evaluation should answer:
- What did the system detect?
- Which rule/source applies?
- Why does it apply to this Character/request?
- What consequence follows?
- What must happen next?
- Is this deterministic or awaiting human interpretation?

## 12. Compliance Language
Never label a sheet/character "illegal."

Statuses:
- CLEAR
- REVIEW
- BLOCKED
- MANUAL_REVIEW

"BLOCKED" means the requested system action cannot finalize under configured requirements; it is not a moral/organizational accusation.

## 13. House Rules
House Rules are first-class Rule Sources with versions/effective dates.

Publishing a new House Rule version:
- preserves prior version
- requires authorized chronicle actor
- records effective date
- triggers validation for contradictory relaxation against configured binding rules
- may create review tasks for affected future/current state as later specified.

## 14. Rules Data Administration
V1 requires a controlled admin/editor workflow:
DRAFT → REVIEWED → PUBLISHED → SUPERSEDED/RETIRED.

No arbitrary player-authored rule becomes machine-enforced.

Rule publication audit records editor/reviewer/source.

## 15. Acceptance Criteria
1. Every machine-enforced rule has a source/version.
2. Historical evaluation remains reproducible.
3. Ambiguity can route to MANUAL_REVIEW.
4. Rules can vary by PC/NPC and context.
5. Costs can be context-dependent.
6. House Rules are versioned.
7. Current office-holder contact is not hardcoded into rule.
8. Rule update does not silently rewrite character history.
9. Evaluation is explainable.
10. Controlled Item requirements can be emitted as structured governance dependencies.
