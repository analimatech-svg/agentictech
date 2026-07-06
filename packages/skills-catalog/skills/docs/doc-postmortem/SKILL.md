# Skill: Postmortem sem Culpa

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Regente |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar um postmortem sem culpa a partir de um incidente, documentando a linha do tempo, o impacto nos usuários, a causa raiz identificada pelo método dos 5 Porquês, as ações corretivas com responsável e prazo, e o que funcionou bem durante a resposta ao incidente.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| linha_do_tempo | Sim | Sequência de eventos desde a detecção até a resolução, com horários |
| impacto_medido | Sim | Número de usuários afetados, funcionalidades indisponíveis e duração total |
| logs_relevantes | Não | Trechos de logs ou métricas que sustentam a análise de causa raiz |
| acoes_tomadas | Sim | Ações executadas durante o incidente e quem as executou |
| acoes_corretivas | Sim | Lista de melhorias planejadas com responsável nomeado e prazo definido |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/postmortem-{AAAA-MM-DD}-{slug-incidente}.md`
- **Campos obrigatórios:** resumo executivo, linha do tempo, impacto, análise de causa raiz (5 Porquês), o que funcionou bem, ações corretivas com responsável e prazo

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a blameless postmortem document based on the provided context.

Rules:
- Never assign blame to individuals. Root cause analysis must reach the level of process, system design, tooling, or organizational factors.
- Use the 5 Whys method to drill down to the root cause. Each "why" must be answered with evidence, not assumption.
- Include an explicit "What worked well" section to reinforce positive behaviors and responses.
- Every corrective action must have: a named owner (role, not just a name), a defined deadline, and a measurable outcome.
- Tone: factual, neutral, constructive. No judgment language.
- Structure: Executive summary, Timeline, Impact, Root cause analysis (5 Whys), What worked well, Corrective actions, Open questions.

Context:
- Timeline: {linha_do_tempo}
- Measured impact: {impacto_medido}
- Relevant logs: {logs_relevantes}
- Actions taken during incident: {acoes_tomadas}
- Planned corrective actions: {acoes_corretivas}

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] A causa raiz chega ao nível de processo, sistema ou fator organizacional, sem apontar responsabilidade individual
- [ ] Cada ação corretiva tem responsável nomeado (pelo papel) e prazo definido com data
- [ ] A seção "O que funcionou bem" está presente e contém pelo menos dois pontos positivos concretos
- [ ] Os 5 Porquês estão encadeados com evidências, não com suposições
- [ ] O documento pode ser compartilhado com toda a organização sem constrangimento para nenhuma pessoa

## Boas práticas verificadas

- [ ] Linguagem neutra e factual em todo o documento, sem adjetivos de julgamento
- [ ] Linha do tempo com horários precisos e atribuição de eventos ao sistema ou processo (não à pessoa)
- [ ] Impacto quantificado: número de usuários, tempo de indisponibilidade, transações afetadas
- [ ] Causa raiz sustentada por evidência de log ou métrica
- [ ] Ações corretivas organizadas em tabela com responsável, prazo e critério de conclusão
