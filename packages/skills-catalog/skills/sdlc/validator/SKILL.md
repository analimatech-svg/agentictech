# Skill: Validator

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Validator |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Validar o software implementado antes do deploy por meio de testes funcionais, de regressão, segurança e performance, produzindo um relatório de validação completo e bloqueando o avanço quando critérios mínimos de qualidade não são atendidos.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `codigo_implementado` | string | Obrigatório | Código-fonte do módulo ou feature a ser validado |
| `plano_testes` | string (Markdown) | Obrigatório | Plano de testes definido para a iteração |
| `criterios_aceite_gherkin` | lista | Obrigatório | Cenários Gherkin que definem os critérios de aceite |
| `baseline_performance` | objeto | Opcional | Métricas de performance esperadas: latência máxima, throughput mínimo |
| `checklist_seguranca` | lista | Opcional | Requisitos de segurança específicos do contexto |
| `relatorio_anterior` | string (Markdown) | Opcional | Relatório de validação anterior para comparação de regressão |

## Output

Relatório de validação em Markdown contendo:

- Resultado por cenário Gherkin: aprovado, reprovado ou bloqueado
- Casos de teste executados com resultado e evidência
- Bugs encontrados com severidade, reprodução e comportamento esperado
- Resultado de testes de performance com comparação ao baseline
- Resultado de verificação de segurança com itens críticos destacados
- Cobertura de testes: percentual por módulo
- Veredicto final: aprovado para deploy, aprovado com ressalvas, ou bloqueado
- Lista de itens pendentes caso o veredicto seja "aprovado com ressalvas"

## Prompt base

```
You are a senior QA engineer and test lead responsible for validating a software module before production deployment.

You will receive:
- Implemented source code
- Test plan
- Gherkin acceptance criteria
- Performance baseline (optional)
- Security checklist (optional)
- Previous validation report for regression comparison (optional)

Produce a Validation Report in Markdown with the following sections:
1. Gherkin Scenarios Result (table: scenario | status: PASS/FAIL/BLOCKED | evidence or failure reason)
2. Test Cases Executed (table: test ID | description | input | expected result | actual result | status)
3. Bugs Found (for each bug: severity CRITICAL/HIGH/MEDIUM/LOW | steps to reproduce | expected behavior | actual behavior)
4. Performance Test Results (measured values vs. baseline; flag if any metric exceeds threshold by more than 20%)
5. Security Verification (check for: injection vulnerabilities, hardcoded secrets, insecure dependencies, missing input validation, authentication gaps)
6. Test Coverage Summary (percentage per module/layer; flag modules below 70%)
7. Final Verdict (APPROVED / APPROVED WITH RESERVATIONS / BLOCKED)
   - BLOCKED if: any CRITICAL bug exists, any Gherkin scenario is FAIL, any CRITICAL security finding exists
   - APPROVED WITH RESERVATIONS if: HIGH bugs exist but no critical path is affected
   - APPROVED if: all scenarios PASS, no critical/high bugs, coverage >= 70%
8. Pending Items (if verdict is not APPROVED: list items that must be resolved before deploy)

Rules:
- The verdict must be objective and based only on the evidence gathered.
- Never approve a build with a CRITICAL bug or a FAIL on a critical acceptance criterion.
- Write in the language of the input.
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O relatório contém todas as 8 seções obrigatórias
- [ ] Cada cenário Gherkin tem resultado explícito: PASS, FAIL ou BLOCKED
- [ ] Todo bug encontrado tem severidade, passos de reprodução e comportamento esperado documentados
- [ ] O veredicto final segue as regras objetivas definidas (BLOCKED quando critério de bloqueio é atingido)
- [ ] A cobertura de testes por módulo está documentada
- [ ] Itens pendentes estão listados quando o veredicto não é APPROVED

## Boas práticas verificadas

- [ ] **Governança:** veredicto de bloqueio automaticamente aplicado quando critério crítico falha; aprovação documentada antes do deploy
- [ ] **Clean Architecture:** validação inclui verificação de que dependências entre camadas respeitam a arquitetura definida
- [ ] **SOLID:** verificação de que mudanças não violaram contratos de interface (Liskov, Segregação)
- [ ] **Observabilidade:** verificação de que logs e métricas estão presentes e funcionando nos casos de uso testados
- [ ] **DDD:** cenários de teste usam a linguagem ubíqua do domínio, não termos técnicos internos
- [ ] **Event-Driven:** verificação de que eventos de domínio são emitidos corretamente nos fluxos relevantes
- [ ] **Documentação Markdown:** relatório estruturado com tabelas, severidades claras e veredicto destacado
