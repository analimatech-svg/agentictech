# Skill: Roadmap

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar um roadmap de produto ou projeto organizado nos três horizontes Now / Next / Later, comunicando o que está em execução, o que vem a seguir e a direção de longo prazo — sem comprometer datas fixas onde o futuro ainda é incerto. Um roadmap com datas no horizonte Later ou com itens sem responsável é um Gantt chart disfarçado e deve ser bloqueado.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `business_case` | string (Markdown) | Obrigatório | Business case aprovado ou documento de visão com o problema, objetivo e critério de sucesso do projeto |
| `itens_priorizados` | lista de strings | Obrigatório | Funcionalidades, épicos ou entregas priorizadas — cada item deve ter um responsável identificado (squad, pessoa ou papel) |
| `restricoes_prazo` | lista de strings | Opcional | Datas fixas inamovíveis: lançamentos contratuais, eventos do setor, janelas regulatórias. Itens sem restrição real não devem ser listados aqui |
| `criterios_priorizacao` | string | Opcional | Critério usado para ordenar os itens (ex: RICE, MoSCoW, valor de negócio vs. esforço, dependência técnica) |
| `horizonte_now` | string | Opcional | Prazo do horizonte Now (padrão: sprint atual ou até 4 semanas) |
| `horizonte_next` | string | Opcional | Prazo do horizonte Next (padrão: 1 a 3 meses após Now) |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/roadmap.md`
- **Seções obrigatórias:**
  1. Objetivo do roadmap (problema que está sendo resolvido e critério de sucesso, em 2 linhas)
  2. Horizonte Now — em execução (itens com responsável, critério de conclusão e status)
  3. Horizonte Next — próximo ciclo (itens com responsável e critério de conclusão; sem data exata)
  4. Horizonte Later — direção (intenções de longo prazo, intencionalmente vagas, sem datas fixas e sem escopo detalhado)
  5. Dependências identificadas (o que bloquearia o avanço entre horizontes)
  6. Fora do escopo (itens que não entram neste roadmap e o motivo)

## Prompt base

```
You are a senior product manager building a roadmap that communicates direction without over-committing to timelines that are still uncertain.

You will receive:
- Approved business case or vision document
- Prioritized items with responsible owners
- Fixed-date constraints (optional)
- Prioritization criteria (optional)
- Now and Next horizon definitions (optional, defaults: Now = current sprint or next 4 weeks; Next = 1-3 months after Now)

Produce a Roadmap Document in Markdown with the following sections:

1. ROADMAP OBJECTIVE (2 lines: what problem this roadmap solves and the one metric that proves the roadmap is working)

2. NOW — IN PROGRESS
   Table: Item | Responsible (squad, person, or role) | Completion Criterion | Current Status
   - Completion criterion must be observable: "feature X is in production and passing smoke tests" — never "feature X is done"
   - Status: On Track / At Risk / Blocked (if At Risk or Blocked, add one line explaining why and what is being done)
   - Items in Now must be actively in progress, not planned. Do not put items here that have not started.

3. NEXT — COMING CYCLE
   Table: Item | Responsible | Completion Criterion | Dependencies (on Now items or external)
   - No fixed dates. Next items have enough detail to plan, but scope is still negotiable.
   - Every item must have a completion criterion — not "we will work on X" but "X is complete when Y is observable"
   - If an item depends on a Now item completing first, list that dependency explicitly.

4. LATER — DIRECTION
   Bullet list of strategic intentions.
   - Intentionally vague: describe the direction, not the solution.
   - No fixed dates. No detailed scope. No committed delivery.
   - Anti-pattern: ❌ "Implement multi-tenant architecture by Q4 2027" → ✅ "Evolve toward multi-tenancy when the customer base justifies the investment"
   - Anti-pattern: ❌ "Build analytics dashboard with 15 KPIs" → ✅ "Invest in self-service analytics to reduce dependency on manual reports"

5. IDENTIFIED DEPENDENCIES
   Table: Item | Depends On | Type (internal / external / regulatory) | Risk if dependency is not resolved
   - List only real blockers. If no external dependencies exist, write "No external dependencies identified at this time."

6. OUT OF SCOPE
   List what is explicitly NOT in this roadmap, with a one-line reason for each.
   - "Out of scope" items are promises to stakeholders that these things will not appear as surprises later.

Rules:
- BLOCKED if: any Later item has a fixed date (quarter, month, or year). Later items must describe direction, not commitment.
- BLOCKED if: any item in Now or Next has no responsible owner (squad, person, or role). Anonymous items cannot be tracked.
- BLOCKED if: any item in Now or Next has no completion criterion. "Done" is not a criterion.
- Do not turn a roadmap into a Gantt chart. If the requester insists on specific dates for every item, document that as a scope concern and flag it — a roadmap with all items on fixed dates is a project plan, not a roadmap.
- The "Out of scope" section is mandatory, even if it contains only one item. It protects the team from scope creep conversations.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Cada item do horizonte Now e Next tem responsável identificado (squad, pessoa ou papel) e critério de conclusão observável
- [ ] O horizonte Later contém apenas intenções de direção, sem datas fixas e sem escopo detalhado comprometido
- [ ] A seção "Fora do escopo" está presente e tem pelo menos um item com motivo explícito
- [ ] Dependências entre itens estão mapeadas, com tipo e risco identificados
- [ ] O objetivo do roadmap está vinculado ao problema e critério de sucesso do business case ou documento de visão

## Boas práticas verificadas

- [ ] **Governança:** roadmap reflete decisões validadas com stakeholders; itens sem responsável não entram no documento; dependências externas têm risco de não resolução documentado
- [ ] **DDD:** itens nomeados em linguagem de negócio, não em termos técnicos internos (ex: "Reduzir tempo de onboarding", não "Refatorar módulo user-service"); Later descreve impacto no domínio, não escolha de tecnologia
- [ ] **Observabilidade:** critérios de conclusão em Now e Next são mensuráveis e vinculados a métricas observáveis, não ao julgamento de "pronto" do time
- [ ] **Documentação Markdown:** tabelas para Now e Next (legíveis por stakeholders não técnicos); bullets para Later (deliberadamente menos estruturado para comunicar incerteza intencional)
- [ ] **Horizonte Later como sinal de maturidade:** o que está no Later reflete decisões ainda não maduras o suficiente para comprometer — não itens esquecidos. Se um stakeholder perguntar "por que X está no Later?", deve haver uma resposta clara
