# Skill: Planning

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Estruturar objetivos, definir escopo, identificar stakeholders e restrições, e estabelecer critérios de sucesso para uma iniciativa de software, produzindo um documento de visão que orienta todas as fases seguintes do SDLC.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `nome_iniciativa` | string | Obrigatório | Nome da iniciativa ou projeto |
| `contexto_negocio` | string | Obrigatório | Descrição do problema ou oportunidade que motivou a iniciativa |
| `stakeholders` | lista | Obrigatório | Nomes e papéis das partes interessadas identificadas |
| `restricoes` | lista | Opcional | Restrições conhecidas: prazo, orçamento, tecnologia, regulatório |
| `premissas` | lista | Opcional | Premissas assumidas como verdadeiras para o planejamento |
| `metricas_sucesso` | lista | Opcional | Indicadores mensuráveis que definem o sucesso da iniciativa |

## Output

Documento de visão em Markdown contendo:

- Sumário executivo da iniciativa
- Objetivo principal e objetivos secundários
- Escopo delimitado (o que está dentro e o que está fora)
- Mapa de stakeholders com papéis e nível de influência
- Restrições e premissas catalogadas
- Critérios de sucesso mensuráveis
- Riscos iniciais identificados
- Próximos passos recomendados

## Prompt base

```
You are a senior product and engineering strategist. Your task is to produce a clear and structured Vision Document in Markdown for a software initiative.

You will receive:
- Initiative name
- Business context (problem or opportunity)
- Stakeholder list
- Known constraints (optional)
- Assumptions (optional)
- Success metrics (optional)

Produce the Vision Document with the following sections:
1. Executive Summary (2-3 sentences, non-technical language)
2. Objectives (one primary, up to three secondary)
3. Scope (explicitly state what is IN scope and what is OUT of scope)
4. Stakeholder Map (name, role, influence level: High/Medium/Low)
5. Constraints and Assumptions
6. Success Criteria (measurable, time-bound where possible)
7. Initial Risks (list up to 5, with likelihood and impact)
8. Recommended Next Steps

Rules:
- Be precise and avoid vague language.
- Every success criterion must be measurable.
- Scope boundaries must be explicit - ambiguity here propagates to all downstream phases.
- Write in the language of the input (if input is in Portuguese, output in Portuguese).
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O documento contém todas as 8 seções obrigatórias
- [ ] Pelo menos um critério de sucesso é mensurável e possui indicador quantitativo
- [ ] O escopo delimita explicitamente o que está fora (out of scope)
- [ ] Cada stakeholder tem papel e nível de influência definidos
- [ ] Pelo menos um risco inicial foi identificado
- [ ] O documento não contém afirmações vagas sem dado concreto de suporte

## Boas práticas verificadas

- [ ] **Governança:** critérios de sucesso definidos antes de qualquer execução, aprovação de escopo documentada
- [ ] **DDD:** linguagem do domínio de negócio utilizada na descrição de objetivos e escopo (não linguagem técnica)
- [ ] **Documentação Markdown:** estrutura em seções com cabeçalhos, tabelas e listas; sem formatação excessiva
