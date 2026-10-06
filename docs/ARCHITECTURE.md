# Conceptual Architecture

## 1. Character Engine
Character é a raiz. Mechanical Sheet, XP, powers, backgrounds, inventory e status são domínios relacionados.

## 2. Narrative Engine
Entidades conceituais:
- Scene
- Event / Event Truth
- Knowledge Claim
- Rumor
- Relationship
- Promise / Obligation
- Narrative Thread / Plot
- Timeline Entry

Scene Intelligence produz propostas; não fatos automaticamente.

## 3. World Graph
Nós potenciais:
Character, Chronicle, Domain, Organization, Society, Location, Event, Plot, Item.

Edges devem ser tipados, possuir provenance e, quando necessário, visibility e temporalidade.

## 4. Rules & Governance Engine
RuleSource → RuleVersion → Requirement/Restriction → Authority → Workflow.

Fontes previstas:
- material/ruleset publicado;
- OWBN Charter/Bylaws;
- binding packets quando aplicáveis;
- Chronicle House Rules.

Precedência não deve ser presumida genericamente quando a norma aplicável exigir interpretação específica.

## 5. Approval Engine
Controlled Item → Rule Evaluation → Required Action → Recipient/Authority → Request → Evidence → Decision → Sheet linkage.

## 6. Directory
Office, Coordinator/role, official contact, team contact, related lists, validity/verification. Directory é independente de Rule.

## 7. Public Identity
Camada deliberadamente publicável do Character. Exportadores (Wiki etc.) consomem apenas dados autorizados dessa camada.

## 8. Permission Model
Autorização deve ocorrer antes da recuperação/entrega do dado. Visibility inicial:
PUBLIC / PLAYER / CHRONICLE_ST / NETWORK_ST / COORDINATOR / RESTRICTED.

## 9. Audit & Provenance
Entidades sensíveis devem registrar criação, alteração, fonte, ator e timestamps. Decisões de approval e mudanças mecânicas devem ser historicamente rastreáveis.

## 10. Design constraint
Projetar para evolução sem implementar prematuramente toda a visão. MVP prova o ciclo Character + Rules + Approval + Scene + Memory.
