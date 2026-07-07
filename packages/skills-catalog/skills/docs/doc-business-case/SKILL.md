# Skill: Business Case

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar o documento que justifica a existência de um projeto antes de qualquer desenvolvimento, apresentando o problema específico com evidência, o custo estimado, o benefício quantificado, o ROI simplificado e as alternativas descartadas com motivo. Um business case sem alternativas descartadas ou sem critério mensurável de sucesso é uma decisão já tomada disfarçada de análise — e deve ser bloqueado.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `contexto_problema` | string | Obrigatório | Descrição específica do problema atual com evidência: frequência, impacto, quem é afetado e dado mensurável (tempo perdido, custo, taxa de erro, etc.) |
| `objetivo_projeto` | string | Obrigatório | O que o projeto entrega e qual resultado mensurável ele produz. Deve ser expresso como mudança observável no negócio, não como lista de features |
| `alternativas` | lista de strings | Obrigatório | Pelo menos duas abordagens alternativas que foram consideradas antes de propor este projeto (incluindo "não fazer nada" como alternativa sempre válida) |
| `restricoes` | lista de strings | Opcional | Restrições de prazo, orçamento, tecnologia ou equipe que limitam as opções disponíveis |
| `stakeholders` | lista de strings | Opcional | Quem é afetado pelo problema, quem aprova o projeto e quem executa — com seus interesses e nível de influência |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/business-case.md`
- **Seções obrigatórias:**
  1. Sumário executivo (3–5 linhas: problema, solução proposta, investimento total, benefício esperado, critério de sucesso)
  2. Problema identificado (específico, com dado mensurável e evidência)
  3. Oportunidade de negócio (o que muda se o problema for resolvido agora vs. não resolvido)
  4. Custo estimado (esforço de time, infraestrutura, custo de oportunidade)
  5. Benefício esperado (quantificado: tempo economizado, receita gerada, custo reduzido, risco mitigado)
  6. ROI simplificado (cálculo ou comparação qualitativa quando números não estão disponíveis — mas justificado)
  7. Alternativas descartadas (tabela: alternativa, razão do descarte)
  8. Critério mensurável de sucesso (o que será medido, como, em qual prazo)
  9. Riscos do projeto (os 3 principais com probabilidade e mitigação)

## Prompt base

```
You are a senior business analyst and product strategist responsible for writing a business case that will be reviewed by stakeholders before any project is approved.

You will receive:
- Problem context with evidence
- Project objective
- Alternatives considered (minimum 2, including "do nothing")
- Constraints (optional)
- Stakeholders (optional)

Produce a Business Case Document in Markdown with the following sections:

1. EXECUTIVE SUMMARY (3-5 sentences: problem in one line, proposed solution in one line, investment, expected benefit, and the one metric that will prove success)

2. PROBLEM IDENTIFIED
   - Description: specific, not generic. Reference the data provided.
   - Evidence: the measurable impact today (cost, time, error rate, affected users, lost revenue).
   - Context: who experiences this problem and when.
   - Anti-pattern to avoid: ❌ "the process is inefficient" → ✅ "the manual reconciliation process takes 4 hours per day across 3 analysts (12 person-hours/day), with an error rate of 8% that requires an additional 1.5 hours of correction per occurrence"

3. BUSINESS OPPORTUNITY
   - Why now: what changes if this problem is solved this quarter vs. next year.
   - Strategic alignment: how this connects to a business objective already recognized by stakeholders.

4. ESTIMATED COST
   - Team effort: roles, estimated hours/weeks, at what cost.
   - Infrastructure and tooling: recurring costs if applicable.
   - Opportunity cost: what the team is NOT doing while working on this.
   - Total: one number or range. Even if uncertain, give a range.

5. EXPECTED BENEFIT
   Quantify wherever possible. For each benefit:
   - Benefit description
   - Measurement method
   - Expected value (e.g., "saves 10 person-hours/week at R$80/h = R$800/week = R$41.600/year")
   If quantification is not possible: explain why and provide a qualitative description with a proxy metric.

6. SIMPLIFIED ROI
   - If numbers are available: (Total Benefit - Total Cost) / Total Cost × 100%
   - If quantification is limited: present a breakeven scenario ("at what point does the benefit exceed the cost?")
   - Include payback period when applicable.

7. DISCARDED ALTERNATIVES (Table)
   | Alternative | Description | Reason for Rejection |
   |---|---|---|
   | Do Nothing | Keep the current process | [impact of not solving + cost of inaction] |
   | [Alt 2] | [description] | [specific reason this was rejected over the proposed solution] |
   | [Alt N] | [description] | [specific reason] |
   
   Rule: "Do nothing" must always be listed. Every alternative must have a specific rejection reason — "insufficient" or "not ideal" are not valid reasons.

8. MEASURABLE SUCCESS CRITERION
   Define exactly what will be measured, how, and when:
   - Metric: [specific metric]
   - Baseline (current value): [current value with source]
   - Target (after project): [target value]
   - Measurement method: [how it will be measured]
   - Evaluation timeline: [when the measurement will happen, e.g., "90 days after go-live"]
   Minimum: one criterion. Maximum: three criteria (more than three makes accountability diffuse).

9. PROJECT RISKS
   List the top 3 risks (table: Risk | Probability: High/Medium/Low | Impact: High/Medium/Low | Mitigation)

Rules:
- Never produce a business case without at least one measurable success criterion. If the requester cannot define one, document that as a blocker and halt.
- Never approve an alternative table with only one alternative — a business case with a single alternative is a decision already made, not an analysis.
- The problem description must contain at least one number (cost, time, frequency, error rate, affected users). If none is provided, flag it and ask — or explicitly note it is estimated.
- Do not use vague benefit language: "it will save time" is not a benefit. "It will save 3 hours per transaction, with 50 transactions/month, at R$120/h = R$18.000/month" is a benefit.

Write in the language of the input. Numbers and financial figures should use the locale of the input language.
```

## Critério de sucesso

- [ ] O problema tem pelo menos um dado mensurável (custo, tempo, frequência, taxa de erro, usuários afetados)
- [ ] Há pelo menos duas alternativas descartadas, incluindo "não fazer nada", cada uma com motivo específico
- [ ] O critério de sucesso é mensurável: tem métrica, valor atual, valor alvo e prazo de avaliação
- [ ] O benefício esperado está quantificado ou, quando impossível, tem proxy métrico explícito
- [ ] Os riscos do projeto estão listados com probabilidade, impacto e mitigação

## Boas práticas verificadas

- [ ] **Governança:** o business case existe antes de qualquer desenvolvimento; a decisão de avançar está vinculada ao critério de sucesso definido aqui, não a preferências pessoais
- [ ] **DDD:** problema e oportunidade descritos na linguagem do negócio (não termos técnicos); alternativas avaliadas pelo impacto no domínio, não pela tecnologia
- [ ] **Observabilidade:** o critério de sucesso mapeia para uma métrica que o sistema ou processo já produz — ou define explicitamente como ela será instrumentada
- [ ] **Documentação Markdown:** sumário executivo legível em menos de 2 minutos por alguém fora do projeto; tabela de alternativas navegável; números em destaque (não enterrados em parágrafos)
- [ ] **Governança de decisão:** alternativas com custo-benefício inferior ao projeto proposto têm rejeição documentada — se uma alternativa for mais barata e o projeto for aprovado mesmo assim, o motivo deve estar explícito (ex: escalabilidade, risco regulatório, alinhamento estratégico)
