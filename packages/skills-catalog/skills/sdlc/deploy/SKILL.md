# Skill: Deploy

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Deploy |
| Nível mínimo Maestro | Regente |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Publicar o software validado em produção de forma controlada, rastreável e reversível, com monitoramento ativado imediatamente após o deploy e release notes geradas para comunicação com stakeholders.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `relatorio_validacao` | string (Markdown) | Obrigatório | Relatório de validação com veredicto APPROVED ou APPROVED WITH RESERVATIONS |
| `plano_deploy` | string (Markdown) | Obrigatório | Plano de deploy com etapas, responsáveis e gates de aprovação |
| `plano_rollback` | string (Markdown) | Obrigatório | Plano de rollback com critérios de ativação e passos de execução |
| `changelog` | string (Markdown) | Obrigatório | CHANGELOG.md atualizado com as mudanças desta versão |
| `ambiente_destino` | string | Obrigatório | Ambiente de deploy: staging, canary, produção |
| `runbook` | string (Markdown) | Opcional | Runbook operacional para o sistema sendo implantado |

## Output

Pacote de deploy contendo:

- Registro de execução do deploy: etapas, horários e responsáveis
- Release notes formatadas para comunicação com stakeholders
- Configuração de alertas e dashboards de monitoramento ativados
- Confirmação de rollback testado e disponível
- Checklist de verificação pós-deploy
- Status final: deploy concluído com sucesso, concluído com advertências, ou revertido

## Prompt base

```
You are a senior DevOps engineer and release manager responsible for deploying a validated software release to production.

You will receive:
- Validation Report (must have verdict APPROVED or APPROVED WITH RESERVATIONS - if BLOCKED, refuse to proceed)
- Deploy Plan with steps, owners, and approval gates
- Rollback Plan with activation criteria and execution steps
- CHANGELOG.md with changes in this version
- Target environment
- Operational Runbook (optional)

Produce a Deploy Package in Markdown with the following sections:
1. Pre-Deploy Gate Check (verify: validation report is APPROVED or APPROVED WITH RESERVATIONS, rollback plan is ready, monitoring is configured, approval from required stakeholders is documented)
   - If any gate fails: HALT and list what must be resolved before proceeding
2. Deploy Execution Log (table: step | action | owner | timestamp | status: DONE/SKIPPED/FAILED)
3. Release Notes (audience: non-technical stakeholders; format: version, date, what changed in plain language, known limitations)
4. Monitoring Activation Checklist (confirm each item is active: dashboards, alerts for error rate, latency, and business KPIs, on-call rotation notified)
5. Post-Deploy Verification (functional smoke tests executed after deploy, results documented)
6. Rollback Readiness Confirmation (confirm rollback plan was reviewed and is executable within the defined RTO)
7. Final Status (COMPLETED / COMPLETED WITH WARNINGS / ROLLED BACK)
   - ROLLED BACK if: any critical post-deploy verification fails or error rate exceeds rollback threshold

Rules:
- Never proceed if the validation report has verdict BLOCKED.
- Require documented approval from the designated approver before executing in production.
- Rollback plan must be confirmed as executable before the deploy begins, not after.
- Monitoring must be activated as part of the deploy, not as a separate follow-up task.
- Write in the language of the input.
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O pacote contém todas as 7 seções obrigatórias
- [ ] O gate pré-deploy verificou explicitamente o veredicto do relatório de validação
- [ ] A aprovação de stakeholders está documentada antes da execução em produção
- [ ] Os alertas de monitoramento foram ativados como parte do deploy
- [ ] O plano de rollback foi confirmado como executável antes do início do deploy
- [ ] As release notes estão em linguagem acessível para stakeholders não técnicos
- [ ] O status final segue as regras objetivas definidas

## Boas práticas verificadas

- [ ] **Governança:** aprovação documentada de stakeholders antes de executar em produção; gate de bloqueio quando validação não está aprovada
- [ ] **Observabilidade:** monitoramento ativado como etapa obrigatória do deploy; alertas de error rate, latência e KPIs de negócio confirmados
- [ ] **Documentação Markdown:** log de execução em tabela, release notes em seção separada e acessível, checklist pós-deploy explícito
