# Skill: ADR (Architecture Decision Record)

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Architecture |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Registrar uma decisão de arquitetura relevante no padrão Michael Nygard, documentando o contexto que gerou a decisão, as alternativas consideradas com prós e contras, a decisão tomada, o motivo da escolha e as consequências esperadas.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| decisao | Sim | Descrição da decisão de arquitetura a ser registrada |
| contexto | Sim | Situação, problema ou necessidade que originou a necessidade de decidir |
| alternativas | Sim | Pelo menos duas opções consideradas antes da decisão final |
| restricoes | Não | Restrições técnicas, de negócio, de prazo ou de equipe que influenciaram a escolha |
| numero_sequencial | Sim | Número do ADR no formato ADR-NNN (ex: ADR-001, ADR-002) |
| adrs_relacionados | Não | Números de ADRs anteriores que influenciam ou são influenciados por este |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/adr/ADR-NNN-titulo-da-decisao.md`
- **Campos obrigatórios:** número, título, data, status, contexto, alternativas com prós e contras, decisão, motivo, consequências positivas e negativas

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate an Architecture Decision Record (ADR) following the Michael Nygard format.

Structure:
# ADR-NNN: [Decision Title]

**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Deprecated | Superseded by ADR-NNN

## Context
[The situation that forced this decision. Include technical, business or team constraints.]

## Alternatives Considered
### Option 1: [Name]
- Pros: ...
- Cons: ...

### Option 2: [Name]
- Pros: ...
- Cons: ...

### Option N: [Name] (if applicable)
- Pros: ...
- Cons: ...

## Decision
[The chosen option, stated clearly in one sentence.]

## Rationale
[Why this option was chosen over the others. Reference the constraints that made this option the best fit.]

## Consequences
**Positive:**
- ...

**Negative:**
- ...
- [Include at least one negative consequence — no decision is free of trade-offs]

**Related ADRs:** [List if applicable]

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Pelo menos duas alternativas estão descritas com prós e contras de cada uma
- [ ] A decisão tem consequências positivas E negativas documentadas (toda decisão tem trade-offs)
- [ ] O ADR está numerado sequencialmente e vinculado ao contexto do projeto
- [ ] O status está definido (Proposto, Aceito, Obsoleto ou Substituído por ADR-NNN)
- [ ] O motivo da escolha é específico e referencia as restrições que tornaram essa opção a melhor

## Boas práticas verificadas

- [ ] O contexto descreve o problema de negócio ou técnico, não a solução
- [ ] Prós e contras são concretos e mensuráveis, não opiniões genéricas
- [ ] ADRs anteriores relacionados estão referenciados explicitamente
- [ ] O documento é imutável após aceito: novos entendimentos geram um novo ADR, não editam o existente
- [ ] O título é uma frase nominal que resume a decisão (ex: "Uso de PostgreSQL como banco principal")
