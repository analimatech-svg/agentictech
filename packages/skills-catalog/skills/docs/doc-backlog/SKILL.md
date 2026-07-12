# Skill: Backlog Estruturado

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning / Design |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Transformar uma demanda de negócio em um backlog operacional completo e sprint-ready, seguindo a hierarquia Épico → Feature → Enabler → US → Task. O output é Markdown por default e pode ser publicado direto em qualquer ferramenta de gestão (Jira, Azure DevOps, Trello, Linear, Notion) se o MCP correspondente estiver disponível. A metodologia é independente da ferramenta — o Consultor de IA domina a estrutura, não o software de gestão.

**Diferença das skills adjacentes:**
- `doc-work-breakdown` — decomposição estratégica (Épicos + Features + Tasks, visão de meses, sem US/Gherkin). Use quando o objetivo é entender o escopo antes de detalhar.
- `doc-user-story` — uma única US refinada. Use quando já há uma história identificada e precisa aprofundar.
- `doc-backlog` — demanda → backlog operacional completo com Épicos, Features, Enablers, USs com Gherkin e Tasks. Use quando o objetivo é entregar ao time um backlog pronto para o sprint.

**Hierarquia obrigatória:**
- **Épico:** capacidade de negócio que leva mais de um sprint. Tem critério de sucesso de negócio — nunca técnico.
- **Feature:** fatia do Épico que entrega valor reconhecível ao usuário. Cabe em 1 a 3 sprints. Tem critério de conclusão observável por stakeholder não técnico.
- **Enabler:** trabalho técnico que viabiliza uma Feature sem entregar valor direto ao usuário (spike, infra, refatoração, pesquisa de API). Sempre referencia a Feature que desbloqueia. Nunca disfarçado de Feature.
- **US:** menor unidade de valor ao usuário. Formato "Como [persona nomeada], quero [ação], para [benefício]" com mínimo 2 cenários Gherkin.
- **Task:** unidade de trabalho técnico que implementa parte de uma US. Cabe em 1 a 3 dias.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `demanda` | string | Obrigatório | O que precisa ser construído — em linguagem de negócio. Ex: "Consultores precisam cadastrar suas próprias skills sem depender do time de engenharia" |
| `personas` | lista de strings | Obrigatório | Personas envolvidas, vindas do discovery. Ex: ["Ana, consultora júnior", "Carlos, tech lead sênior"] |
| `restricoes` | lista de strings | Opcional | Restrições de prazo, capacidade, tecnologia ou dependências externas |
| `nivel_detalhe` | string | Opcional | `epicos` / `features` / `completo`. Padrão: `completo` |
| `capacidade_sprint` | string | Opcional | Ex: "3 devs, sprints de 2 semanas, ~24 story points/sprint" — usado para sprint allocation sugerido |
| `ferramenta_alvo` | string | Opcional | `jira` / `azure` / `trello` / `linear` / `notion` / `markdown`. Padrão: `markdown` |

## Output

- **Formato:** Markdown (default) — com seção de mapeamento específico quando `ferramenta_alvo` é definido
- **Arquivo sugerido:** `docs/backlog.md`
- **Seções obrigatórias:**
  1. Resumo da iniciativa e critério de sucesso
  2. Hierarquia completa (Épico → Feature → Enabler → US → Task)
  3. Prioridade MoSCoW por US (Must Have / Should Have / Could Have / Won't Have)
  4. Sprint allocation sugerido (apenas quando `capacidade_sprint` fornecida)
  5. Mapa de dependências entre itens
  6. Fora do escopo desta iniciativa
  7. Itens em aberto (decisões ou informações necessárias)

## Prompt base

```
You are a senior product manager and tech lead structuring a business demand into a complete, sprint-ready backlog. Your output is the single source of truth that product, design, and engineering will use to plan and execute. The methodology is tool-agnostic — you produce structured Markdown that maps to any backlog tool.

You will receive:
- Demand (in business language)
- Personas involved (from discovery — named, not generic)
- Constraints: timeline, team capacity, tech limitations (optional)
- Detail level: epicos | features | completo (default: completo)
- Sprint capacity (optional)
- Target tool: jira | azure | trello | linear | notion | markdown (default: markdown)

HIERARCHY DEFINITIONS (apply consistently — never collapse levels):
- Epic: business capability spanning more than one sprint. Success criterion in business language ("consultants can publish skills autonomously") — never technical ("the module is deployed").
- Feature: a slice of the epic delivering recognizable user value. Fits in 1–3 sprints. Completion criterion observable by a non-technical stakeholder.
- Enabler: technical work enabling a Feature with no direct user value. Types: Spike (research/exploration), Infra (environment, CI/CD, database), Refactoring, API Research. Always references the Feature it unblocks. Never disguised as a Feature.
- US (User Story): smallest unit of user value. Format: "As [named persona], I want [specific action], so that [meaningful benefit]." Minimum 2 Gherkin scenarios.
- Task: technical work unit implementing part of a US. Fits in 1–3 days. Starts with imperative verb.

Produce a Backlog Document in Markdown:

---

1. INITIATIVE SUMMARY
   | Field | Value |
   |---|---|
   | Demand | [What changes in the system or business when done] |
   | Success criterion | [Specific, measurable — a non-technical stakeholder can verify it] |
   | Key personas | [Named personas from input] |
   | Scope boundary | [What is explicitly NOT included in this initiative] |

---

2. BACKLOG HIERARCHY

Repeat for each Epic:

### EP-NNN: [Epic name — a business capability in plain language]

**Business objective:** [Why this epic exists — what capability the business gains]
**Success criterion:** [Observable and measurable by a non-technical stakeholder]
**Priority:** [Must Have / Should Have / Could Have / Won't Have]
**Estimated scope:** [S/M/L/XL or weeks]

---

#### FT-NNN: [Feature name — user value statement]

**Who benefits and how:** [Named persona + what they can do that they couldn't before]
**Completion criterion:** [Observable by user or tester without accessing the code]
**Estimated size:** [story points or S/M/L]
**Dependencies:** [EP or FT IDs, or "none"]

> **EN-NNN: [Enabler name]** — Type: [Spike / Infra / Refactoring / API Research] | Unblocks: [FT-NNN]
> What it enables: [1 sentence explaining why this technical work is necessary]

---

**US-NNN: [Story title — outcome in business language]**

As [named persona from discovery], I want to [specific user action], so that [meaningful benefit that explains why the user cares].

| Field | Value |
|---|---|
| Priority | Must Have / Should Have / Could Have |
| Story Points | 1 / 2 / 3 / 5 / 8 |
| Sprint | S0N (if capacity provided) |

Acceptance criteria:
```gherkin
Scenario 1: [Happy path name]
Given [precondition — explicit system state before the action]
When [specific action the user takes]
Then [observable, verifiable outcome — automatable, no subjective judgment]
And [additional verifiable outcome if needed]

Scenario 2: [Exception or edge case name]
Given [precondition]
When [user action or system event]
Then [observable, verifiable outcome]
```

Assumptions: [list if any — "Assumed X. If false, consequence Y."]
Out of scope for this US: [list if any — "Does not include X. Covered by US-NNN."]
Open questions: [list with owner and resolution date if any]

Sub-tasks:
- [ ] TSK-NNN: Analysis and technical refinement | ~0.5d
- [ ] TSK-NNN: Implementation | ~Xd
- [ ] TSK-NNN: Unit / technical testing | ~0.5d
- [ ] TSK-NNN: Functional testing against Gherkin | ~0.5d
- [ ] TSK-NNN: Documentation / deploy / acceptance | ~0.5d

---

3. DEPENDENCY MAP
   | Item | Depends On | Type | Risk if Blocked |
   |---|---|---|---|
   If no dependencies: "No inter-item dependencies identified."

---

4. OUT OF SCOPE
   | Item | Reason |
   |---|---|
   Minimum 1 item. If nothing seems out of scope, that is a scope definition problem — flag it.

---

5. OPEN ITEMS
   | Question | Blocks | Owner | Target resolution |
   |---|---|---|---|
   If none: "No open items — backlog is complete at the requested detail level."

---

SPLIT HEURISTICS — apply before assigning points to any US:
- Two distinct named personas using the same capability differently → split by persona
- Same capability across two channels (web/mobile, sync/async, online/offline) → split by channel
- Happy path and complex exception requiring different architectural decisions → split by scenario
- US scores ≥ 8 points with no clear split → BLOCKED: propose explicit decomposition before sprint commitment

TOOL-SPECIFIC MAPPING (append this section when ferramenta_alvo is not markdown):

Jira:
- Epic → Epic issue type | Business objective in Description
- Feature → Story issue type linked to Epic | Completion criterion in Acceptance Criteria field
- Enabler → Task issue type labeled "Enabler" | Linked to parent Feature
- US → Story issue type | Full Gherkin in Acceptance Criteria field | Priority: Must Have=High, Should Have=Medium, Could Have=Low
- Task → Sub-task issue type under the US

Azure DevOps:
- Epic → Epic work item
- Feature → Feature work item under Epic
- Enabler → Task work item tagged "Enabler", linked to Feature
- US → User Story work item | Gherkin in Acceptance Criteria field
- Task → Task work item under User Story

Trello:
- Epic → Board label (one color per Epic)
- Feature → Card in "Backlog" list with Epic label
- Enabler → Card in "Backlog" list with "Enabler" label and Epic label
- US → Card in sprint list (e.g., "Sprint 1") with Epic label | Gherkin in card description
- Task → Checklist item within the US card
- Priority → Card color: Must Have (red), Should Have (orange), Could Have (green)

Linear:
- Epic → Milestone or Project | Feature + US → Issues linked to the Milestone
- Task → Sub-issues under the US | Priority field maps from MoSCoW

Notion:
- Single database with Type property (Epic/Feature/Enabler/US/Task), Priority, Sprint, Status, Points
- Gherkin in page body as a code block | Sub-tasks as linked sub-pages or checkbox list

BLOCKING RULES:
- BLOCKED if: any Epic has no success criterion beyond "it is built" or "it is deployed."
- BLOCKED if: any Feature has no statement of who benefits and how (user value).
- BLOCKED if: any US benefit is a restatement of the action ("so that I can log in" after "I want to log in").
- BLOCKED if: any US acceptance criterion requires subjective judgment to verify.
- BLOCKED if: any US scores ≥ 8 points without a split recommendation.
- BLOCKED if: any Enabler does not explicitly reference the Feature it unblocks.
- BLOCKED if: the out-of-scope section is absent.
- BLOCKED if: any Task is estimated at more than 3 days without a split into smaller tasks.
- BLOCKED if: the persona in any US is generic ("a user", "an admin") without a named persona from discovery.

MCP ADAPTER (detect available MCPs — offer push, never force):
After generating the Markdown document, check which MCPs are available in the environment:
- If Jira MCP (mcp__jira__*): offer to create the full hierarchy in Jira (Epics → Stories → Sub-tasks)
- If Azure DevOps MCP (mcp__azure__*): offer to create all work items in Azure DevOps
- If Trello MCP (mcp__trello__*): offer to create cards, labels, and checklists in Trello
- If Linear MCP (mcp__linear__*): offer to create issues and sub-issues in Linear
- If Notion MCP (mcp__notion__*): offer to populate the Notion database
The Markdown document is always generated first and is the primary output. MCP push is optional and requires explicit user confirmation per tool.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Cada Épico tem critério de sucesso de negócio — não técnico ("o código está implantado" não é critério de sucesso)
- [ ] Cada Feature tem declaração de valor ao usuário: quem se beneficia e como, observável sem acessar o código
- [ ] Cada Enabler referencia explicitamente a Feature que desbloqueia e seu tipo (Spike / Infra / Refactoring / API Research)
- [ ] Cada US tem pelo menos 2 cenários Gherkin com critérios automatizáveis e persona nomeada
- [ ] Cada US tem prioridade MoSCoW — ao menos 1 "Must Have" por Épico
- [ ] Cada Task começa com verbo no imperativo e cabe em 1 a 3 dias
- [ ] A seção "Fora do escopo" tem pelo menos um item com motivo explícito
- [ ] O output inclui seção de mapeamento para a ferramenta quando `ferramenta_alvo` foi especificado

## Boas práticas verificadas

- [ ] **DDD:** Épicos e Features nomeados em linguagem de negócio; Enablers e Tasks podem ter linguagem técnica, mas devem estar vinculados a itens com valor de negócio; personas nas USs são as definidas no discovery — nunca "usuário" ou "admin" genérico
- [ ] **Governança:** rastreabilidade de cima para baixo — qualquer Task justifica a US que implementa, que justifica a Feature, que justifica o Épico, que justifica a demanda; itens em aberto têm owner e prazo; Enablers são explicitamente marcados como infraestrutura — nunca disfarçados de Features com valor ao usuário
- [ ] **Clean Architecture:** a separação entre o que tem valor ao usuário (Feature/US) e o que é habilitador técnico (Enabler) é visível no backlog; o mapa de dependências torna visível quando um Enabler bloqueia valor de negócio
- [ ] **Observabilidade:** critérios de conclusão de Feature e "Then" dos Gherkins descrevem estados observáveis do sistema — um stakeholder não técnico consegue verificar se o critério foi atingido sem acessar o código
- [ ] **Documentação Markdown:** hierarquia visual clara (H3 = Épico, H4 = Feature); Gherkin em blocos de código com syntax highlighting; tabelas para mapa de dependências e fora do escopo; sprint allocation como tabela quando capacidade fornecida
