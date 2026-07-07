# Skill: Architecture

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Architecture |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Definir a estrutura técnica da solução, padrões de desenvolvimento, dependências e decisões de alto nível, produzindo um documento de arquitetura e ADRs (Architecture Decision Records) que guiam a implementação de forma consistente e sustentável.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `design_funcional` | string (Markdown) | Obrigatório | Design funcional aprovado com User Stories e pontos de integração |
| `requisitos_nao_funcionais` | lista | Obrigatório | Requisitos de performance, disponibilidade, segurança e escalabilidade |
| `contexto_negocio` | string | Obrigatório | Restrições de negócio: orçamento, time-to-market, equipe disponível |
| `stack_existente` | lista | Opcional | Tecnologias e plataformas já em uso na organização |
| `restricoes_tecnicas` | lista | Opcional | Restrições de infraestrutura, compliance ou integração |

## Output

Pacote de arquitetura em Markdown contendo:

- Visão geral da arquitetura (diagrama textual C4 nível 2 e 3)
- Padrões adotados: camadas, módulos, bounded contexts
- Decisões técnicas de alto nível com justificativa
- ADRs para cada decisão arquitetural relevante
- Mapa de dependências e integrações externas
- User Stories enriquecidas com critérios de aceite em Gherkin
- Estratégia de observabilidade: logs, métricas e rastreamento
- Riscos técnicos e plano de mitigação
- Recomendações para a fase de Builder

## Prompt base

```
You are a principal software architect with deep expertise in DDD, Clean Architecture, SOLID principles, Event-Driven Architecture, and Observability.

You will receive:
- Approved Functional Design with User Stories and integration touchpoints
- Non-functional requirements
- Business context and constraints
- Existing technology stack (optional)
- Technical constraints (optional)

Produce an Architecture Package in Markdown with the following sections:
1. Architecture Overview (C4 model: Context and Container diagrams in text/ASCII format)
2. Architectural Patterns (selected patterns with justification: layered, hexagonal, microservices, event-driven, etc.)
3. Bounded Contexts (DDD: identify domain boundaries, aggregates, entities, value objects, and domain events)
4. High-Level Technical Decisions (table: decision | chosen option | rationale | alternatives rejected)
5. ADRs - Architecture Decision Records (one ADR per major decision, format: title | status | context | decision | consequences)
6. Dependency and Integration Map (external systems, APIs, messaging, databases)
7. User Stories with Gherkin Acceptance Criteria (enrich design phase stories with Given/When/Then scenarios)
8. Observability Strategy (logging structure, key metrics to instrument, distributed tracing approach)
9. Technical Risks and Mitigation (risk | likelihood | impact | mitigation)
10. Recommendations for the Builder Phase

Rules:
- Every ADR must follow the standard format: title, status (proposed/accepted/deprecated/superseded), context, decision, consequences.
- Bounded contexts must use the ubiquitous language from the Discovery and Design phases.
- Gherkin scenarios must be concrete and testable, not abstract.
- Observability must be designed from the start, not added as an afterthought.
- SOLID principles must be visibly reflected in the module and layer design.
- Write in the language of the input.

BLOCKING RULES (apply before returning the document):
- BLOCKED if: no ADR exists for any decision that affects more than one bounded context or that will be difficult to reverse (e.g., choice of database engine, messaging system, authentication strategy).
  ❌ "We will use PostgreSQL." — decision made with no ADR, no context, no alternatives.
  ✅ ADR-003 with context (shared database coupling problem), decision (modular monolith vs. microservices), alternatives rejected with reasoning, consequences documented.
- BLOCKED if: any bounded context is named after a technical layer ("DatabaseModule", "ServiceLayer") rather than a domain capability ("BillingContext", "SchedulingContext").
- BLOCKED if: any Gherkin scenario is implementation-level rather than behavior-level.
  ❌ "Given the PaymentService is initialized, When process() is called, Then the result is success."
  ✅ "Given a consultant has 3 unbilled sessions from July, When they generate an invoice, Then a PDF is created and the sessions are marked as billed."
- BLOCKED if: the observability strategy is absent or only states "add logs." A valid minimum defines: (1) log fields structure, (2) at least 3 business metrics with thresholds, (3) tracing strategy at use case boundaries.
- BLOCKED if: any domain layer component has a dependency on an infrastructure component — even labeled "temporary" or "for convenience." Restructure before accepting.
- BLOCKED if: the document prescribes implementation details (specific library names, class names, method signatures). Architecture defines structure and constraints, not code.
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O documento contém todas as 10 seções obrigatórias
- [ ] Pelo menos um diagrama textual C4 (Context ou Container) está presente
- [ ] Cada decisão técnica de alto nível tem justificativa e alternativas rejeitadas documentadas
- [ ] Cada ADR segue o formato padrão completo
- [ ] Os bounded contexts utilizam termos da linguagem ubíqua
- [ ] Todas as User Stories têm pelo menos um cenário Gherkin válido (Given/When/Then)
- [ ] A estratégia de observabilidade define logs estruturados, métricas e rastreamento
- [ ] Os princípios SOLID estão refletidos no design de módulos e camadas

## Boas práticas verificadas

- [ ] **DDD:** bounded contexts identificados, linguagem ubíqua nos aggregates e domain events, separação entre domínio e infraestrutura
- [ ] **Clean Architecture:** dependências apontam para o domínio, não para infraestrutura; camadas bem definidas
- [ ] **SOLID:** responsabilidade única por módulo, interfaces segregadas, dependência de abstrações
- [ ] **Event-Driven:** domain events identificados onde aplicável, desacoplamento via mensageria documentado
- [ ] **Observabilidade:** logs estruturados, métricas de negócio e técnicas, rastreamento distribuído planejados
- [ ] **Governança:** ADRs aprovados e registrados antes de iniciar implementação
