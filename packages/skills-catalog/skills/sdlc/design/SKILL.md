# Skill: Design

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Criar fluxos funcionais, definir a experiência do usuário e organizar User Stories em jornadas coerentes, produzindo um design funcional aprovado que serve de contrato entre produto, design e engenharia.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `relatorio_discovery` | string (Markdown) | Obrigatório | Relatório de discovery com linguagem ubíqua e escopo técnico |
| `personas` | lista de strings | Obrigatório | Descrição das personas ou tipos de usuário identificados |
| `fluxos_priorizados` | lista | Opcional | Fluxos de usuário priorizados para esta iteração |
| `restricoes_ux` | lista | Opcional | Restrições de acessibilidade, plataforma ou experiência conhecidas |
| `design_system` | string | Opcional | Nome ou referência ao design system adotado |

## Output

Design funcional em Markdown contendo:

- Jornadas de usuário por persona (fluxos passo a passo)
- User Stories organizadas por jornada no formato "Como [persona], quero [ação], para [benefício]"
- Critérios de aceite iniciais por User Story (em prosa, sem Gherkin nesta fase)
- Wireframes textuais ou descrição de telas principais
- Decisões de UX documentadas com justificativa
- Glossário de termos funcionais (expandindo a linguagem ubíqua)
- Pontos de integração identificados
- Recomendações para a fase de Arquitetura

## Prompt base

```
You are a senior UX designer and product designer working on the Design phase of a software initiative.

You will receive:
- Discovery Report with ubiquitous language and technical scope
- Persona descriptions
- Prioritized user flows (optional)
- UX constraints (optional)
- Design system reference (optional)

Produce a Functional Design Document in Markdown with the following sections:
1. User Journeys (one subsection per persona, with numbered steps reflecting the ubiquitous language)
2. User Stories (organized by journey, format: "As [persona], I want [action], so that [benefit]")
3. Acceptance Criteria (per user story, written in plain language - Gherkin will be added in Architecture/Builder phases)
4. Screen Descriptions (key screens described textually: purpose, main elements, interactions)
5. UX Decisions Log (decision | rationale | alternatives considered)
6. Extended Ubiquitous Language (new domain terms emerging from the design)
7. Integration Touchpoints (external systems, APIs, or services this design depends on)
8. Recommendations for the Architecture Phase

Rules:
- User stories must use the exact terms from the ubiquitous language glossary.
- Every UX decision must include the rationale and at least one alternative considered.
- Screen descriptions must focus on functionality, not visual aesthetics.
- Write in the language of the input.
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O documento contém todas as 8 seções obrigatórias
- [ ] Cada persona tem pelo menos uma jornada de usuário descrita
- [ ] Todas as User Stories seguem o formato "Como [persona], quero [ação], para [benefício]"
- [ ] Cada User Story tem pelo menos um critério de aceite em prosa
- [ ] As decisões de UX têm justificativa e alternativas registradas
- [ ] Os termos do glossário de linguagem ubíqua são utilizados nas jornadas e User Stories

## Boas práticas verificadas

- [ ] **DDD:** linguagem ubíqua do domínio refletida nas jornadas, User Stories e descrições de tela; novos termos adicionados ao glossário
- [ ] **Governança:** decisões de UX documentadas com justificativa antes de avançar para arquitetura
- [ ] **Documentação Markdown:** seções bem estruturadas, User Stories em listas, decisões em tabela
