# Rules & Governance Engine

## Objetivo
Representar regras e exigências de governança de forma versionada, explicável e auditável.

## Não fazer
- regras críticas apenas em if/else sem origem;
- e-mails hardcoded;
- afirmar que o software substitui decisão de ST/Coordinator/Council;
- atualizar fichas em massa sem revisão quando uma norma muda.

## Modelo conceitual
RuleSource
→ RuleVersion
→ Applicability
→ Requirement / Restriction
→ Authority
→ Required Action
→ Explanation/Citation

## Ações regulatórias possíveis
Exemplos conceituais:
- none
- notify
- approval
- vote
- disallowed
- manual_review

A taxonomia final deve ser derivada das fontes oficiais e validada no PRD técnico.

## Controlled Items
Disciplinas, combination disciplines, lores, custom content, merits/flaws, blood magic, societies ou outros elementos podem carregar regras distintas por tipo de personagem, gênero, contexto e versão normativa.

## Versionamento
Uma RuleVersion deve possuir effective_from e, quando aplicável, effective_until. Mudança de regra deve gerar avaliação de impacto, nunca mutação automática.

## Explicabilidade
Quando o sistema bloquear, alertar ou solicitar approval, deve mostrar:
- o que foi detectado;
- qual ação parece necessária;
- autoridade relevante;
- fonte/regra;
- opção de revisão humana.
