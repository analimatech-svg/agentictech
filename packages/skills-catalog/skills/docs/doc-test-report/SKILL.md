# Skill: Relatório de Testes

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Validator |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar o relatório consolidado de execução dos testes, com métricas de cobertura, bugs encontrados e recomendação explícita de go ou no-go para o deploy.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| casos_executados | lista de objetos | Sim | Lista de casos de teste com status de execução (Passou, Falhou, Bloqueado) |
| bugs_registrados | lista de objetos | Sim | Lista de bugs encontrados com severidade (crítica, alta, média, baixa) |
| plano_testes | string (Markdown) | Sim | Plano de testes com critérios de saída definidos |
| data_execucao | string | Sim | Período de execução dos testes (data de início e data de fim) |
| ambiente | string | Não | Ambiente em que os testes foram executados (staging, homologação, etc.) |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/test-report.md`
- **Campos obrigatórios:** total de casos, passaram, falharam, bloqueados, cobertura em número e porcentagem, bugs bloqueantes separados dos não bloqueantes, recomendação go ou no-go com justificativa

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a test execution report based on the provided test cases and bugs.

Include the following sections:
1. Executive summary: total cases, pass rate, coverage percentage
2. Results by type: functional, regression, performance, security
3. Blocking bugs: list with ID, title, severity and impacted user flow
4. Non-blocking bugs: list with ID, title, severity and risk assessment
5. Comparison with exit criteria from the test plan
6. Go / No-go recommendation with explicit justification

Language: Brazilian Portuguese

ANTI-PATTERNS — apply blocking rules:
- BLOCKED if: the go/no-go recommendation is missing, implicit, or not tied to exit criteria
  ❌ "Os testes foram executados. A maioria dos casos passou. Existem alguns problemas em aberto."
  ✅ "NO-GO — 87/100 casos passaram (87%). Critério de saída exige 95%. 2 bugs críticos bloqueiam o fluxo de checkout: BUG-014 e BUG-017. Recomendação: corrigir os bloqueantes e re-executar o suite de regressão antes de prosseguir com o deploy."
- BLOCKED if: any bug in the blocking list has no severity classification or no impacted user flow
  ❌ "BUG-014: erro na tela de pagamento."
  ✅ "BUG-014 | Severidade: Crítica | Pagamento com cartão de crédito falha quando o CVV tem 4 dígitos (cartões Amex) | Fluxo afetado: checkout — etapa 3/4 | Bloqueante para deploy: SIM | Workaround disponível: NÃO."
- BLOCKED if: coverage is expressed only as a percentage without the absolute numbers
  ❌ "Cobertura de testes: 92%."
  ✅ "Cobertura de testes: 92% (92/100 casos planejados executados). Casos não executados: 8 — bloqueados por indisponibilidade do ambiente de integração nos dias 03 e 04/07."
```

## Critério de sucesso

- [ ] A recomendação de go ou no-go é explícita, está destacada no documento e tem justificativa baseada nos critérios de saída do plano de testes
- [ ] Os bugs bloqueantes estão listados separadamente dos não bloqueantes, com indicação do fluxo de usuário afetado
- [ ] A cobertura está expressa em número absoluto de casos e em porcentagem do total planejado
- [ ] O relatório compara os resultados obtidos com os critérios de saída definidos no plano de testes

## Boas práticas verificadas

- [ ] O sumário executivo é legível por gestores sem conhecimento técnico detalhado
- [ ] Cada bug bloqueante tem indicação de qual critério de saída ele impede de atingir
- [ ] Bugs não bloqueantes têm avaliação de risco para produção (probabilidade de impacto ao usuário)
- [ ] O relatório registra o ambiente exato em que os testes foram executados (versão do sistema, data dos dados)
