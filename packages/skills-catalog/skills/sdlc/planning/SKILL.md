# Skill: Planning

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Estruturar objetivos, definir escopo, identificar stakeholders e restrições, e estabelecer critérios de sucesso para uma iniciativa de software, produzindo um documento de visão que orienta todas as fases seguintes do SDLC. O Planning não é burocracia — é o momento em que os maiores erros do projeto são evitados: escopo mal definido, stakeholders esquecidos e critérios de sucesso que ninguém pode medir.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `nome_iniciativa` | string | Obrigatório | Nome da iniciativa ou projeto |
| `contexto_negocio` | string | Obrigatório | Descrição do problema ou oportunidade que motivou a iniciativa — deve ser escrito em linguagem de negócio, não técnica |
| `stakeholders` | lista de strings | Obrigatório | Nomes e papéis das partes interessadas identificadas |
| `restricoes` | lista de strings | Opcional | Restrições conhecidas: prazo, orçamento, tecnologia, regulatório |
| `premissas` | lista de strings | Opcional | Premissas assumidas como verdadeiras para o planejamento |
| `metricas_sucesso` | lista de strings | Opcional | Indicadores mensuráveis que definem o sucesso da iniciativa |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/vision.md`
- **Seções obrigatórias:**
  1. Sumário executivo (2–3 frases, linguagem não técnica)
  2. Objetivos (um primário, até três secundários)
  3. Escopo — IN e OUT (explícito)
  4. Mapa de stakeholders (nome, papel, nível de influência: Alto / Médio / Baixo)
  5. Restrições e premissas
  6. Critérios de sucesso (mensuráveis, com prazo quando possível)
  7. Riscos iniciais (até 5, com probabilidade e impacto)
  8. Próximos passos recomendados

## Prompt base

```
You are a senior product and engineering strategist. Your task is to produce a clear and structured Vision Document in Markdown for a software initiative. This document is the source of truth for scope, success criteria, and stakeholder alignment — every downstream phase depends on its precision.

You will receive:
- Initiative name
- Business context (problem or opportunity, in business language)
- Stakeholder list
- Known constraints (optional)
- Assumptions (optional)
- Success metrics (optional)

Produce the Vision Document with the following sections:

1. EXECUTIVE SUMMARY
   2–3 sentences in non-technical language. Answers: what problem is being solved, for whom, and what the expected outcome is.

2. OBJECTIVES
   One primary objective + up to three secondary objectives.
   Format each as: "[verb] [measurable outcome] by [timeframe or condition]"
   Anti-pattern ❌: "Improve the customer experience."
   Correct ✅: "Reduce support ticket volume for onboarding-related issues by 40% within 3 months of launch."

3. SCOPE
   Two subsections — both are mandatory:
   - IN SCOPE: what this initiative will deliver
   - OUT OF SCOPE: what is explicitly excluded (minimum 2 items)
   Anti-pattern ❌: Out of scope left blank or with "TBD."
   Correct ✅: "OUT OF SCOPE: (1) Mobile app version — separate initiative planned for Q3. (2) Integration with legacy billing system — blocked by ongoing vendor migration."

4. STAKEHOLDER MAP
   Table format:
   | Name | Role | Influence | Key concern |
   |---|---|---|---|
   | [Name] | [Role in project] | High / Medium / Low | [What they care about most] |
   - High influence = their approval is required to proceed
   - Medium = should be informed and consulted
   - Low = receives outcomes but does not shape decisions

5. CONSTRAINTS AND ASSUMPTIONS
   Two subsections:
   - Constraints: things that are fixed and cannot be changed (budget cap, regulatory deadline, mandated technology)
   - Assumptions: things believed to be true but not yet confirmed (team availability, market condition, dependency timeline)
   Flag each assumption with its validation owner and target date.

6. SUCCESS CRITERIA
   Table format:
   | Criterion | Metric | Baseline | Target | Measurement method | Owner |
   |---|---|---|---|---|---|
   Rules:
   - Every criterion must have a numeric target. "Better", "faster", "more reliable" are NOT criteria.
   - Anti-pattern ❌: "Users will be satisfied with the new experience." → no metric, no baseline.
   - Correct ✅: "NPS for the onboarding flow increases from 32 to ≥55, measured by in-app survey 30 days post-launch."

7. INITIAL RISKS
   Table format:
   | Risk | Probability (H/M/L) | Impact (H/M/L) | Mitigation |
   |---|---|---|---|
   - List up to 5 risks
   - Anti-pattern ❌: "Risk: technical complexity." → too vague to act on.
   - Correct ✅: "Risk: third-party payment API rate limits exceed our transaction volume during peak periods. Probability: Medium. Impact: High. Mitigation: request enterprise tier contract before development starts."

8. RECOMMENDED NEXT STEPS
   Numbered list of the next 3–5 concrete actions, with owner and timeframe.

BLOCKING RULES (apply before returning the document):
- BLOCKED if: any success criterion has no numeric target or measurement method.
- BLOCKED if: the OUT OF SCOPE section is absent or has fewer than 1 item.
- BLOCKED if: any stakeholder is listed without influence level and key concern.
- BLOCKED if: context_negocio contains technical implementation details instead of business problem description ("we need to refactor the auth module" is not a business problem — the business problem is "consultants cannot access the system without IT intervention, causing 48h average onboarding delay").
- BLOCKED if: the document has fewer than 8 sections.

Write in the language of the input.
```

## Critério de sucesso

- [ ] O documento contém todas as 8 seções obrigatórias
- [ ] Cada critério de sucesso tem indicador numérico, baseline e método de medição definidos
- [ ] O escopo delimita explicitamente o que está fora (mínimo 1 item com justificativa)
- [ ] Cada stakeholder tem nível de influência e preocupação principal documentados
- [ ] O contexto de negócio descreve o problema em linguagem de domínio — não implementação técnica
- [ ] Cada premissa tem owner de validação identificado

## Boas práticas verificadas

- [ ] **Governança:** critérios de sucesso definidos antes de qualquer execução; premissas com owner explícito evitam que suposições se tornem verdades tácitas
- [ ] **DDD:** linguagem do domínio de negócio usada nos objetivos e critérios — sem vocabulário de implementação (não "implementar microserviço", mas "permitir que consultores se cadastrem sem intervenção do time de TI")
- [ ] **Clean Architecture:** o escopo IN/OUT delimitado no Planning previne decisões de implementação prematuras que criam acoplamento antes do design estar definido
- [ ] **Observabilidade:** critérios de sucesso com método de medição explícito garantem que o sistema produzirá os dados necessários para validar o resultado — se o critério não pode ser medido automaticamente, a observabilidade do sistema é insuficiente
- [ ] **Documentação Markdown:** estrutura em seções com cabeçalhos, tabelas e listas; linguagem executiva no sumário, linguagem técnica apenas nas seções de risco e próximos passos
