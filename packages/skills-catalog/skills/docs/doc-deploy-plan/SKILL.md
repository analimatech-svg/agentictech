# Skill: Plano de Deploy

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Deploy |
| Categoria | docs |
| Nível mínimo Maestro | Regente |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar o plano de deploy com sequência de passos, validações intermediárias, janela de manutenção, responsáveis e critérios objetivos de sucesso.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| software_validado | Sim | Descrição do software que será publicado (versão, artefatos gerados, resultado dos testes) |
| arquitetura | Sim | Arquitetura de infraestrutura: servidores, banco de dados, serviços externos, CDN, filas |
| historico_deploys | Não | Registro de deploys anteriores: problemas encontrados, tempo médio de execução, itens de atenção |
| janela_manutencao | Não | Janela de manutenção proposta (horário, duração máxima, critério de cancelamento) |
| equipe | Não | Nomes e papéis dos responsáveis pelo deploy |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/deploy-plan.md`
- **Campos obrigatórios:** sequência numerada de passos, responsável por cada passo, validação após cada etapa crítica, janela de manutenção com critério de cancelamento, critérios de sucesso do deploy

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a deploy plan based on the provided architecture and software context.

Include the following sections:
1. Pre-deploy checklist: artifacts validated, approvals obtained, team notified
2. Execution steps: numbered, with responsible party and estimated duration per step
3. Intermediate validations: smoke tests or health checks after critical steps (database migrations, service restarts, CDN cache invalidation)
4. Maintenance window: start time, maximum duration, cancellation criteria
5. Success criteria: observable and measurable conditions that confirm the deploy succeeded
6. Contacts: who to call if a step fails, with role and contact information

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada passo tem pelo menos um responsável nomeado (pessoa ou papel, como "Tech Lead")
- [ ] Há validação intermediária após cada etapa crítica, como migração de banco de dados, reinicialização de serviço ou invalidação de cache
- [ ] A janela de manutenção está definida com horário, duração máxima e critério objetivo de cancelamento
- [ ] Os critérios de sucesso do deploy são observáveis e mensuráveis (ex: health check retorna HTTP 200, taxa de erro abaixo de 0,1% por 5 minutos)

## Boas práticas verificadas

- [ ] O plano prevê a notificação aos usuários antes do início da manutenção, quando aplicável
- [ ] Cada validação intermediária tem tempo máximo de espera e ação definida caso não passe (aguardar mais X minutos ou acionar rollback)
- [ ] O plano lista o contato de escalonamento para cada etapa crítica, não apenas o responsável técnico
- [ ] A ordem dos passos respeita dependências técnicas (banco antes da aplicação, migração antes do serviço que consome os novos campos)
