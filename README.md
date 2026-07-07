# MaestroAI-Tech — Registry de Skills para o SDLC Completo

> Formando Consultores de IA em desenvolvimento de software — do planejamento ao monitoramento em produção.

Registry de 43 skills agênticas validadas para o ciclo de vida completo de desenvolvimento de software. Cada skill é uma unidade de capacidade especializada: autocontida, com contexto próprio, prompt estruturado, critério de sucesso definido e boas práticas verificadas. O Consultor de IA aprende construindo — não estudando teoria.

---

## Para quem é

**Consultores de IA** que trabalham com desenvolvimento de software e querem:

- Estruturar como usam IA em cada fase do SDLC
- Elevar a qualidade dos entregáveis com critérios objetivos
- Evoluir de executor pontual a gestor de stack agêntica

**Times de tecnologia** que querem:

- Padronizar o uso de IA no ciclo de desenvolvimento
- Rastrear decisões técnicas com artefatos verificáveis
- Reduzir variação de qualidade entre sprints e projetos

---

## Escala Maestro

Todo Consultor de IA começa como Aprendiz e conquista autonomia progressivamente. Os níveis não são autoatribuídos — são verificados pelo Judge com base em evidência.

| Nível | O que o Consultor de IA faz | Critério de progressão |
|---|---|---|
| **Aprendiz** | Executa cada fase com guia passo a passo | 3 artefatos criados e validados sem pular etapas |
| **Praticante** | Usa IA como assistente, encadeia pipelines simples | Pipeline de 3 skills com output válido em todas as etapas |
| **Regente** | Dirige agentes, valida outputs, configura pipelines completos | Ciclo SDLC completo entregue sem intervenção além de confirmações |
| **Maestro** | Governa a stack agêntica, cria skills novas de forma autônoma | Nova skill criada do zero, documentada, testada e integrada ao pipeline |

---

## Arquitetura do Registry

```
packages/skills-catalog/
├── judge/              ← Verificação central e roteamento por nível Maestro
├── pipelines/          ← Sequências predefinidas: sdlc-completo, mvp-rapido, analise-codebase
├── context/            ← Templates de contexto para injeção nas skills
├── glossary/           ← Vocabulário único do registry
├── indicators/         ← KPIs e métricas por fase do SDLC
├── onboarding/         ← Assessment inicial para identificar nível Maestro
└── skills/
    ├── sdlc/           ← 8 skills de execução das fases (planning → monitor)
    ├── quality/        ← 3 skills transversais de qualidade
    └── docs/           ← 31 skills de geração de artefatos (uma por tipo de documento)
```

**43 skills · 3 pipelines · 8 conjuntos de indicadores · 100% Ouro na QUALITY-RUBRIC**

---

## As 8 Fases do SDLC

| # | Fase | Skill de execução | Nível mínimo |
|---|---|---|---|
| 1 | Planning | `sdlc/planning` | Aprendiz |
| 2 | Discovery | `sdlc/discovery` | Praticante |
| 3 | Design | `sdlc/design` | Aprendiz |
| 4 | Architecture | `sdlc/architecture` | Praticante |
| 5 | Builder | `sdlc/builder` | Praticante |
| 6 | Validator | `sdlc/validator` | Praticante |
| 7 | Deploy | `sdlc/deploy` | Regente |
| 8 | Monitor | `sdlc/monitor` | Regente |

---

## Skills de Qualidade (transversais)

Executadas automaticamente pelo Judge em todo output — podem bloquear o avanço entre fases.

| Skill | O que faz | Quando bloqueia |
|---|---|---|
| `quality/best-practices` | Audita DDD, Clean Architecture, SOLID, Event-Driven, Governança, Observabilidade, Markdown | Violação crítica em qualquer princípio |
| `quality/code-review` | Legibilidade, duplicação, complexidade ciclomática, SRP | Violação crítica de SRP ou acoplamento excessivo |
| `quality/security` | OWASP Top 10: injeção, autenticação, exposição de dados | Vulnerabilidade crítica antes do deploy |

---

## Skills de Documentação (31 artefatos)

Cada skill produz um artefato verificável. A Escala Maestro define o nível mínimo para usar cada uma.

**Planning / Gestão**
`doc-business-case` · `doc-roadmap` · `doc-work-breakdown` · `doc-sprint-plan` · `doc-status-report`

**Discovery / Pesquisa**
`doc-requirements` · `doc-persona` · `doc-user-journey`

**Design / UX**
`doc-user-story` · `doc-wireframe` · `doc-mockup` · `doc-prototype`

**Architecture / Técnico**
`doc-adr` · `doc-architecture` · `doc-security` · `doc-code-style` · `doc-data-model` · `doc-api-contract`

**Builder**
`doc-changelog`

**Validator / Qualidade**
`doc-test-plan` · `doc-test-cases` · `doc-test-report` · `doc-bug-report`

**Deploy / Operações**
`doc-deploy-plan` · `doc-rollback-plan` · `doc-release-notes`

**Monitor / Melhoria Contínua**
`doc-runbook` · `doc-postmortem` · `doc-user-guide` · `doc-knowledge-base` · `doc-retrospective`

---

## O Judge

O Judge é o componente central de verificação. Recebe qualquer artefato + nome da skill + nível Maestro e emite um Relatório de Verificação com veredicto **APROVADO / APROVADO COM RESSALVAS / BLOQUEADO**.

Quando bloqueia, nomeia o critério exato que falhou, cita o trecho específico do artefato e descreve a ação corretiva concreta. Nunca bloqueia com motivo genérico.

```
Princípio: Entender antes de agir. Propor antes de executar. Validar antes de avançar.
```

---

## Os 3 Pipelines

| Pipeline | Skills | Nível mínimo | Quando usar |
|---|---|---|---|
| `sdlc-completo` | 38 skills, 8 fases | Regente | Projeto novo com rastreabilidade total |
| `mvp-rapido` | 10 skills, foco no essencial | Praticante | Validar hipótese em produção rapidamente |
| `analise-codebase` | 7 skills, auditoria | Aprendiz | Entender e documentar sistema existente |

---

## Compatibilidade

Compatível com Claude Code, Cursor, Cline, GitHub Copilot e qualquer agente que suporte arquivos de instrução em Markdown.

---

## Licença

MIT · © 2026 MentoraTech
