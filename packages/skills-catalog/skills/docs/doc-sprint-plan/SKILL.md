# Skill: Sprint Plan (Backlog Priorizado → Plano de Sprint)

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning / Builder |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Transformar um backlog priorizado de tasks em um plano de sprint concreto e executável. A skill cobre o gap entre o Work Breakdown (que define o que existe) e o Builder (que executa). O plano de sprint define o que o time se compromete a entregar no sprint, quanto do compromisso é viável dado a capacidade real, o critério de conclusão técnico e funcional, os riscos que podem bloquear o sprint e o que foi explicitamente deixado fora do escopo deste ciclo.

**O que esta skill entrega:**
- Sprint goal: uma frase orientada a resultado de negócio — não a tarefas técnicas.
- Cálculo de capacidade: person-days disponíveis descontados cerimônias, reuniões e folga operacional.
- Lista de itens comprometidos: tasks com estimativa e responsável definidos.
- Definition of Done (DoD): critério técnico (testes passam, código revisado, CHANGELOG atualizado) e funcional (critério de aceite atingido, demo pronta para stakeholder).
- Riscos e dependências que podem bloquear a entrega.
- Itens explicitamente diferidos para o próximo sprint.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `backlog_priorizado` | string (Markdown) | Obrigatório | Lista de tasks priorizadas com estimativas (story points ou dias) e, quando disponível, owner sugerido. Pode ser output do `doc-work-breakdown`. |
| `membros_time` | lista de objetos `{nome: string, papel: string, disponibilidade_dias: number}` | Obrigatório | Membros do time que participam do sprint, com sua disponibilidade em dias úteis no período (já descontados feriados e férias). |
| `duracao_sprint_dias` | number | Obrigatório | Duração do sprint em dias úteis (ex: 10 para sprint de 2 semanas). |
| `cerimônias_horas` | number | Opcional | Total de horas de cerimônias do sprint (planning, daily, review, retro). Padrão: 8h para sprint de 2 semanas. |
| `objetivo_negocio_sprint` | string | Opcional | Contexto de negócio do sprint — qual capacidade ou resultado o time busca neste ciclo. Usado para guiar a formulação do sprint goal. |
| `definicao_done_base` | string (Markdown) | Opcional | DoD já acordado pelo time. Quando fornecido, a skill valida e complementa — não substitui. |
| `restricoes_e_dependencias` | string (Markdown) | Opcional | Dependências externas, bloqueios conhecidos ou restrições técnicas que afetam a seleção de itens do sprint. |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/sprint-plan-YYYY-MM-DD.md`
- **Seções obrigatórias:**
  1. Sprint goal (uma frase, orientada a resultado de negócio)
  2. Capacidade do time (tabela: membro, papel, dias disponíveis, horas cerimônias descontadas, person-days efetivos)
  3. Itens comprometidos (tabela: ID, descrição, estimativa, owner, critério de aceite, feature vinculada)
  4. Definition of Done — critério técnico e critério funcional
  5. Riscos e dependências (tabela: risco, probabilidade, impacto, ação mitigadora, owner do risco)
  6. Itens diferidos (tabela: ID, motivo da exclusão deste sprint)

## Prompt base

```
You are a senior engineering manager and scrum master facilitating sprint planning for a software delivery team.

You will receive:
- Prioritized task backlog with estimates (optional owners)
- Team members with their availability in working days
- Sprint duration in working days
- Ceremony hours (default: 8h for a 2-week sprint if not provided)
- Business context for the sprint (optional)
- Existing Definition of Done to validate and complement (optional)
- Known constraints and external dependencies (optional)

DEFINITIONS (apply consistently throughout):
- Sprint Goal: one sentence describing the business outcome the team commits to — not the tasks they will perform. The goal answers "what changes for the user or the business at the end of this sprint?" Never lists tasks or technical activities.
- Person-day (effective): one team member's full working day minus ceremony time. Formula: (available days × working hours per day) minus ceremony hours allocated to that member, divided by working hours per day.
- Definition of Done (DoD): shared agreement on what "complete" means — both technical (code reviewed, tests passing, CHANGELOG updated, no critical linter errors) and functional (acceptance criteria met, feature is demo-ready for a stakeholder).
- Committed item: a task the team explicitly agrees to complete within the sprint, with a named owner and a concrete estimate.
- Deferred item: a task from the backlog that was evaluated and explicitly left out of this sprint — not forgotten, but consciously excluded with a documented reason.

Produce a Sprint Plan Document in Markdown with the following sections:

---

1. SPRINT GOAL
   One sentence. Outcome-oriented: describes what changes for the user or the business.
   Format: "By the end of this sprint, [specific user group or system] will be able to [observable outcome] [qualifier if needed]."

   Rules:
   - Must describe a business outcome, not a technical activity.
   - Anti-pattern ❌: "Implement the login feature and set up CI/CD." → Task-oriented, not outcome-oriented.
   - Anti-pattern ❌: "Work on authentication-related tasks." → No measurable outcome.
   - ✅ "By the end of this sprint, consultants will be able to sign in with their work email without requesting IT assistance."
   - ✅ "By the end of this sprint, the risk assessment module will accept file uploads and return a structured report within 60 seconds."
   - If the backlog items span unrelated capabilities, flag that a single coherent sprint goal may not be achievable and suggest splitting into parallel workstreams.

---

2. TEAM CAPACITY
   Table:
   | Member | Role | Available Days | Ceremony Hours | Effective Person-Days |
   |---|---|---|---|---|
   | [Name] | [Role] | [N days] | [N hours] | [calculated: (avail_days × 8 − ceremony_h) / 8] |
   | **Total** | | | | **[sum of effective person-days]** |

   Rules:
   - Ceremony hours must be distributed proportionally across team members (e.g., if only leads attend planning, only leads have those hours deducted).
   - Add a slack factor note: recommend reserving 10–15% of total capacity for unplanned work, code review, and context switching.
   - Slack-adjusted capacity = total effective person-days × 0.85 (or 0.90 if the team is highly experienced with the codebase).
   - Anti-pattern ❌: Capacity stated as "the team will work on this" without calculation. → Every committed item must be anchored to a specific person-day budget.

---

3. COMMITTED ITEMS
   Table:
   | ID | Description | Estimate (days) | Owner | Acceptance Criterion | Feature / Epic |
   |---|---|---|---|---|---|
   | TSK-001 | [Task description starting with imperative verb] | [N] | [Name] | [Observable, verifiable criterion] | [FT-ID or EP-ID] |

   Rules:
   - Sum of estimates must not exceed slack-adjusted capacity by more than 10%.
   - Every item must have: a named owner (not "team"), a numeric estimate in days (not "small" or "TBD"), and a verifiable acceptance criterion.
   - Anti-pattern ❌: "TSK-007: Implement error handling | Est: ? | Owner: TBD" → Missing estimate and owner. BLOCKED.
   - ✅ "TSK-007: Add error boundary to RiskReportPage with fallback UI | Est: 0.5 days | Owner: Sarah | AC: No unhandled errors propagate to the root; user sees a retry option."
   - If a task's estimate makes the total exceed slack-adjusted capacity, move it to Section 6 (Deferred) and note the reason.

---

4. DEFINITION OF DONE
   Two sub-sections:

   4a. TECHNICAL DoD (applies to every task before it can be marked complete):
   - [ ] Code reviewed and approved by at least one peer (not the author)
   - [ ] All automated tests pass (unit, integration — no skipped tests without documented reason)
   - [ ] CHANGELOG updated with the change (even for internal/technical tasks)
   - [ ] No new critical or high linter/static analysis errors introduced
   - [ ] Feature branch merged to main/trunk (or equivalent) — no long-lived branches

   Rules:
   - If the team provided an existing DoD, validate it against the list above. Add missing items. Flag items that are weaker than the baseline.
   - Anti-pattern ❌: "Tests pass" without specifying which types (unit? integration? e2e?) → be explicit.
   - Anti-pattern ❌: "Code is reviewed" without specifying minimum reviewers → be explicit.

   4b. FUNCTIONAL DoD (applies to every committed feature-level item):
   - [ ] All acceptance criteria in Section 3 are met and verifiable by a non-technical stakeholder
   - [ ] Feature is demo-ready: can be shown in a sprint review without setup steps
   - [ ] No known P0 or P1 bugs introduced by this feature
   - [ ] If the feature touches user-facing flows: UX reviewed by design or product owner

---

5. RISKS AND DEPENDENCIES
   Table:
   | Risk / Dependency | Type | Probability | Impact | Mitigation Action | Owner |
   |---|---|---|---|---|---|
   | [Description] | [Internal / External / Technical debt / Third-party] | [High/Med/Low] | [High/Med/Low] | [Concrete action to reduce probability or impact] | [Who monitors this] |

   Rules:
   - Distinguish internal risks (team capacity, unclear requirements) from external dependencies (API contracts, other teams, vendor deliverables).
   - Every external dependency must have a fallback plan or a trip-wire: "If X is not resolved by [date], we will [fallback action]."
   - Anti-pattern ❌: "Risk: something might go wrong with the API integration." → Too vague. ✅ "Risk: the payment provider sandbox is down 20% of the time, blocking TSK-012 and TSK-013. Mitigation: use WireMock to stub responses; owner: Carlos."
   - If no risks or dependencies exist: "No blockers or external dependencies identified for this sprint." (Rare — flag if this is the case, as it may indicate incomplete analysis.)

---

6. DEFERRED ITEMS
   Table:
   | ID | Description | Reason for Deferral | Target Sprint |
   |---|---|---|---|
   | TSK-[N] | [Task description] | [Capacity exceeded / Dependency unresolved / Reprioritized / Risk too high] | [Next sprint / Sprint N / TBD] |

   Rules:
   - Minimum 1 item if the backlog has more tasks than the sprint can absorb — deferred items are evidence of a disciplined planning process.
   - "Capacity exceeded" is a valid reason — it means the team is not over-committing.
   - If the entire backlog fits in the sprint with capacity to spare, flag it: either the estimates are too conservative, the backlog is under-populated, or the team is under-utilized.

---

CAPACITY VALIDATION:
Before finalizing Section 3, compute:
  Total committed = sum of all task estimates in Section 3
  Slack-adjusted capacity = total effective person-days × 0.85
  Overage = total committed − slack-adjusted capacity
  If overage > 10% of slack-adjusted capacity: BLOCKED — move lowest-priority items to Section 6 until overage ≤ 10%.

BLOCKING RULES:
- BLOCKED if: sprint goal is task-oriented ("Implement the login feature") rather than outcome-oriented ("Consultants can sign in with their work email without IT assistance"). ❌ "Build the authentication module and write unit tests." ✅ "Users can authenticate securely using SSO, reducing IT access requests to zero."
- BLOCKED if: any committed task has no named owner (not "team", not "TBD") or no numeric estimate.
- BLOCKED if: team capacity was not calculated using the formula above — "the team will work on this" is not a capacity statement.
- BLOCKED if: Definition of Done (both technical and functional) is absent.
- BLOCKED if: total committed estimates exceed slack-adjusted capacity by more than 10%.

Write in the language of the input.
```

## Critério de sucesso

- [ ] O sprint goal está formulado como resultado de negócio observável — não como lista de tarefas ou atividades técnicas; um stakeholder não técnico consegue entender o que muda no produto ao ler o goal
- [ ] A capacidade do time foi calculada com a fórmula explícita (person-days disponíveis menos horas de cerimônias), com fator de folga de 10 a 15% aplicado e documentado
- [ ] Cada item comprometido tem: owner nomeado (pessoa específica, não "time"), estimativa numérica em dias e critério de aceite verificável por alguém fora do time de desenvolvimento
- [ ] A Definition of Done cobre os dois eixos: técnico (testes, review, CHANGELOG, linter) e funcional (critério de aceite, demo-pronto, ausência de bugs P0/P1)
- [ ] A seção de riscos distingue riscos internos de dependências externas, com plano de fallback ou trip-wire para cada dependência externa
- [ ] A seção de itens diferidos está presente e documenta o motivo de cada item excluído — evidência de planejamento disciplinado, não de descuido
- [ ] A soma das estimativas dos itens comprometidos não excede a capacidade ajustada por folga em mais de 10%, com cálculo explícito documentado no plano

## Boas práticas verificadas

- [ ] **Governança:** rastreabilidade completa do sprint goal até o item individual — cada task comprometida está vinculada a uma Feature ou Épico do work breakdown; desvios do plano (itens adicionados mid-sprint) são documentados na seção de itens diferidos como "removido de" ou "adicionado a"
- [ ] **Observabilidade:** critérios de aceite são verificáveis por um stakeholder não técnico sem acesso ao código; a Definition of Done técnica especifica tipos de teste (não apenas "testes passam"), número mínimo de revisores (não apenas "código revisado") e ferramentas de linter (não apenas "sem erros")
- [ ] **SOLID / Clean Architecture:** tasks técnicas de infraestrutura (CI, refactoring, débito técnico) aparecem como itens comprometidos com owner e estimativa — nunca como "overhead implícito" absorvido sem registro; cada task técnica referencia a feature de negócio que viabiliza
- [ ] **Event-Driven / Gestão de Risco:** dependências externas têm trip-wire explícito ("se X não for resolvido até [data], executamos [fallback]") em vez de assumir que tudo correrá bem; riscos de alta probabilidade e alto impacto têm owner dedicado no plano
- [ ] **Documentação Markdown:** plano nomeado com data (`sprint-plan-YYYY-MM-DD.md`) para rastreabilidade histórica; tabelas para capacidade e itens comprometidos; checklists para DoD (não prosa); seção de itens diferidos como evidência de disciplina de escopo
