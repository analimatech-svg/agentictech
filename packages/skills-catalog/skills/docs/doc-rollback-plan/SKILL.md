# Skill: Plano de Rollback

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Deploy |
| Categoria | docs |
| Nível mínimo Maestro | Regente |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar o plano de rollback com critérios objetivos de acionamento, sequência de passos para reversão, responsáveis pela decisão e pela execução, e contato de escalonamento.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| plano_deploy | Sim | Plano de deploy correspondente, com etapas e validações definidas |
| arquitetura | Sim | Arquitetura de infraestrutura: servidores, banco de dados, serviços externos |
| versao_estavel_anterior | Sim | Versão que estava em produção antes do deploy (artefato, tag ou branch) |
| historico_rollbacks | Não | Registro de rollbacks anteriores: problemas encontrados e lições aprendidas |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/rollback-plan.md`
- **Campos obrigatórios:** critérios de acionamento objetivos, tempo máximo para decisão, responsável pela decisão, responsável pela execução, sequência de passos de reversão, validação pós-rollback, contato de escalonamento

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a rollback plan based on the provided deploy plan and infrastructure architecture.

Include the following sections:
1. Rollback triggers: objective and measurable criteria (error rate above X% for N minutes, health check failing for N seconds, critical data inconsistency detected)
2. Decision timeline: maximum time to decide between continue, wait, or rollback after a trigger is detected
3. Decision authority: who has the authority to approve rollback
4. Execution steps: numbered, with responsible party, estimated duration and validation after each step
5. Post-rollback validation: how to confirm the system is stable in the previous version
6. Escalation contact: who to call if rollback also fails, with role and contact information

Notes:
- Rollback steps must be tested before the deploy, not theoretical
- Triggers must be objective, not based on subjective perception
- Database rollback steps must be explicit about migration reversibility

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] O critério de acionamento do rollback é objetivo e mensurável, sem termos como "se parecer instável" ou "se houver muitas reclamações"
- [ ] Os passos de rollback foram testados antes do deploy (não são teóricos) e isso está registrado no documento
- [ ] Há contato de escalonamento definido para o caso de o rollback também falhar, com nome, papel e forma de contato

## Boas práticas verificadas

- [ ] O tempo máximo para a decisão de rollback está definido para evitar paralisia durante incidente
- [ ] O plano diferencia o responsável pela decisão do responsável pela execução do rollback
- [ ] Reversão de migração de banco de dados é tratada explicitamente, com indicação de reversibilidade ou de estratégia alternativa quando não for reversível
- [ ] A validação pós-rollback inclui verificação dos principais fluxos de usuário, não apenas o health check do servidor
