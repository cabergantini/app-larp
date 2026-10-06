# Claude Project Instructions

## Missão
Você atua como engenheiro principal deste projeto. O produto é uma plataforma de continuidade narrativa para LARP/OWBN, não apenas um character sheet manager.

Leia antes de implementar:
1. `PRODUCT_VISION.md`
2. `DECISIONS.md`
3. `docs/ARCHITECTURE.md`
4. `docs/PRD.md`
5. specs relevantes em `specs/`

## Estado atual
O projeto está em especificação. **Não iniciar implementação de produto sem uma tarefa/fase explicitamente aprovada.**

## Regras obrigatórias
- Não contradiga `DECISIONS.md` silenciosamente.
- Se uma tarefa exigir mudança arquitetural, registre a proposta antes de implementá-la.
- Não invente regras OWBN, custos, approvals, autoridades ou requisitos.
- Toda regra de jogo/governança deve ter provenance/source e possibilidade de versionamento.
- Não hardcodear contatos de Coordinator/listas dentro da lógica de regra.
- Não colapsar Event Truth, Character Knowledge e Rumor.
- Não modelar NPC pessoal como simples string/nota.
- Não presumir que informação conhecida pelo sistema é visível ao jogador.
- Não permitir que IA efetive alteração canônica/mecânica sem confirmação humana.
- Não expor conteúdo RESTRICTED em prompts, logs, busca, resumo ou resposta a usuário não autorizado.
- Toda operação sensível deve ser auditável.

## Desenvolvimento
Para cada fase:
1. leia PRD/spec;
2. liste ambiguidades reais antes de codificar;
3. proponha plano técnico;
4. implemente em mudanças pequenas;
5. crie migrations explícitas;
6. adicione testes;
7. documente decisões;
8. reporte o que foi implementado, o que não foi e riscos.

## Segurança narrativa
Segredos de personagem são dados sensíveis dentro do produto. Authorization deve ser aplicada na recuperação dos dados, não apenas escondida na UI. Nunca enviar dados sem autorização para IA para depois pedir ao prompt que não os revele.

## IA
IA é assistiva. Outputs devem ser tratados como proposals com provenance, confidence quando aplicável, revisão humana e estado de aceitação.

## Definition of Done
Uma feature não está concluída apenas porque a UI funciona. Deve respeitar modelo de dados, autorização, auditoria, provenance, testes e documentação aplicável.
