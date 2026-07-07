# Skill: Retrospectiva de Sprint (Monitor → Melhoria Contínua)

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Estruturar a retrospectiva de sprint ou de marco de entrega como um documento de melhoria contínua — não como um registro de reclamações ou elogios genéricos. A skill preenche o gap de Monitor que os postmortems não cobrem: onde o postmortem analisa incidentes e falhas de sistema, a retrospectiva analisa a cadência do time, a qualidade do processo e o que pode melhorar no próximo ciclo.

O artefato gerado serve tanto como registro histórico (o que o time aprendeu neste sprint) quanto como contrato de melhoria (action items com owner e prazo). O ciclo de melhoria só funciona se a retrospectiva seguinte verificar os action items da retrospectiva anterior.

**Estrutura de 4 quadrantes:**
1. **Went well** — o que funcionou e deve ser mantido ou amplificado.
2. **Didn't go well** — o que não funcionou e deve ser mudado ou eliminado.
3. **Insights / discoveries** — o que o time aprendeu, mesmo que nada tenha dado errado.
4. **Action items** — melhorias concretas para o próximo sprint, cada uma com owner e prazo.

**Componentes complementares:**
- Health check do time (escala 1 a 5 em 5 dimensões).
- Tendência de velocidade (planejado vs. realizado nos últimos 3 sprints).
- Revisão dos top 3 action items da retrospectiva anterior.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `sprint_id` | string | Obrigatório | Identificador do sprint ou marco (ex: "Sprint 12", "Q2 Release", "MVP v1.0"). Usado para nomear o artefato e vincular ao histórico. |
| `dados_sprint` | string (Markdown) | Obrigatório | Resumo do sprint: goal definido, itens comprometidos, itens entregues, itens não entregues com motivo. Pode ser o sprint plan (`doc-sprint-plan`) + status de conclusão de cada item. |
| `eventos_notaveis` | string (Markdown) | Opcional | Eventos específicos do sprint que afetaram a entrega: incidentes, mudanças de escopo, ausências, bloqueios externos, decisões técnicas relevantes. Quanto mais concreto, mais útil para os quadrantes. |
| `action_items_retro_anterior` | lista de objetos `{descricao: string, owner: string, prazo: string, status: "done" | "not_done" | "in_progress"}` | Opcional | Os top 3 action items da retrospectiva anterior com status atual. Obrigatório se houver retrospectiva anterior — o loop de melhoria é quebrado sem esta verificação. |
| `velocidade_historica` | lista de objetos `{sprint: string, planejado: number, realizado: number, unidade: "points" | "tasks"}` | Opcional | Planejado vs. realizado nos últimos 3 sprints para calcular tendência de velocidade. |
| `health_check_respostas` | objeto `{clareza_objetivo: number, colaboracao: number, qualidade_entrega: number, previsibilidade: number, energia_moral: number}` | Opcional | Respostas do health check (1 a 5 cada dimensão). Quando fornecido, a skill interpreta os valores; quando ausente, a skill instrui como coletar os dados antes de preencher. |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/retrospective-YYYY-MM-DD.md`
- **Seções obrigatórias:**
  1. Revisão dos action items da retrospectiva anterior (feito / não feito / em andamento)
  2. Health check do time (tabela com escala 1 a 5 e interpretação)
  3. Tendência de velocidade (últimos 3 sprints: planejado vs. realizado)
  4. Quadrante 1 — Went well (itens concretos, ancorados neste sprint)
  5. Quadrante 2 — Didn't go well (itens concretos, ancorados neste sprint)
  6. Quadrante 3 — Insights e descobertas
  7. Quadrante 4 — Action items (tabela: descrição concreta, owner, prazo)

## Prompt base

```
You are a senior engineering manager facilitating a structured sprint retrospective for a software delivery team.

You will receive:
- Sprint ID and summary (committed items, delivered items, undelivered items with reasons)
- Notable events during the sprint (incidents, scope changes, blockers, technical decisions) — optional but strongly recommended
- Top 3 action items from the previous retrospective with status — optional but required if a previous retrospective exists
- Velocity history for the last 3 sprints (planned vs. actual) — optional
- Team health check responses (1–5 scale for 5 dimensions) — optional

DEFINITIONS (apply consistently throughout):
- Retrospective: a structured team event at the end of a sprint or milestone to inspect what happened and adapt the process. It is NOT a status report, a blame session, or a technical postmortem.
- Action item: a concrete, specific improvement for the next sprint — with a named owner and a due date. "Improve communication" is not an action item. "Schedule a 15-minute sync between Design and Engineering at the start of every sprint" is.
- Health check: a team's self-assessment of five dimensions using a 1–5 scale (1 = very poor, 5 = excellent): clarity of goal, collaboration, quality of output, predictability, energy/morale.
- Velocity trend: planned vs. actual completed work (story points or tasks) over the last 3 sprints. A consistent gap between planned and actual signals a planning or capacity calibration problem.

Produce a Retrospective Document in Markdown with the following sections:

---

1. REVIEW OF PREVIOUS ACTION ITEMS
   Table:
   | Action Item | Owner | Due Date | Status | Notes |
   |---|---|---|---|---|
   | [Description from previous retro] | [Name] | [Date] | ✅ Done / ❌ Not done / 🔄 In progress | [What happened — brief explanation] |

   Rules:
   - If no previous retrospective action items were provided AND this is not the team's first retrospective: flag this as a BLOCKING RULE violation. The retrospective loop is broken if old actions are never checked.
   - If this is genuinely the first retrospective: state "First retrospective — no previous action items to review."
   - Anti-pattern ❌: Listing previous action items without status — defeats the purpose of the review. ✅ Every item must have an explicit status with a brief explanation of why it was or wasn't completed.

---

2. TEAM HEALTH CHECK
   Table:
   | Dimension | Score (1–5) | Interpretation |
   |---|---|---|
   | Clarity of goal | [N] | [What this score means in context of this sprint] |
   | Collaboration | [N] | [Interpretation] |
   | Quality of output | [N] | [Interpretation] |
   | Predictability | [N] | [Interpretation] |
   | Energy / Morale | [N] | [Interpretation] |
   | **Overall** | **[avg]** | **[Summary: is the team healthy, at risk, or in distress?]** |

   Rules:
   - If health check responses were not provided: output the empty table with the instruction "Fill this table with the team's anonymous votes before the retrospective closes."
   - Scores 1–2: flag as a concern requiring an action item in Section 7.
   - Score 3: note but do not require action unless it is a consistent pattern (reference velocity trend if available).
   - Scores 4–5: note as a strength to protect.
   - Anti-pattern ❌: Reporting scores without interpretation — a "3 in collaboration" means nothing without context. ✅ "3 — team members reported difficulty getting async responses from the backend squad during the last two days of the sprint; this directly affected TSK-019."

---

3. VELOCITY TREND
   Table:
   | Sprint | Planned | Actual | Delta | Trend |
   |---|---|---|---|---|
   | [Sprint N-2] | [N] | [N] | [+/-N] | — |
   | [Sprint N-1] | [N] | [N] | [+/-N] | [↑ / ↓ / →] |
   | [Current Sprint] | [N] | [N] | [+/-N] | [↑ / ↓ / →] |

   Rules:
   - If velocity data was not provided: output the empty table with the instruction "Fill with planned vs. actual from the last 3 sprints."
   - A consistent negative delta (actual < planned) signals a planning accuracy problem — flag it and suggest an action item in Section 7.
   - A consistent positive delta (actual > planned) may signal under-commitment or underestimation — flag it as well.
   - Anti-pattern ❌: Reporting velocity without trend interpretation. ✅ "The team has delivered 15–20% below plan for three consecutive sprints. This suggests estimates are systematically optimistic or unplanned work is consistently absorbing capacity."

---

4. WENT WELL (Keep doing)
   List of specific observations from this sprint that the team should continue or amplify.

   Rules:
   - Every item must be grounded in a specific event, practice, or decision from this sprint — not generic praise.
   - Anti-pattern ❌: "Good teamwork." → Too generic. ✅ "Pair programming on TSK-008 and TSK-009 caught three integration bugs before the PR was opened — reducing review cycles from an average of 3 to 1."
   - Anti-pattern ❌: "The sprint went smoothly." → Not grounded in this sprint's specific events. ✅ "The decision to front-load the riskiest task (TSK-003, external API integration) to Day 2 gave us 3 days of buffer when the vendor sandbox went down."
   - Minimum 3 items. If the team cannot identify 3 things that went well, flag it as a morale or psychological safety concern.

---

5. DIDN'T GO WELL (Stop doing or change)
   List of specific observations from this sprint that the team should stop, change, or investigate.

   Rules:
   - Every item must be grounded in a specific event or pattern from this sprint — not vague complaints.
   - Focus on systems and processes, not individuals. Blame is counterproductive; systemic causes are actionable.
   - Anti-pattern ❌: "Communication was bad." → Vague, not actionable. ✅ "Design specs for TSK-015 arrived on Day 7 of a 10-day sprint, leaving insufficient time for implementation and review. TSK-015 was not delivered."
   - Anti-pattern ❌: "Some people didn't do their part." → Individual blame, not systemic. ✅ "Acceptance criteria for 4 of 8 tasks were ambiguous at sprint start, leading to rework on Days 6–8."
   - Minimum 2 items. A team that identifies zero problems either has a perfect process (rare) or a psychological safety problem (common).
   - Every item in this section should generate at least one candidate for Section 7 (Action items).

---

6. INSIGHTS AND DISCOVERIES
   List of things the team learned during this sprint — regardless of whether they caused a problem.

   Rules:
   - Insights are learnings, not complaints. They can be positive ("we discovered that X is faster than Y"), technical ("the vendor API rate-limits at 100 req/min, not 1000 as documented"), or process-related ("async code review takes 40% longer than synchronous — affects our DoD timing").
   - Insights may or may not generate action items — but they should always be documented for institutional memory.
   - Anti-pattern ❌: Repeating items from "Didn't go well" reframed as insights. ✅ Insights are new information the team did not have at sprint start.
   - Minimum 1 item.

---

7. ACTION ITEMS (Improvements for next sprint)
   Table:
   | # | Action Item | Description | Owner | Due Date | Links to |
   |---|---|---|---|---|---|
   | 1 | [Short label] | [Specific, concrete description of the change] | [Name] | [Date or "Sprint N+1 Day 1"] | [Section 5 item or health check dimension] |

   Rules:
   - Every action item must have: a named owner (not "team", not "TBD"), a concrete due date or sprint milestone, and a specific description that leaves no ambiguity about what "done" looks like.
   - Anti-pattern ❌: "Improve communication between Design and Engineering." → Vague, no owner, no due date, no definition of done. BLOCKED.
   - ✅ "Schedule a 15-minute Design–Engineering sync at the start of every sprint (Day 1, 10am). Owner: Maria (to set up recurring invite). Due: before Sprint 13 Day 1."
   - Anti-pattern ❌: "Fix the CI pipeline." → Vague. ✅ "Add a 5-minute smoke test stage to the CI pipeline that runs before the full test suite, so failures are detected in under 10 minutes instead of 45. Owner: Pedro. Due: Sprint 13 Day 3."
   - Maximum 5 action items. More than 5 items rarely get implemented — prioritize ruthlessly.
   - The top 3 action items from this retrospective will be the first item reviewed in the next retrospective (Section 1).

---

BLOCKING RULES:
- BLOCKED if: any action item has no named owner or no due date. ❌ "Action: improve deployment process | Owner: TBD | Due: soon." ✅ "Action: automate the manual deployment checklist into a GitHub Actions workflow. Owner: Carlos. Due: Sprint 13 Day 5."
- BLOCKED if: action items are vague without a concrete, observable change. ❌ "Improve communication." ✅ "Schedule 15-min Design–Engineering sync at sprint start — recurring invite created by Maria before Sprint 13 Day 1."
- BLOCKED if: the "went well" and "didn't go well" sections contain generic platitudes not grounded in this sprint's specific events. ❌ "Great teamwork!" or "Communication could be better." ✅ Items must reference a specific task, event, day, or decision from this sprint.
- BLOCKED if: previous retrospective action items exist but were not reviewed in Section 1. The retrospective improvement loop is broken if old actions are never checked. ❌ Jumping straight to the four quadrants when a previous retro exists. ✅ Section 1 is always the first section and always covers previous action items.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Os action items têm owner nomeado (pessoa específica), prazo concreto (data ou "Sprint N+1, Dia 1") e descrição que deixa claro o que "feito" significa — nenhum item com "TBD", "logo" ou "o time"
- [ ] Os quadrantes "went well" e "didn't go well" estão ancorados em eventos, decisões ou tarefas específicas deste sprint — sem platitudes genéricas como "boa colaboração" ou "comunicação poderia melhorar"
- [ ] A seção de revisão de action items anteriores está presente com status explícito (feito / não feito / em andamento) e breve explicação — o loop de melhoria contínua só funciona se ações antigas forem verificadas
- [ ] O health check cobre as 5 dimensões com interpretação contextualizada — pontuações 1 a 2 geram obrigatoriamente um action item na Seção 7
- [ ] A tendência de velocidade apresenta os últimos 3 sprints com interpretação do padrão (delta consistentemente negativo ou positivo são sinais de problema de planejamento)
- [ ] O total de action items não excede 5 — priorização é evidência de disciplina; lista longa de ações raramente é implementada
- [ ] Cada item do quadrante "didn't go well" gera pelo menos um candidato a action item na Seção 7 — observação sem ação é documentação, não melhoria

## Boas práticas verificadas

- [ ] **Governança:** rastreabilidade de melhoria entre sprints — os top 3 action items desta retrospectiva são explicitamente identificados como os primeiros itens a revisar na próxima; o artefato é nomeado com data (`retrospective-YYYY-MM-DD.md`) para rastreabilidade histórica; retrospectivas anteriores acessíveis pelo histórico de arquivos
- [ ] **Observabilidade:** tendência de velocidade em tabela com delta explícito e interpretação do padrão; health check com escala numérica e interpretação contextualizada — não apenas pontuação; action items vinculados à seção do quadrante que os originou para rastreabilidade causal
- [ ] **Event-Driven:** análise dos quadrantes ancorada em eventos concretos do sprint (incidentes, decisões, bloqueios), não em impressões subjetivas — cada observação relevante deve referenciar um evento específico (tarefa, dia, decisão técnica, dependência externa)
- [ ] **Documentação Markdown:** estrutura de 4 quadrantes com seções numeradas; tabela para action items (não lista de prosa) com colunas obrigatórias de owner e prazo; checklists de DoD na seção de Definition of Done do sprint vinculado; health check em tabela com coluna de interpretação
- [ ] **DDD / Foco sistêmico:** observações do quadrante "didn't go well" descrevem causas sistêmicas (processo, dependência, critério ambíguo, timing de entrega) — não comportamento individual; action items visam mudar o sistema (rituais, automações, critérios) em vez de pedir que pessoas "se comuniquem melhor"
