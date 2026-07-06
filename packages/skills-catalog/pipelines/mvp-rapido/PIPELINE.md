# Pipeline: MVP Rápido

> Subconjunto essencial do SDLC para validar uma hipótese em produção com o mínimo viável.
> Indicado para Consultores de IA no nível Praticante ou acima.

## Quando usar

- A ideia ainda não foi validada com usuários reais
- O objetivo é aprender, não entregar um produto completo
- O prazo é curto e a prioridade é velocidade de aprendizado

## Nível mínimo na Escala Maestro

Praticante

---

## Sequência de skills

| Ordem | Fase | Skill | Output principal |
|---|---|---|---|
| 1 | Planning | `skills/sdlc/planning` | Escopo mínimo e hipótese central |
| 2 | Discovery | `skills/sdlc/discovery` | Problema validado com 3 a 5 entrevistas |
| 3 | Discovery | `skills/docs/doc-requirements` | Requisitos mínimos (apenas o essencial) |
| 4 | Design | `skills/docs/doc-user-story` | 3 a 5 USs da jornada principal |
| 5 | Design | `skills/docs/doc-wireframe` | Wireframe da tela principal |
| 6 | Architecture | `skills/docs/doc-adr` | Uma decisão de arquitetura: stack e modelo de dados |
| 7 | Builder | `skills/sdlc/builder` | Código do fluxo principal |
| 8 | Validator | `skills/docs/doc-test-cases` | Casos de teste do fluxo principal |
| 9 | Deploy | `skills/sdlc/deploy` | MVP em produção |
| 10 | Monitor | `skills/docs/doc-status-report` | Primeiras métricas de uso |

---

## Critério de conclusão

- A hipótese principal foi testada com usuários reais
- O sistema está em produção, mesmo que com limitações
- Existe pelo menos uma métrica de uso sendo coletada

## O que fica de fora (intencionalmente)

- Postmortem (não há incidente esperado)
- Rollback Plan formal (reversão manual é aceitável no MVP)
- Knowledge Base (ainda não há base de conhecimento suficiente)
- Runbook (operação manual no MVP)

Esses documentos são criados no ciclo seguinte, quando o MVP evolui para produto.
