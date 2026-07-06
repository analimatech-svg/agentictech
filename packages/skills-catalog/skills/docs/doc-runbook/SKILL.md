# Skill: Runbook Operacional

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Regente |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar um runbook operacional para uma tarefa recorrente, com passos detalhados, comandos prontos para execução, saídas esperadas em cada etapa, critérios de decisão objetivos e contatos de escalonamento, para que qualquer membro do time consiga executar a tarefa sem conhecimento prévio do sistema.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| tarefa_operacional | Sim | Nome e descrição da tarefa a documentar (ex: rotina de backup, restart de serviço, deploy manual) |
| stack_infra | Sim | Tecnologias e ferramentas envolvidas (ex: AWS, Kubernetes, PostgreSQL, Bash) |
| historico_execucoes | Não | Registros de execuções anteriores, incluindo erros conhecidos e soluções aplicadas |
| contatos_escalonamento | Sim | Nome, função, canal de contato e horário de disponibilidade de cada responsável |
| pre_requisitos | Não | Acessos, permissões e ferramentas necessárias antes de iniciar a tarefa |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/runbook-{slug-da-tarefa}.md`
- **Campos obrigatórios:** propósito, quando usar, pré-requisitos, passos com comandos e saídas esperadas, decisões com critério objetivo, contatos de escalonamento com horário

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate an operational runbook based on the provided context.

Rules:
- The runbook must be executable by any team member with no prior knowledge of the system.
- Each step must include: the action to perform, the exact command or UI interaction, and the expected output.
- Every decision point must have an objective criterion (e.g., "if exit code is non-zero, go to step X" — never "evaluate if necessary").
- Escalation contacts must include name, role, communication channel, and availability hours.
- Structure: Purpose, When to use, Prerequisites, Step-by-step (numbered), Common errors and solutions, Escalation contacts.

Context:
- Task: {tarefa_operacional}
- Infrastructure stack: {stack_infra}
- Previous execution history: {historico_execucoes}
- Escalation contacts: {contatos_escalonamento}
- Prerequisites: {pre_requisitos}

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Qualquer membro do time consegue executar a tarefa do início ao fim sem consultar outra pessoa
- [ ] Cada passo tem o comando exato ou a ação de interface a realizar, e a saída esperada ao final
- [ ] Cada decisão tem critério objetivo e mensurável (sem "avalie se necessário" ou "conforme o caso")
- [ ] Contatos de escalonamento incluem nome, canal e horário de disponibilidade
- [ ] Erros conhecidos têm solução documentada com passo a passo

## Boas práticas verificadas

- [ ] Propósito e contexto da tarefa explicados no início do documento
- [ ] Pré-requisitos listados antes do primeiro passo (acessos, permissões, ferramentas)
- [ ] Comandos em bloco de código com linguagem especificada (bash, sql, etc.)
- [ ] Saída esperada descrita logo após cada comando
- [ ] Seção de erros frequentes com sintoma, causa provável e solução
- [ ] Contatos de escalonamento em tabela com disponibilidade horária
