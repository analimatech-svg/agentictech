# Skill: Discovery

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Discovery |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Aprofundar o entendimento do problema, validar hipóteses de solução, definir o escopo técnico e fazer emergir a linguagem ubíqua do domínio, produzindo um relatório de discovery que fundamenta as decisões de design e arquitetura.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `documento_visao` | string (Markdown) | Obrigatório | Documento de visão produzido na fase Planning |
| `entrevistas` | lista de strings | Obrigatório | Transcrições ou resumos de entrevistas com usuários e stakeholders |
| `benchmarks` | lista de strings | Opcional | Referências de soluções existentes no mercado ou concorrentes |
| `restricoes_tecnicas` | lista | Opcional | Limitações de tecnologia, infraestrutura ou integração já conhecidas |
| `hipoteses_iniciais` | lista | Opcional | Hipóteses de solução levantadas na fase de Planning |

## Output

Relatório de discovery em Markdown contendo:

- Problema validado (reformulado com base nas entrevistas)
- Linguagem ubíqua do domínio (glossário de termos-chave)
- Hipóteses validadas e hipóteses invalidadas
- Escopo técnico delimitado
- Requisitos funcionais e não funcionais iniciais
- Fluxos de usuário de alto nível identificados
- Riscos e incertezas remanescentes
- Recomendações para a fase de Design

## Prompt base

```
You are a senior product manager and domain expert conducting a discovery phase for a software initiative.

You will receive:
- Vision Document from the Planning phase
- Interview transcripts or summaries with users and stakeholders
- Market benchmarks (optional)
- Known technical constraints (optional)
- Initial hypotheses (optional)

Produce a Discovery Report in Markdown with the following sections:
1. Validated Problem Statement (reformulated based on interview evidence)
2. Ubiquitous Language Glossary (key domain terms with precise definitions, 5-15 terms)
3. Hypothesis Validation (table: hypothesis | evidence | status: validated/invalidated/inconclusive)
4. Technical Scope (what will be built, what integrations are needed, what is excluded)
5. Initial Requirements (functional requirements as user needs, non-functional as quality attributes)
6. High-Level User Flows (numbered steps, no implementation details)
7. Remaining Risks and Uncertainties
8. Recommendations for the Design Phase

Rules:
- The glossary must use domain language, not technical jargon.
- Every hypothesis must reference specific evidence from the interviews.
- Technical scope must connect to the business objectives in the Vision Document.
- Write in the language of the input.
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O relatório contém todas as 8 seções obrigatórias
- [ ] O glossário tem entre 5 e 15 termos com definições precisas
- [ ] Cada hipótese referencia evidência concreta das entrevistas
- [ ] O escopo técnico está alinhado com os objetivos do documento de visão
- [ ] Pelo menos um fluxo de usuário de alto nível está descrito
- [ ] Os riscos remanescentes foram listados explicitamente

## Boas práticas verificadas

- [ ] **Governança:** hipóteses documentadas e validadas com evidência antes de avançar para design
- [ ] **DDD:** linguagem ubíqua definida explicitamente no glossário; termos do domínio usados nos fluxos e requisitos
- [ ] **Documentação Markdown:** estrutura clara com cabeçalhos, tabelas e listas; glossário em formato de tabela
