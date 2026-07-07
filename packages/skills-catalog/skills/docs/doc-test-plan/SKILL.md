# Skill: Plano de Testes

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Validator |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar o plano de testes de uma funcionalidade ou release, definindo escopo, tipos de teste, ambientes, critérios de entrada e saída e responsáveis.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| requisitos | string (Markdown) | Sim | Requisitos funcionais e não funcionais da funcionalidade ou release |
| user_stories | string (Markdown) | Sim | User Stories com critérios de aceitação no formato Gherkin (Given/When/Then) |
| arquitetura | string (Markdown) | Sim | Descrição da arquitetura técnica: serviços, banco de dados, integrações externas |
| ambientes_disponiveis | lista de strings | Não | Ambientes disponíveis para teste (dev, staging, homologação, produção) |
| equipe | lista de strings | Não | Nomes e papéis dos membros da equipe de testes |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/test-plan.md`
- **Campos obrigatórios:** escopo do teste, tipos de teste com cobertura mínima, critério de entrada, critério de saída, ambientes, responsáveis por tipo de teste

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a test plan based on the provided requirements and user stories.

Include the following sections:
1. Scope: what is covered and what is explicitly out of scope
2. Test types: functional, regression, performance, security (each with minimum coverage defined)
3. Entry criteria: measurable conditions that must be met before testing begins
4. Exit criteria: measurable conditions that define when testing is complete
5. Required environments: configuration and data needed per environment
6. Responsible parties: who executes and who approves each test type

Language: Brazilian Portuguese

ANTI-PATTERNS — apply blocking rules:
- BLOCKED if: entry or exit criteria are not measurable (contain words like "suficiente", "adequado", "estável")
  ❌ "Critério de saída: testes concluídos e sistema estável."
  ✅ "Critério de saída: 95% dos casos de teste passando, zero bugs de severidade crítica em aberto, tempo de resposta p95 abaixo de 400ms no ambiente de homologação com carga de 100 usuários simultâneos."
- BLOCKED if: a test type has no defined minimum coverage
  ❌ "Testes de regressão serão executados conforme disponibilidade da equipe."
  ✅ "Testes de regressão: 100% dos casos do suite core (45 casos) + 80% dos casos das funcionalidades adjacentes modificadas (estimado: 12 casos)."
- BLOCKED if: any test type has no named responsible party
  ❌ "Testes de performance serão feitos pelo time."
  ✅ "Testes de performance: execução — Carla Mendes (QA Sênior); aprovação — Roberto Lima (Tech Lead)."
```

## Critério de sucesso

- [ ] O critério de entrada é mensurável e objetivo (ex: 100% dos casos de teste escritos, ambiente de homologação disponível e com dados atualizados)
- [ ] O critério de saída é mensurável e objetivo (ex: 95% dos casos passando, zero bugs críticos em aberto)
- [ ] Cada tipo de teste tem cobertura mínima expressa em porcentagem ou número absoluto de casos
- [ ] O escopo deixa explícito o que está fora dos testes desta release
- [ ] Cada tipo de teste tem pelo menos um responsável nomeado

## Boas práticas verificadas

- [ ] Testes de regressão cobrem as funcionalidades adjacentes que podem ter sido afetadas pelas mudanças
- [ ] Testes de performance têm baseline definido (tempo de resposta aceitável, volume de requisições)
- [ ] Testes de segurança mencionam pelo menos OWASP Top 10 para aplicações web
- [ ] O plano prevê o que acontece se o critério de entrada não for atingido na data prevista
