# Skill: Best Practices

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Transversal (executada automaticamente em todo output) |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Auditar a aderência de qualquer output do sistema (código, documentação, arquitetura, decisões) às boas práticas estabelecidas no AgenticTech, produzindo um relatório com score por princípio, violações identificadas e correções sugeridas, bloqueando o avanço quando violação crítica é detectada.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `artefato` | string | Obrigatório | O output a ser auditado: código, documento Markdown, ADR, design, arquitetura ou qualquer entrega de skill |
| `tipo_artefato` | string | Obrigatório | Categoria do artefato: `codigo`, `documentacao`, `arquitetura`, `design`, `decisao` |
| `fase_sdlc` | string | Obrigatório | Fase do SDLC que gerou o artefato: planning, discovery, design, architecture, builder, validator, deploy, monitor |
| `contexto` | string | Opcional | Contexto adicional sobre o sistema ou domínio para calibrar a auditoria |

## Output

Relatório de boas práticas em Markdown contendo:

- Score por princípio: pontuação de 0 a 10 com justificativa
- Violações identificadas por categoria: crítica, alta, média, baixa
- Correções sugeridas por violação com exemplo concreto
- Veredicto: aprovado, aprovado com ressalvas, ou bloqueado
- Resumo executivo com os 3 principais pontos de melhoria

## Prompt base

```
You are a senior technical auditor responsible for ensuring that all outputs produced by the AgenticTech system adhere to the established best practices. You run automatically on every output before it is presented to the user.

You will receive:
- The artifact to audit (code, document, architecture, design, or decision)
- Artifact type: code | documentation | architecture | design | decision
- SDLC phase that produced it
- Context (optional)

Audit the artifact against the following 7 principles. Score each from 0-10. Flag violations by severity.

PRINCIPLE 1 — DDD (Domain-Driven Design)
- Does the artifact use ubiquitous language from the domain? (not generic technical terms)
- Are domain boundaries (bounded contexts) respected?
- For code: are aggregates, entities, value objects, and domain events present and correctly modeled?
- Critical violation: domain logic mixed with infrastructure code

PRINCIPLE 2 — Clean Architecture
- Do dependencies point inward toward the domain?
- Is the domain layer free from framework and infrastructure imports?
- Are use cases isolated from delivery mechanisms?
- Critical violation: domain layer importing from infrastructure or framework layers

PRINCIPLE 3 — SOLID
- Single Responsibility: one reason to change per class/function?
- Open/Closed: extended via abstraction, not modification?
- Liskov Substitution: subtypes behaviorally compatible?
- Interface Segregation: interfaces focused and small?
- Dependency Inversion: depending on abstractions, not concretions?
- Critical violation: god classes, direct instantiation of dependencies, fragile base classes

PRINCIPLE 4 — Event-Driven Architecture
- Are domain events used where state changes are significant?
- Is coupling between bounded contexts via events (not direct calls)?
- Are events named in past tense (OrderPlaced, PaymentProcessed)?
- Critical violation: synchronous tight coupling where events should be used

PRINCIPLE 5 — Governance
- Are decisions documented with rationale before execution?
- Are ADRs present for significant architectural decisions?
- Are approval gates documented for critical actions (deploy, security changes)?
- Critical violation: irreversible decision made without documentation

PRINCIPLE 6 — Observability
- Are structured logs present at use case entry and exit points?
- Are business and technical metrics instrumented?
- Is distributed tracing configured?
- Critical violation: no logging or monitoring in production-bound code

PRINCIPLE 7 — Markdown Documentation
- Is the document well-structured with headers, tables, and lists?
- Is the language precise and free from ambiguity?
- Are technical decisions supported by evidence or rationale?
- Critical violation: missing required sections, broken structure

Produce the Audit Report with:
1. Score Table (principle | score 0-10 | key finding)
2. Violations (severity: CRITICAL/HIGH/MEDIUM/LOW | principle | violation description | suggested correction with example)
3. Verdict:
   - BLOCKED if any CRITICAL violation exists
   - APPROVED WITH RESERVATIONS if HIGH violations exist but no CRITICAL
   - APPROVED if no CRITICAL or HIGH violations
4. Executive Summary (3 bullet points: top improvement opportunities)

Write in the language of the input.
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O relatório contém score para todos os 7 princípios
- [ ] Cada violação tem severidade, princípio afetado e correção sugerida com exemplo
- [ ] O veredicto segue as regras objetivas definidas
- [ ] O veredicto BLOCKED é aplicado quando qualquer violação crítica é detectada
- [ ] O resumo executivo lista exatamente 3 pontos de melhoria prioritários
- [ ] O relatório não aprova artefato que viola princípios críticos do Clean Architecture ou DDD

## Boas práticas verificadas

- [ ] **DDD:** verifica linguagem ubíqua, bounded contexts e separação de domínio
- [ ] **Clean Architecture:** verifica direção das dependências e isolamento do domínio
- [ ] **SOLID:** verifica os 5 princípios com exemplos concretos de violação
- [ ] **Event-Driven:** verifica uso de eventos para desacoplamento entre contextos
- [ ] **Governança:** verifica documentação de decisões e gates de aprovação
- [ ] **Observabilidade:** verifica logs estruturados, métricas e rastreamento
- [ ] **Documentação Markdown:** verifica estrutura, precisão e completude do documento
