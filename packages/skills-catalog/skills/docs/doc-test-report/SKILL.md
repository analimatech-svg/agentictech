# Skill: Relatório de Testes

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Validator |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar o relatório consolidado de execução dos testes, com métricas de cobertura, bugs encontrados e recomendação explícita de go ou no-go para o deploy.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| casos_executados | Sim | Lista de casos de teste com status de execução (Passou, Falhou, Bloqueado) |
| bugs_registrados | Sim | Lista de bugs encontrados com severidade (crítica, alta, média, baixa) |
| plano_testes | Sim | Plano de testes com critérios de saída definidos |
| data_execucao | Sim | Período de execução dos testes (data de início e data de fim) |
| ambiente | Não | Ambiente em que os testes foram executados (staging, homologação, etc.) |

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
