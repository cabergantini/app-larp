# Decision Log

Registro das decisões estruturais já tomadas. Alterações relevantes devem ser documentadas aqui antes ou junto da implementação.

## D-001 — Character, não Sheet, é a entidade central
A ficha mecânica é uma representação de Character. Character também possui memória narrativa, conhecimento, relações, identidade pública e governança.

## D-002 — NPC é entidade de primeira classe
NPCs não são texto dentro de Backgrounds. Chronicle NPCs, Coordinator NPCs, published/canon NPCs e personal NPCs podem possuir ficha, timeline, relações, conhecimento, controle e permissões próprios.

## D-003 — Event Truth ≠ Character Knowledge ≠ Rumor
O sistema deve armazenar separadamente o que aconteceu, quem sabe o quê e o que circula como rumor. Rumor pode ser verdadeiro, falso, parcial ou desconhecido.

## D-004 — IA sugere; humanos decidem
IA nunca altera automaticamente ficha, XP, cânone, relações, conhecimento, rumores, approvals ou consequências narrativas.

## D-005 — Proveniência é obrigatória
Alterações e conhecimentos relevantes devem poder apontar para fonte: cena, regra, aprovação, usuário, evento ou documento.

## D-006 — Visibilidade é granular
Permissões devem poder existir no nível do registro/campo, não apenas no nível da ficha. Camadas iniciais: PUBLIC, PLAYER, CHRONICLE_ST, NETWORK_ST, COORDINATOR, RESTRICTED.

## D-007 — Regras são dados versionados
Não hardcodear regras OWBN como lógica irreversível. Rules Engine deve armazenar fonte, versão, vigência, autoridade, requisito e precedência.

## D-008 — Contatos oficiais são dinâmicos
E-mails de Coordinator/listas não devem ser hardcoded nas regras. Usar Directory separado e atualizável.

## D-009 — Approval é workflow rastreável
Controlled items podem gerar DRAFT → READY → SENT → WAITING → MORE_INFO → APPROVED/DENIED, preservando evidência e histórico.

## D-010 — Sovereignty by design
Cross-chronicle discovery não concede autoridade sobre outra crônica, NPC ou território. Descoberta e solicitação são separadas de autorização.

## D-011 — Public Identity é independente da ficha privada
Dados públicos podem ser exportados para Camarilla Wiki e outros destinos sem expor dados privados.

## D-012 — Scene logs são fontes, não cânone automático
Logs podem gerar extrações e sugestões. O registro canônico depende de confirmação/autorização apropriada.

## D-013 — Arquitetura deve suportar mais que o MVP
O banco e os domínios devem evitar atalhos que impossibilitem Narrative Graph, NPC Network, rules versioning e cross-chronicle no futuro; isso não significa implementar tudo na V1.

## D-014 — GitHub é a fonte de verdade do projeto
Decisões, PRD, specs e código aprovados vivem neste repositório. Conversas com agentes não substituem documentação versionada.
