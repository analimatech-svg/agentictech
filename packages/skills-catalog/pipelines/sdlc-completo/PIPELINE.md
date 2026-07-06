# Pipeline: SDLC Completo

> Ciclo completo de desenvolvimento de software, da ideação ao suporte em produção.
> Indicado para Consultores de IA no nível Regente ou Maestro.

## Quando usar

- O projeto é novo e precisa de estrutura completa
- O cliente quer rastreabilidade total das decisões
- O time quer documentação viva junto com o desenvolvimento

## Nível mínimo na Escala Maestro

Regente

---

## Sequência de skills

| Ordem | Fase | Skill | Output principal |
|---|---|---|---|
| 1 | Planning | `skills/sdlc/planning` | Documento de visão, escopo e critérios de sucesso |
| 2 | Planning | `skills/docs/doc-business-case` | Business Case aprovado |
| 3 | Planning | `skills/docs/doc-roadmap` | Roadmap por fase |
| 4 | Planning | `skills/docs/doc-status-report` | Status Report inicial |
| 5 | Discovery | `skills/sdlc/discovery` | Relatório de discovery com hipóteses validadas |
| 6 | Discovery | `skills/docs/doc-requirements` | Documento de requisitos |
| 7 | Discovery | `skills/docs/doc-persona` | Personas do sistema |
| 8 | Discovery | `skills/docs/doc-user-journey` | Jornada do usuário |
| 9 | Design | `skills/sdlc/design` | Design funcional e fluxos aprovados |
| 10 | Design | `skills/docs/doc-wireframe` | Wireframes das telas principais |
| 11 | Design | `skills/docs/doc-mockup` | Mockups com identidade visual |
| 12 | Design | `skills/docs/doc-prototype` | Protótipo navegável |
| 13 | Design | `skills/docs/doc-user-story` | User Stories com critérios Gherkin |
| 14 | Architecture | `skills/sdlc/architecture` | Documento de arquitetura técnica |
| 15 | Architecture | `skills/docs/doc-adr` | ADRs das decisões relevantes |
| 16 | Architecture | `skills/docs/doc-security` | Documento de segurança |
| 17 | Architecture | `skills/docs/doc-code-style` | Guia de estilo de código |
| 18 | Builder | `skills/sdlc/builder` | Código implementado com documentação inline |
| 19 | Builder | `skills/docs/doc-changelog` | Changelog da versão |
| 20 | Validator | `skills/sdlc/validator` | Relatório de validação |
| 21 | Validator | `skills/docs/doc-test-plan` | Plano de testes |
| 22 | Validator | `skills/docs/doc-test-cases` | Casos de teste |
| 23 | Validator | `skills/docs/doc-test-report` | Relatório de testes |
| 24 | Validator | `skills/docs/doc-bug-report` | Relatório de bugs encontrados |
| 25 | Deploy | `skills/sdlc/deploy` | Deploy em produção |
| 26 | Deploy | `skills/docs/doc-deploy-plan` | Plano de deploy |
| 27 | Deploy | `skills/docs/doc-rollback-plan` | Plano de rollback |
| 28 | Deploy | `skills/docs/doc-release-notes` | Release notes da versão |
| 29 | Monitor | `skills/sdlc/monitor` | Dashboard de monitoramento ativo |
| 30 | Monitor | `skills/docs/doc-runbook` | Runbook operacional |
| 31 | Monitor | `skills/docs/doc-postmortem` | Postmortem (se incidente ocorrer) |
| 32 | Monitor | `skills/docs/doc-knowledge-base` | Base de conhecimento |
| 33 | Monitor | `skills/docs/doc-user-guide` | Guia do usuário |

---

## Critério de conclusão

O pipeline é considerado concluído quando:
- Todos os documentos obrigatórios estão gerados e aprovados pelo Consultor de IA
- O sistema está em produção com monitoramento ativo
- O runbook está disponível para o time de operações

## Pontos de checkpoint obrigatório

O Judge solicita aprovação explícita antes de avançar entre as seguintes fases:
- Planning → Discovery
- Discovery → Design
- Design → Architecture
- Architecture → Builder
- Builder → Validator
- Validator → Deploy
- Deploy → Monitor
