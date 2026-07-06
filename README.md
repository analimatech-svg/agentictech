# AgenticTech — Registry de Skills para o SDLC Completo

> Da Ideação ao Suporte, com IA.

Registry validado de skills agênticas para o ciclo de vida de desenvolvimento de software (SDLC). Inspirado no modelo do [tech-leads-club/agent-skills](https://github.com/tech-leads-club/agent-skills), focado no fluxo completo: do planejamento ao monitoramento em produção.

## O que é este repositório

Uma coleção de skills reutilizáveis que ensinam e assistem profissionais a construir software com IA — seguindo boas práticas de mercado (DDD, Clean Architecture, SOLID, Event-Driven, Governança, Observabilidade).

Cada skill é uma unidade de capacidade especializada: autocontida, com contexto próprio, template e critério de sucesso definido.

## Para quem é

- Pessoas aprendendo a trabalhar com IA no desenvolvimento de software
- Devs que querem estruturar melhor seu fluxo com agentes
- Times que querem padronizar como usam IA no SDLC

## Escala Maestro

Todo usuário entra como **Aprendiz** e evolui:

| Nível | O que o Consultor de IA faz |
|---|---|
| Aprendiz | Entende o processo, executa manualmente com guia passo a passo |
| Praticante | Usa IA como assistente, começa a delegar tarefas simples |
| Regente | Dirige agentes, valida outputs, configura pipelines |
| Maestro | Governa a stack, define estratégia, cria skills novas de forma autônoma |

## Estrutura do Repositório

```
packages/skills-catalog/
├── judge/              ← Roteamento e detecção de nível Maestro
├── pipelines/          ← Sequências predefinidas de skills
├── context/            ← Templates de contexto para injeção nas skills
├── glossary/           ← Vocabulário único do repositório
├── indicators/         ← KPIs e métricas por fase do SDLC
├── onboarding/         ← Assessment inicial de nível Maestro
└── skills/
    ├── sdlc/           ← Skills de fase (planning → monitor)
    ├── quality/        ← Skills transversais de qualidade
    └── docs/           ← Skills de geração de documentos
```

## Fases do SDLC cobertas

| # | Fase | Skill |
|---|---|---|
| 1 | Planning & Setup | `sdlc/planning` |
| 2 | Ideação e Planejamento | `sdlc/discovery` |
| 3 | Design | `sdlc/design` |
| 4 | Arquitetura | `sdlc/architecture` |
| 5 | Desenvolvimento | `sdlc/builder` |
| 6 | Testes e Qualidade | `sdlc/validator` |
| 7 | Implantação e Lançamento | `sdlc/deploy` |
| 8 | Monitoramento e Suporte | `sdlc/monitor` |

## Skills de Qualidade (transversais)

| Skill | O que faz |
|---|---|
| `quality/best-practices` | Valida DDD, Clean Architecture, SOLID, Event-Driven em qualquer fase |
| `quality/code-review` | Revisão de código com critérios técnicos |
| `quality/security` | Segurança e compliance — Architecture, Deploy e Monitor |

## Skills de Documentação (por artefato)

`doc-business-case` · `doc-requirements` · `doc-roadmap` · `doc-user-story` · `doc-wireframe` · `doc-mockup` · `doc-prototype` · `doc-persona` · `doc-user-journey` · `doc-adr` · `doc-architecture` · `doc-code-style` · `doc-security` · `doc-changelog` · `doc-test-plan` · `doc-test-cases` · `doc-test-report` · `doc-bug-report` · `doc-deploy-plan` · `doc-rollback-plan` · `doc-user-guide` · `doc-knowledge-base` · `doc-release-notes` · `doc-postmortem` · `doc-runbook` · `doc-status-report`

## Compatibilidade

Compatível com Claude Code, Cursor, Cline, GitHub Copilot e qualquer agente que suporte arquivos de instrução em Markdown.

## Licença

MIT
