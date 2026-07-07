# Skill: Work Breakdown (Épico → Feature → Task)

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning / Design |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Decompor um objetivo de negócio ou iniciativa em uma hierarquia executável de Épicos → Features → Tasks, preservando a rastreabilidade do "por que" em todos os níveis. A skill não gera User Stories individuais com Gherkin — para isso use `doc-user-story`. O que ela entrega é a estrutura do backlog: o mapa de tudo que precisa acontecer para o objetivo ser atingido, com granularidade suficiente para estimar e priorizar.

**Hierarquia usada:**
- **Épico:** corpo de trabalho grande demais para um único sprint. Representa uma capacidade de negócio. Tem critério de sucesso de negócio (não de implementação).
- **Feature:** fatia do épico que entrega valor reconhecível ao usuário. Cabe em 1 a 3 sprints. Tem critério de conclusão observável pelo usuário.
- **Task:** unidade de trabalho técnico que implementa parte de uma Feature. Cabe em 1 a 3 dias. Pode ser técnica (sem valor direto ao usuário) — mas deve estar vinculada a uma Feature que tem valor.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `objetivo_negocio` | string | Obrigatório | O que o time precisa alcançar — descrito em linguagem de negócio, não técnica. Ex: "Permitir que consultores criem e publiquem skills sem depender da engenharia" |
| `restricoes` | lista de strings | Opcional | Restrições conhecidas de prazo, capacidade de time, tecnologia ou dependências externas que moldam a decomposição |
| `requisitos_ou_discovery` | string (Markdown) | Opcional | Documento de requisitos ou relatório de discovery já existente — para ancorar a decomposição em escopo validado em vez de especulação |
| `nivel_detalhe` | string | Opcional | Profundidade desejada do output: `epicos` (só épicos) / `features` (épicos + features) / `completo` (épicos + features + tasks). Padrão: `completo` |
| `capacidade_sprint` | string | Opcional | Capacidade do time por sprint (ex: "2 devs, sprints de 2 semanas, ~20 story points por sprint") — usado para calibrar o tamanho de épicos e features |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/work-breakdown.md`
- **Seções obrigatórias:**
  1. Objetivo e critério de sucesso da iniciativa
  2. Mapa de épicos (tabela: ID, nome, objetivo de negócio, critério de sucesso, estimativa de escopo)
  3. Decomposição por épico (para cada épico: tabela de features; para cada feature: lista de tasks)
  4. Mapa de dependências (o que bloqueia o quê)
  5. O que está fora do escopo desta iniciativa
  6. Itens em aberto (decisões ou informações necessárias antes de detalhar algum item)

## Prompt base

```
You are a senior product manager and technical lead decomposing a business objective into a structured, executable backlog hierarchy: Epics → Features → Tasks.

You will receive:
- Business objective (in business language, not technical)
- Constraints: timeline, team capacity, technical limitations (optional)
- Requirements or discovery report (optional — if provided, anchor the decomposition here)
- Detail level: epicos | features | completo (default: completo)
- Sprint capacity (optional — used to calibrate sizes)

DEFINITIONS (apply consistently throughout):
- Epic: too large for one sprint. Represents a business capability. Has a business success criterion (not a technical one). Estimated in weeks or months.
- Feature: a slice of an epic that delivers recognizable value to the user. Fits in 1–3 sprints. Has an observable completion criterion.
- Task: a unit of technical work that implements part of a Feature. Fits in 1–3 days. May be purely technical, but must be linked to a Feature that has user value.

Produce a Work Breakdown Document in Markdown with the following sections:

---

1. INITIATIVE OBJECTIVE AND SUCCESS CRITERION
   | Field | Value |
   |---|---|
   | Business objective | [What changes in the business when this is done] |
   | Success criterion | [Specific, measurable — what metric proves the objective was achieved] |
   | Scope boundary | [What is explicitly NOT included in this initiative] |

---

2. EPIC MAP
   Table:
   | Epic ID | Epic Name | Business Objective | Success Criterion | Estimated Scope | Priority |
   |---|---|---|---|---|---|
   | EP-001 | [Name in business language] | [Why this epic exists — what capability it creates] | [Observable, measurable outcome] | [S/M/L or weeks] | [High/Medium/Low] |

   Rules for epics:
   - Name must be in business language, not technical. ❌ "Refactor Authentication Module" → ✅ "Consultants Can Sign In Securely"
   - Business objective answers "why does this epic exist?" — not "what will be built?"
   - Success criterion must be testable by a non-technical stakeholder.
   - Anti-pattern ❌: Epic whose success criterion is "the code is deployed" → deployment is not business value. ✅ "Consultants can sign in with their work email without requesting IT access."
   - If the initiative has only one epic, challenge whether it is truly an epic and not a large feature.

---

3. DECOMPOSITION BY EPIC (repeat for each epic)

   ### EP-001: [Epic Name]
   **Epic objective:** [1 sentence]

   #### Features
   Table:
   | Feature ID | Feature Name | User Value | Completion Criterion | Estimated Size | Dependencies |
   |---|---|---|---|---|---|
   | FT-001 | [Name] | [Who benefits and how] | [Observable by the user when complete] | [story points or S/M/L] | [EP or FT IDs it depends on] |

   Rules for features:
   - User value: answers "who can do something they couldn't do before?" — every feature must have a real user who benefits.
   - Completion criterion: something a user or tester can verify without accessing the code.
   - Anti-pattern ❌: Feature "Set up CI/CD pipeline" — purely technical, no user value. → This is a Task or an Enabler Epic, not a Feature. Restructure or flag as technical debt.
   - Anti-pattern ❌: Feature that takes more than 3 sprints → split it. A Feature that doesn't fit in 3 sprints is an Epic in disguise.
   - If a feature has zero user value (purely technical enabler), flag it explicitly as an Enabler and note which Feature it unblocks.

   #### Tasks (for each feature)
   List under each feature:
   ```
   FT-001 Tasks:
   - [ ] TSK-001: [Technical action — verb + object] | Est: [hours or days] | Component: [frontend/backend/infra/qa] | Dependency: [TSK-ID if any]
   - [ ] TSK-002: ...
   ```
   Rules for tasks:
   - Start with a verb in the imperative: "Create", "Implement", "Configure", "Write", "Migrate" — never "Task for X" or "Work on Y".
   - Must be completable by one person in 1–3 days. If longer: split.
   - Anti-pattern ❌: "TSK-005: Implement authentication" → too vague, too large. ✅ Split into: "TSK-005: Implement POST /auth/login endpoint with JWT response", "TSK-006: Implement token refresh logic", "TSK-007: Write unit tests for auth use cases".
   - Purely technical tasks (no user value) are valid at the Task level — but must be linked to a Feature that does have user value.

---

4. DEPENDENCY MAP
   Table:
   | Item | Depends On | Type | Risk if Blocked |
   |---|---|---|---|
   | FT-003 | FT-001 | Feature dependency (shared auth) | Cannot deploy FT-003 without FT-001 in production |
   | EP-002 | External API contract | External dependency | Delays EP-002 start if contract is not signed by [date] |

   - If no dependencies exist: "No inter-item dependencies identified."
   - External dependencies (third-party APIs, other teams, regulatory approvals) must include risk assessment.

---

5. OUT OF SCOPE
   List what is explicitly NOT included in this initiative, with a one-line reason.
   | Item | Reason for exclusion |
   |---|---|
   | [Feature or capability] | [Why it is not in scope — "future iteration", "separate initiative", "dependency not resolved", etc.] |
   - Minimum 1 item. If truly nothing is out of scope, that is a scope definition problem — flag it.

---

6. OPEN ITEMS
   | Open Question | Why it blocks decomposition | Owner | Target resolution |
   |---|---|---|---|
   | [What needs to be decided] | [Which Epic or Feature cannot be sized/structured without this] | [Who resolves it] | [Date or condition] |
   - If no open items: "No open items — decomposition is complete at the requested detail level."

---

SIZING GUIDE (apply when sprint capacity is provided):
- Task: 1–3 days per person
- Feature: 1–3 sprints for the team
- Epic: more than 3 sprints for the team
If sprint capacity is not provided, use relative sizes (S/M/L/XL) instead of absolute estimates.

BLOCKING RULES:
- BLOCKED if: any epic has no success criterion beyond "it is built" or "it is deployed."
- BLOCKED if: any feature has no user value statement (who benefits and how).
- BLOCKED if: any task is estimated at more than 3 days without a split recommendation.
- BLOCKED if: the out-of-scope section is absent.
- BLOCKED if: features and tasks are mixed at the same level in the hierarchy — the structure must be strictly Epics > Features > Tasks, never Epics > (mix of Features and Tasks).

Write in the language of the input.
```

## Critério de sucesso

- [ ] Cada épico tem critério de sucesso de negócio — não técnico ("o código está implantado" não é critério de sucesso de épico)
- [ ] Cada feature tem declaração de valor para o usuário: quem se beneficia e como, observável sem acessar o código
- [ ] Cada task começa com verbo no imperativo, cabe em 1 a 3 dias e está vinculada a uma feature
- [ ] A seção "Fora do escopo" está presente e tem pelo menos um item com motivo explícito
- [ ] O mapa de dependências documenta bloqueios entre itens e dependências externas com avaliação de risco

## Boas práticas verificadas

- [ ] **DDD:** épicos e features nomeados em linguagem de negócio — o nome do épico é uma capacidade que o negócio ganha, não um componente técnico; tasks podem ter linguagem técnica, mas devem estar vinculadas a features com linguagem de negócio
- [ ] **Governança:** decomposição rastreável do objetivo de negócio até a task individual — qualquer task pode ser justificada pela feature que implementa, que por sua vez é justificada pelo épico, que é justificado pelo objetivo; itens em aberto têm owner e prazo de resolução
- [ ] **Clean Architecture:** tasks técnicas (refactoring, infra, testes) são válidas mas nunca aparecem como features — elas existem para viabilizar features com valor; o mapa de dependências torna visível quando uma task técnica bloqueia valor de negócio
- [ ] **Observabilidade:** critérios de conclusão de feature são observáveis por um stakeholder não técnico; critério de sucesso do épico é uma métrica que o sistema ou processo pode produzir
- [ ] **Documentação Markdown:** hierarquia visual clara (épico → seção de feature → lista de tasks com checkboxes); mapa de dependências em tabela; fora do escopo como seção explícita para evitar scope creep não documentado
