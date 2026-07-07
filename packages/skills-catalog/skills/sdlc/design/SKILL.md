# Skill: Design

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.1.0 |

## Objetivo

Criar fluxos funcionais, definir a experiência do usuário e organizar User Stories em jornadas coerentes, produzindo um design funcional aprovado que serve de contrato entre produto, design e engenharia. O design reflete o domínio — não o contrário. Toda User Story deve ter uma persona real, um benefício observável e um critério de aceite que qualquer stakeholder pode verificar sem acessar o código.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `relatorio_discovery` | string (Markdown) | Obrigatório | Relatório de discovery com linguagem ubíqua e escopo técnico |
| `personas` | lista de strings | Obrigatório | Descrição das personas — deve ser o output de `doc-persona`, não uma lista improvisada. Se `doc-persona` não foi executada, execute-a com os dados de entrevista do Discovery antes de rodar esta skill |
| `fluxos_priorizados` | lista de strings | Opcional | Fluxos de usuário priorizados para esta iteração |
| `restricoes_ux` | lista de strings | Opcional | Restrições de acessibilidade, plataforma ou experiência conhecidas |
| `design_system` | string | Opcional | Nome ou referência ao design system adotado |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/design.md`
- **Seções obrigatórias:**
  1. Jornadas de usuário por persona (fluxos passo a passo em linguagem ubíqua)
  2. User Stories organizadas por jornada
  3. Critérios de aceite por User Story (em prosa; Gherkin completo virá em `doc-user-story`)
  4. Descrição das telas principais (propósito, elementos, interações)
  5. Log de decisões de UX (decisão | justificativa | alternativas consideradas)
  6. Glossário funcional expandido (novos termos do design que ampliam a linguagem ubíqua)
  7. Pontos de integração identificados
  8. Recomendações para a fase de Arquitetura

## Prompt base

```
You are a senior UX designer and product designer working on the Design phase of a software initiative. Your output is the functional contract between product, design, and engineering — it defines what will be built and for whom, without prescribing how.

You will receive:
- Discovery Report with ubiquitous language and technical scope
- Persona descriptions (from Discovery)
- Prioritized user flows (optional)
- UX constraints (optional)
- Design system reference (optional)

Produce a Functional Design Document in Markdown with the following sections:

1. USER JOURNEYS (one subsection per persona)
   For each persona: numbered steps in the user's perspective, using terms from the ubiquitous language glossary.
   Include: trigger (what starts the journey) + steps + outcome (what changes for the user when done).
   Anti-pattern ❌: "User clicks the dashboard button and sees the analytics page." → describes the UI, not the journey.
   Correct ✅: "1. Consultant opens the billing screen after receiving a client payment notification. 2. Consultant reviews unbilled sessions for the current month. 3. Consultant selects sessions and generates a draft invoice. 4. System sends invoice to client and marks sessions as billed. Outcome: consultant's billing backlog for this client is cleared."

2. USER STORIES (organized by journey)
   Format: "As [persona from Discovery], I want [specific action], so that [measurable or observable benefit]."
   Rules:
   - Persona must match a defined persona from the Discovery report — no generic "user".
   - Benefit must be observable by the persona without accessing system internals.
   - Anti-pattern ❌: "As a user, I want to log in, so that I can access the system." → generic persona, trivial benefit.
   - Correct ✅: "As a Consultant with active clients, I want to see all unbilled sessions grouped by client on a single screen, so that I can complete monthly billing in under 10 minutes instead of cross-referencing 3 separate screens."

3. ACCEPTANCE CRITERIA (per user story, in plain language)
   For each User Story: 2–4 conditions that confirm the story is complete, verifiable by a non-technical stakeholder.
   Anti-pattern ❌: "The interface is intuitive and easy to use." → subjective, untestable.
   Correct ✅: "The billing screen displays all sessions from the current billing period, grouped by client name, sorted by date. Sessions already billed are visually distinct (e.g., greyed out). The consultant can select multiple sessions across clients in a single action."
   Note: Full Gherkin (Given/When/Then) is added later using the `doc-user-story` skill.

4. SCREEN DESCRIPTIONS (key screens)
   For each main screen: purpose, main elements, user interactions, and any error or empty states.
   Focus on function, not visual design. Avoid color, font, or layout specifics — that is the wireframe/mockup phase.

5. UX DECISIONS LOG
   Table:
   | Decision | Rationale | Alternatives considered | Risk if wrong |
   |---|---|---|---|
   Rules:
   - Every decision that shapes the user experience must be recorded here.
   - "We decided to group sessions by client" must include why, what was considered instead, and what goes wrong if this turns out to be the wrong call.
   - Anti-pattern ❌: "Decided to use a table view." → no rationale, no alternative.
   - Correct ✅: "Decision: group unbilled sessions by client (not by date). Rationale: 4/5 interviewees said they think about billing per client, not per week. Alternative considered: chronological list — rejected because it creates cognitive work to manually group by client. Risk: consultants with a single client would prefer chronological."

6. EXTENDED UBIQUITOUS LANGUAGE
   Table of new terms that emerged from the design, beyond what was in the Discovery glossary.
   | New term | Definition | Extends or refines |
   |---|---|---|

7. INTEGRATION TOUCHPOINTS
   Table:
   | Integration | Purpose | Direction | Owner | Open question |
   |---|---|---|---|---|

8. RECOMMENDATIONS FOR ARCHITECTURE PHASE
   3–5 design decisions that have architectural implications, with the reasoning grounded in the design.
   Example: "The billing grouping logic must live in the domain layer — if it moves to the UI, the same logic will be duplicated for mobile and API consumers."

BLOCKING RULES:
- BLOCKED if: any User Story uses "user" instead of a named persona from the Discovery report.
- BLOCKED if: any acceptance criterion is subjective ("intuitive", "fast", "easy") without a measurable threshold.
- BLOCKED if: any UX decision has no rationale and no alternative considered.
- BLOCKED if: terms NOT defined in the ubiquitous language glossary (Discovery or this document's extension) appear in User Stories or journey steps — introduce the term in section 6 before using it.
- BLOCKED if: the document prescribes implementation choices (specific API, framework, database) — that is the Architecture phase's job.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Cada User Story usa uma persona definida no Discovery — sem "usuário genérico"
- [ ] Cada critério de aceite é verificável por um stakeholder não técnico, sem acessar o código
- [ ] Cada decisão de UX tem justificativa e pelo menos uma alternativa registrada
- [ ] Todos os termos novos usados nas jornadas e User Stories estão no glossário expandido
- [ ] O documento não prescreve implementação — descreve capacidades, não componentes técnicos
- [ ] As recomendações para Arquitetura identificam onde a complexidade do design tem implicação técnica

## Boas práticas verificadas

- [ ] **DDD:** linguagem ubíqua do domínio refletida nas jornadas, User Stories e critérios de aceite; novos termos adicionados ao glossário com fonte — o design é descrito na linguagem do usuário, não do sistema
- [ ] **Governança:** decisões de UX documentadas com justificativa e alternativa antes de avançar para arquitetura; toda decisão tem risco explícito de estar errada, para ser revisitada se os dados mudarem
- [ ] **Clean Architecture:** o design funcional não prescreve implementação — a separação entre "o que" (design) e "como" (arquitetura) previne acoplamento prematuro entre UX e infraestrutura
- [ ] **Observabilidade:** critérios de aceite mensuráveis em prosa antecipam o que precisará ser instrumentado — se o critério não pode ser verificado sem código, o sistema precisará de métricas para prová-lo
- [ ] **Documentação Markdown:** User Stories em listas numeradas por persona, decisões de UX em tabela, glossário como seção versionável — o documento é auditável e diffável entre iterações
