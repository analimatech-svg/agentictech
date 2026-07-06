# Skill: Status Report

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning / Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar um relatório de status do projeto registrando o que foi concluído, o que está em andamento, os impedimentos ativos e os próximos passos, produzido na abertura do projeto (baseline) e periodicamente durante o monitoramento.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| estado atual do projeto | Sim | Lista do que foi concluído e do que está em andamento neste ciclo |
| impedimentos conhecidos | Sim | Bloqueios ativos que impedem avanço (pode ser vazio se não houver nenhum) |
| próximos marcos | Sim | Entregas ou eventos previstos para o próximo ciclo |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/status-report-YYYY-MM-DD.md`
- **Campos obrigatórios:** data do relatório, status geral (Verde/Amarelo/Vermelho), concluído no ciclo, em andamento, impedimentos (seção obrigatória mesmo que vazia), próximos passos, data prevista da próxima atualização

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a project status report based on the provided context.

Structure the document as follows:
1. Report date and status indicator (Green / Yellow / Red with justification).
2. Completed this cycle: bullet list of what was delivered or closed.
3. In progress: bullet list of what is currently being worked on, with expected completion.
4. Impediments: bullet list of active blockers. If there are none, include the section with "No active impediments."
5. Next steps: what is planned for the next cycle, with responsible owner when known.
6. Next update: the date or trigger for the next status report.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] A seção de impedimentos está presente mesmo quando está vazia (registrada como "Sem impedimentos ativos")
- [ ] O relatório tem data de emissão explícita
- [ ] Há data ou condição prevista para a próxima atualização

## Boas práticas verificadas

- [ ] Governança (decisões registradas e rastreáveis)
- [ ] Documentação em Markdown (formatação consistente)
