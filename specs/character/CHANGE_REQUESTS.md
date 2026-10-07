# Character Change Requests — V1

**Status:** Product specification draft
**Scope:** Vampire/MET official mechanical state
**Related:** specs/character/VAMPIRE_SHEET.md, specs/character/XP.md, docs/RULES_ENGINE.md

## 1. Purpose

Define how Players and authorized staff propose changes to official Character mechanical state.

A Change Request is the boundary between editable intent and canonical sheet state.

## 2. Principle

**Player proposes. Authorized humans decide. System validates/explains.**

No pending request is displayed as official sheet state.

## 3. Request Types

Initial generic operations:
- ADD
- INCREASE
- DECREASE
- REMOVE
- REPLACE
- SET/CHANGE where a domain requires a controlled transition
- CUSTOM_REVIEW

The UI presents domain-specific language ("Buy Ability", "Learn Power", etc.) rather than generic database verbs.

## 4. Request Structure

Change Request:
- id
- character_id
- requested_by
- chronicle_id
- status
- submitted_at
- reviewed_at
- reviewer(s)
- player justification/note
- source/action/scene reference nullable
- rules evaluation snapshot
- governance dependencies
- total estimated cost
- total accepted cost
- decision reason
- version/concurrency token
- audit metadata

Change Request Item:
- id
- request_id
- domain
- target catalog/holding
- operation
- before value
- proposed value
- quantity
- estimated XP cost
- accepted XP cost
- acquisition context
- teacher/grantor reference where applicable
- rule result
- governance requirement
- disposition

## 5. Status Model

Baseline:
DRAFT → SUBMITTED → UNDER_REVIEW → APPROVED | REJECTED

Additional:
- NEEDS_INFO
- WITHDRAWN
- SUPERSEDED
- BLOCKED_EXTERNAL (or equivalent UX state) when required governance remains unresolved

Transitions must be permission-checked and audited.

## 6. Player Flow

1. Open Character.
2. Choose Add/Change on relevant sheet section.
3. Select catalog item/value.
4. Enter acquisition context required for that domain.
5. System previews known prerequisites, XP estimate and governance requirements.
6. Add additional items if desired.
7. Review request summary.
8. Submit.

After submission, player can:
- see status;
- answer NEEDS_INFO;
- withdraw while policy permits;
- see decisions/reasons;
- see external requirement status they are authorized to see.

Player cannot edit the official holding directly.

## 7. ST Review Flow

Review page shows:
- current official state;
- proposed delta;
- XP available;
- estimated/accepted cost;
- prerequisites;
- applicable rule source/version;
- compliance result;
- teacher/source/context;
- linked Action/Scene if any;
- external governance requirements;
- previous relevant approvals/history where useful.

Reviewer can:
- approve;
- reject;
- request information;
- handle line items individually if partial review is supported;
- initiate/link governance workflow;
- add decision note.

## 8. Rule Evaluation Result

Each item may resolve to:
- CLEAR
- REVIEW
- BLOCKED
- UNKNOWN/MANUAL_REVIEW

Evaluation includes:
- rule id/version;
- explanation;
- required action;
- authority;
- cost result where applicable.

Unknown is not approval and not illegality.

## 9. External Governance Dependency

If a requested item requires Coordinator/Council/other external action:
- Change Request can be reviewed locally;
- final official commit must wait for required resolution where rules demand it;
- Governance Request is linked;
- evidence/decision is preserved;
- denial prevents that item from becoming official.

Do not infer external approval from a note.

## 10. Acquisition Context

Different domains can require different context.

Power example:
- teacher;
- learning source;
- date/effective date;
- Action/Scene;
- book/source;
- special justification.

Standing example:
- grantor;
- office/domain;
- date;
- reason.

Background example:
- linked NPC/organization where applicable;
- descriptor.

The core request model supports typed domain metadata rather than one universal free-text field.

## 11. Bundles

Players may group related purchases into one request.

Benefits:
- one narrative justification;
- one scene/action;
- one review session.

But each item remains independently evaluable.

If one item requires external approval while others do not, product may:
A. hold entire bundle; or
B. approve independent items and keep blocked item pending.

**Recommended V1:** support explicit per-item disposition so independent valid purchases need not be blocked by one controlled item, while making the final cost/result unambiguous.

## 12. Partial Approval

If reviewer changes requested quantity/value or approves only some items:
- system recalculates cost;
- player sees final accepted delta;
- audit records requested vs accepted;
- only accepted items commit.

If reviewer materially substitutes a different item, prefer return/needs-info or a revised proposal rather than silently changing player intent.

## 13. Official Commit

Before commit:
- reload current official character version;
- detect conflicting changes since submission;
- rerun relevant deterministic validations;
- verify XP;
- verify governance dependencies;
- confirm reviewer authority.

Commit:
- write accepted holdings/state;
- write XP transactions;
- write acquisition/provenance links;
- update request/item status;
- create audit events.

Failure in any required step rolls back the official mutation.

## 14. Conflicts

Example:
Player requests Ability 3→4. Before review another authorized change makes it 4.

System must not blindly apply stale delta.

Mark conflict/review and show:
- state at submission;
- current state;
- requested intent.

## 15. Staff-Originated Changes

ST may initiate a Change Request on behalf of a Character for:
- adjudication;
- correction;
- award/grant;
- migration reconciliation;
- rules-required change.

Staff-originated does not mean unaudited. Request can use an expedited workflow when role/policy permits, but provenance remains.

## 16. Creation vs Post-Creation

Initial character creation may use a separate Draft Sheet workflow because approving every starting trait individually is poor UX.

Once Character becomes Active/official, post-creation mechanical changes use Change Requests.

Exact character-creation workflow is a separate spec.

## 17. Visibility

Players see their own request details unless a staff-only rule/evidence field is explicitly restricted.

Governance correspondence may contain restricted material; authorization is evaluated per linked object, not inherited blindly into all player-visible request data.

## 18. Notifications

Events worth notifying:
- submitted;
- needs information;
- external requirement created/updated;
- approved;
- partially approved;
- rejected;
- conflict detected.

Channel implementation is technical-plan scope.

## 19. Acceptance Criteria

1. Player cannot directly mutate official post-creation mechanical state.
2. Draft/pending/official are visually and technically distinct.
3. Every accepted change identifies requester and decision authority.
4. Cost is recalculated before commit.
5. Required external approval can block finalization.
6. Partial approval never charges rejected items.
7. Stale requests cannot overwrite newer state silently.
8. Approved change and XP transaction are atomic.
9. Rule evaluation retains source/version.
10. Acquisition provenance can attach teacher/source/scene/action.
11. Staff changes remain auditable.
12. Unknown/ambiguous rules route to human review.
