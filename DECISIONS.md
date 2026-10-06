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


## D-015 — V1 é Vampire: The Masquerade / MET
O primeiro produto suporta exclusivamente Vampire: The Masquerade/MET. A arquitetura deve ser genre-extensible, mas não deve introduzir abstrações, requisitos ou complexidade de Garou, Mage, Changeling, Wraith ou outros gêneros no MVP.

## D-016 — Player propõe; alteração oficial passa por workflow
O jogador pode solicitar compras e alterações da própria ficha. A proposta não altera silenciosamente o estado oficial. Alterações passam por workflow de validação/aprovação do ST e, quando aplicável, Rules/Approval Engine para outras autoridades.

## D-017 — Actions/Downtimes fazem parte da V1
Actions/Downtimes são parte do produto inicial, não backlog pós-MVP. Devem ser modelados de forma compatível com futura integração ao Narrative Engine e Scene/Chronicle Memory.

## D-018 — Migração é requisito do MVP
Uma crônica existente deve conseguir migrar personagens sem reconstrução manual integral. O PRD deve definir estratégia e formatos suportados após investigação dos dados/exportações disponíveis, com validação humana do resultado importado.

## D-019 — Desenvolvimento começa com uma crônica piloto real
A primeira implantação será validada com uma crônica brasileira real. Nenhuma regra, nome, permissão ou fluxo exclusivo da crônica piloto deve ser hardcoded; configurações locais pertencem à camada Chronicle/House Rules/Settings.

## D-020 — Princípio Vampire-first, genre-extensible
O modelo deve evitar bloqueios óbvios à expansão futura para outros gêneros, mas YAGNI prevalece: não construir sistemas multi-genre antes de existir requisito aprovado.
