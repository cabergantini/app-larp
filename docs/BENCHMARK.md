# Benchmark Funcional — HallerGames × OWBN × OWBN Narrative Platform

Data da pesquisa: 2026-10-06

## Objetivo
Mapear capacidades reais do HallerGames, obrigações/restrições relevantes da OWBN e oportunidades de diferenciação. Este documento é insumo para escopo MVP e PRD; não é especificação de implementação.

## Fontes principais
- HallerGames — site público, New Game e User Manual.
- OWBN — Charter, Character Bylaws, Mechanics Bylaws, Administrative Bylaws e Coordinator Bylaws vigentes consultados na data acima.

## Correção de hipótese
HallerGames não é apenas um gerenciador de ficha. Sua documentação descreve gestão online de characters, plots, rumors, actions, visitors, locations, XP e ferramentas de ST. Nosso diferencial não pode ser simplesmente "adicionar narrativa".

## Capacidades observadas no Haller

| Área | Haller observado | Implicação |
|---|---|---|
| Contas e roles | User, Player, ST e roles especializados como Harpy/Galliard | Precisamos RBAC/ABAC mais granular, sem perder simplicidade |
| Crônicas | cadastro, game info, genre, datas e acesso | Paridade MVP |
| Character sheets | múltiplos gêneros MET/BNS; criação e visualização | Paridade MVP, inicialmente com escopo de gênero deliberado |
| Edição | player cria/submete; após status definido, ST mantém sheet oficial | Precisamos workflow auditável de proposals, não edição irrestrita |
| Validação | manual afirma que não faz validation checks | Grande oportunidade: Rules Engine explicável |
| XP | log, spends, requests, bulk XP, monthly cap; custos de mudança apenas básicos | Precisamos ledger + cálculo contextual + regras/versionamento |
| Notes | biography, notes, player notes, ST notes | Paridade, porém com visibility/provenance granular |
| Actions/Downtime | player/visitor submete; ST responde | Paridade MVP desejável |
| Plots/Events | plot contém events; actions e rumors podem se relacionar | Não reinventar; evoluir para Narrative Graph |
| Rumors | por game date; filtros por traits; distribuição a personagens | Evoluir separando Rumor de Event Truth e Character Knowledge |
| Locations | locations associadas a characters | Evoluir para World Graph |
| Visitors | envio de sheet a outra game, prazo/ongoing, read-only, actions/sign-in | Forte baseline para Travel Brief/Return Report |
| Boons/Status | Harpy pode gerenciar status/negative status/boons | Modelar relações/obrigações como entidades rastreáveis |
| Sign-in | presença, visitors e notificação à home game | Paridade operacional útil |
| Search | busca de traits | Evoluir para graph/search narrativo com autorização |
| Transfer | transferência de Character entre games + email | Deve obedecer workflow OWBN e approvals envolvidos |
| Import/export | Grapevine XML, backup XML, print/PDF | Migração é requisito comercial importante |
| Reports | backgrounds/influences, status/boons, plots | Evoluir para Chronicle Memory/compliance/reporting |
| Event games | cópia/import de sheets sem afetar original; attendance notifications | Considerar após núcleo MVP |

## Obrigações/necessidades OWBN relevantes

### Chronicle sovereignty
A arquitetura deve preservar autoridade da crônica dentro de seus limites e impedir que cross-chronicle discovery equivalha a permissão de uso.

### Home Chronicle e lifecycle
PC possui uma Home Chronicle. Character deve distinguir PC/NPC e lifecycle/status regulamentar. Transferência não é mera alteração de foreign key; pode exigir concordância e histórico.

### Controlled Items
Character Bylaws possuem níveis como Disallowed, Council vote, Coordinator Approval e Coordinator Notify, com diferenças possíveis entre PC e NPC. O produto precisa representar requirement, authority, source e version.

### Prazos e evidência
Approval de Coordinator possui processo e prazo regulamentar. Approval Engine precisa registrar envio, respostas, pedidos de informação, bumps/extensões quando aplicáveis e evidência.

### Rules precedence
Mechanics Bylaws funcionam como limites vinculantes; chronicles podem ser mais restritivas, não mais permissivas. House Rules oficiais também precisam ser versionadas e associadas à crônica.

### Digital item accountability
OWBN atribui responsabilidade/aprovação a item cards digitais. Provenance e audit trail devem ser parte do modelo, não feature opcional.

### Reporting
Crônicas possuem reporting periódico e precisam registrar grandes plots/eventos com potencial de impacto externo. Chronicle Memory pode reduzir drasticamente o trabalho manual desses relatórios.

### Shared-world continuity
A própria estrutura OWBN exige cooperação entre crônicas e continuidade global. Esse é o fundamento do World Graph e do Storyteller Exchange.

## Matriz competitiva

| Capacidade | Haller | Necessidade OWBN | Nossa direção | Prioridade |
|---|---|---|---|---|
| Character/PC/NPC | Sim | Essencial | Character Engine | P0 |
| Sheet | Sim | Essencial | Sheet versionada/auditável | P0 |
| XP | Sim | Essencial | Ledger + contextual rules | P0 |
| Chronicle/users/roles | Sim | Essencial | RBAC + atributos | P0 |
| House Rules | links/texto externo em games observadas | Oficial e vinculante localmente | Rules Engine versionado | P0 |
| Rules validation | Não | Alto valor/compliance | Explainable Rules Engine | P0 |
| Controlled Items | não identificado como engine normativa | Obrigatório em vários casos | Regulation metadata + evaluation | P0 |
| Approval workflow | não identificado como workflow integrado | Essencial | Approval Engine | P0 |
| OWBN Directory | não identificado | necessário para routing | Directory dinâmico | P0/P1 |
| Actions/Downtimes | Sim | Operação comum | Estruturado + provenance | P0/P1 |
| Visitors | Sim | Shared network | Travel workflow | P1 |
| Scene logs | não identificado como ingestão/análise | continuidade | Scene Intelligence | P1 |
| Character Knowledge | Não identificado | forte valor narrativo | Knowledge graph | P1 |
| Event Truth vs Rumor | Rumor existe, separação semântica não identificada | continuidade | modelo tripartido | P1 |
| NPC first-class narrative entity | NPC sheet existe | alto valor | NPC Engine + control | P1 |
| Plots/Events | Sim | alto valor | Narrative Graph | P1 |
| Chronicle Memory | relatórios manuais | reporting/continuidade | summaries + traceability | P1 |
| Cross-chronicle discovery | visitors existem | missão shared-world | Storyteller Exchange | P2 |
| Narrative matching | Não identificado | diferencial | Looking for Plot | P2 |
| Wiki publishing | Não identificado | conveniência/public identity | Publishing Engine | P2 |
| Coordinator dashboard | Não identificado | potencial institucional | Governance workspace | P2 |
| Legacy import | Grapevine XML | adoção/migração | Import Haller/Grapevine quando viável | P0/P1 |

## Gap principal
Haller registra objetos narrativos, mas sua documentação mostra um modelo predominantemente administrativo: sheet, action, plot, rumor, visitor, report. A oportunidade é criar semântica e conexão entre esses objetos.

Exemplo:
Haller: Rumor R foi entregue a personagens A/B/C.
Nossa plataforma: Rumor R tem provenance; surgiu de Event E; A ouviu de B; C recebeu versão distorcida; ST sabe truth state; cada personagem possui knowledge state independente.

## Diferenciais defensáveis

1. **Rules-aware character management** — não apenas armazenar trait, mas explicar permissibilidade, custo, requisito e autoridade com fonte/versionamento.
2. **Narrative memory** — cenas viram propostas estruturadas de fatos, conhecimento, relações, promessas e hooks.
3. **Knowledge-aware world** — o sistema diferencia o que aconteceu do que cada personagem sabe.
4. **Cross-chronicle graph with sovereignty** — descoberta sem vazamento de segredo nem perda de autoridade.
5. **Governance provenance** — approvals, rules, item accountability e decisões ficam rastreáveis.
6. **AI constrained by permissions** — IA só recebe dados autorizados e produz proposals revisáveis.

## Recomendação de MVP

### P0 — precisa existir para substituir o núcleo administrativo
- Auth/users
- Chronicle + membership/roles
- Character (PC/NPC) + lifecycle + Home Chronicle
- Vampire/MET sheet inicial (escopo final a confirmar)
- XP ledger + requests/spends
- notes com visibility
- Rules Registry/Rule Version
- House Rules
- Controlled Item evaluation
- Approval/Notification workflow
- audit/provenance
- import strategy para reduzir custo de migração

### P1 — cria o primeiro diferencial claro
- Actions/Downtimes
- Scene record/upload
- Scene Intelligence proposals
- Event Truth
- Character Knowledge
- Rumor
- Relationships
- NPC first-class model
- Visitor/Travel Brief
- Chronicle Memory

### P2 — efeito de rede
- World Graph discovery
- Storyteller Exchange
- narrative hook matching
- Narrative Agreements
- Wiki/Public Identity publishing
- Coordinator workspace
- advanced cross-chronicle reporting

## Decisão recomendada
Não tentar vencer Haller por quantidade de checkboxes na primeira versão. Construir paridade suficiente para uma crônica operar, mas investir cedo nos dois pontos que Haller explicitamente não resolve como engine: **Rules/Governance** e **Narrative Memory**.

## Perguntas para fechamento do MVP
1. O primeiro gênero suportado será somente Vampire MET Revised/3rd Edition ou já precisamos multi-genre?
2. Player poderá propor alterações à própria ficha após aprovação inicial, com ST aprovando/rejeitando, ou toda edição oficial continuará ST-only?
3. Actions/Downtimes entram no lançamento inicial ou imediatamente após o Character/Rules/Approval núcleo?
4. Migração do Haller/Grapevine é requisito de lançamento?
5. O primeiro mercado é uma crônica piloto brasileira específica ou qualquer crônica OWBN?
