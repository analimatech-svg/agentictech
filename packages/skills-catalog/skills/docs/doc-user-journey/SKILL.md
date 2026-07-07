# Skill: Jornada do Usuário

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Discovery |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Mapear a jornada do usuário descrevendo cada etapa da interação com o sistema: ação realizada, pensamento, emoção, ponto de atrito e oportunidade de melhoria. A skill produz dois artefatos distintos: a jornada AS-IS (como o usuário faz hoje, com o sistema atual ou processo manual) e, quando solicitada, a jornada TO-BE (como deveria ser com o sistema proposto). Mapear apenas a TO-BE sem a AS-IS é um anti-padrão — não revela onde está o atrito real.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `persona` | string | Obrigatório | Persona cujo fluxo será mapeado — deve ser uma persona definida no documento de discovery com objetivos e frustrações documentados |
| `objetivo_jornada` | string | Obrigatório | O objetivo específico que a persona quer alcançar nesta jornada (ex: "contratar um consultor para um projeto de 3 meses") — uma persona pode ter múltiplas jornadas; esta skill cobre uma por vez |
| `fluxo_atual` | string | Obrigatório | Descrição do fluxo atual (AS-IS): passos que o usuário percorre hoje com o sistema existente ou processo manual — pode ser transcrito de entrevistas ou observações |
| `dados_emocionais` | string | Opcional | Dados qualitativos de pesquisa sobre como o usuário se sente em cada etapa (citações de entrevistas, resultados de escala de esforço CES, NPS por etapa) |
| `fluxo_proposto` | string | Opcional | Descrição do fluxo futuro (TO-BE) se já for conhecido — quando ausente, a skill gera apenas a AS-IS com oportunidades, sem especular o TO-BE |
| `limite_etapas` | número | Opcional | Número máximo de etapas por jornada (padrão: 10). Jornadas com mais de 10 etapas devem ser divididas em sub-jornadas com ponto de handoff explícito |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/user-journey-[persona]-[objetivo-slug].md`
- **Seções obrigatórias:**
  1. Cabeçalho da jornada (persona, objetivo, tipo: AS-IS / TO-BE / AS-IS + TO-BE)
  2. Tabela de jornada AS-IS (etapa, ação, pensamento, emoção, ponto de atrito, oportunidade)
  3. Tabela de jornada TO-BE (quando `fluxo_proposto` é fornecido)
  4. Mapa de calor emocional (resumo visual das emoções por etapa — alto/médio/baixo engajamento)
  5. Top 3 atritos por impacto (ordenados por frequência × severidade)
  6. Top 3 oportunidades acionáveis (cada uma deve ser executável por um time a partir da descrição)
  7. Pontos de handoff (onde a jornada começa, termina ou passa para outro sistema ou persona)

## Prompt base

```
You are a senior UX researcher creating a journey map that the product and engineering team will use to prioritize improvements. The map must reflect reality (AS-IS) before proposing a future state (TO-BE).

You will receive:
- Persona (with goals and frustrations from the discovery document)
- Journey objective (one specific goal the persona is trying to achieve)
- Current flow description (AS-IS) — what the user does today
- Emotional data (optional)
- Proposed flow (TO-BE) — optional; only generate if provided
- Maximum steps limit (default: 10 per journey)

Produce a User Journey Document in Markdown with the following sections:

1. JOURNEY HEADER
   | Field | Value |
   |---|---|
   | Persona | [Persona name] |
   | Journey objective | [What the persona is trying to accomplish] |
   | Journey type | AS-IS / TO-BE / AS-IS + TO-BE |
   | Data source | [What evidence grounds this journey — interviews, observations, analytics] |

2. AS-IS JOURNEY TABLE (always required)
   Columns: Stage | Action | Thought | Emotion | Friction Point | Improvement Opportunity
   
   Rules for each column:
   - Stage: a noun describing where the user is in the flow (not a verb).
   - Action: what the user actually does (observed or reported) — not what the system does.
   - Thought: what the user is thinking at this moment — must reflect their mental model, not ours.
   - Emotion: use a consistent 5-point scale: 😤 Frustrated | 😕 Confused | 😐 Neutral | 🙂 Satisfied | 😊 Delighted. If emotional data is not available, leave the column with "[No data — estimated]" and note the limitation.
   - Friction Point: specific and contextualized. Must answer: what exactly makes this step harder than it should be?
     Anti-pattern ❌: "The process is complex" → ✅ "The user must switch between 3 different tabs to cross-reference the data needed to complete this step, causing an average of 4 minutes of context-switching."
   - Improvement Opportunity: actionable. Must be specific enough that a team could start working on it immediately.
     Anti-pattern ❌: "Improve the interface" → ✅ "Consolidate the 3 data sources into a single side-panel view so the user can complete this step without changing tabs."

3. TO-BE JOURNEY TABLE (only if proposed flow is provided)
   Same columns as AS-IS. Add a "Delta" column: what specifically changes compared to the AS-IS step.
   - Anti-pattern: do not generate a TO-BE based on assumptions. If no proposed flow is provided, state: "TO-BE journey not generated — proposed flow not provided. Generate AS-IS and opportunities first, then define the TO-BE based on prioritized opportunities."

4. EMOTIONAL HEATMAP
   Text-based representation of emotional intensity per stage:
   | Stage | AS-IS Emotion | TO-BE Emotion (if available) | Change |
   |---|---|---|---|
   Summarize the emotional arc: where does the experience peak negatively? where is the lowest point?

5. TOP 3 FRICTION POINTS BY IMPACT
   Ordered by: frequency (how often this affects the persona) × severity (impact when it occurs).
   For each: Friction | Frequency | Severity | Business Impact | Source
   - Business impact must connect the friction to a measurable consequence: abandonment rate, time lost, support tickets generated, NPS detractor, etc.

6. TOP 3 ACTIONABLE OPPORTUNITIES
   For each: Opportunity | Effort (Low/Medium/High) | Impact (Low/Medium/High) | Friction it resolves | Recommended next step
   - "Next step" must be concrete: a User Story, a discovery spike, an A/B test, a stakeholder decision needed.

7. HANDOFF POINTS
   Where does this journey start? (precondition or trigger)
   Where does it end? (outcome or exit condition)
   Are there steps where the user transitions to another system, persona, or channel? Document each handoff.
   - Handoffs with no clear owner or no defined trigger are design risks — flag them.

Rules:
- BLOCKED if: the journey has more than [limite_etapas] steps without a recommended split point. A journey with 15+ steps is a process, not a journey — split at the most natural handoff and map each sub-journey separately.
- BLOCKED if: any friction point is generic ("the process is complex", "the UI is confusing") without specific context.
- BLOCKED if: a TO-BE journey is generated without a provided proposed flow — do not invent a future state.
- BLOCKED if: emotions are invented without data or without explicit flagging as estimated.
- AS-IS must always precede TO-BE. Never map only the TO-BE — it produces a vision disconnected from the real problem.

Write in the language of the input.
```

## Critério de sucesso

- [ ] A jornada AS-IS está presente e baseada no fluxo atual descrito — não especulativa
- [ ] Cada etapa tem ação e emoção associadas (ou emoção flagada como "[estimada — sem dado]" quando não há evidência)
- [ ] Os pontos de atrito são específicos: descrevem o que exatamente dificulta a etapa, com contexto e frequência quando disponível
- [ ] As 3 oportunidades de melhoria são acionáveis: um time consegue iniciar o trabalho com base apenas na descrição fornecida
- [ ] Os pontos de handoff (início, fim e transições) estão documentados — handoffs sem dono identificado são sinalizados como risco

## Boas práticas verificadas

- [ ] **Governança:** AS-IS documentada antes de qualquer TO-BE — decisões de produto baseadas em jornada AS-IS têm evidência auditável; oportunidades têm próximo passo concreto (US, spike, decisão), não ficam em aberto
- [ ] **DDD:** ações e pensamentos descritos na linguagem do usuário — o domínio é visto pelo olhar da persona, não pelo olhar do sistema; etapas nomeadas em linguagem de negócio (não nomes de telas ou componentes)
- [ ] **Observabilidade:** mapa de calor emocional conecta fricção à métrica de negócio (abandonment, NPS, suporte) — não apenas descreve como o usuário se sente, mas qual impacto isso tem no produto
- [ ] **Documentação Markdown:** tabela de jornada legível por stakeholders não técnicos; mapa de calor separado para leitura executiva; top 3 atritos e oportunidades como seções destacadas para facilitar priorização
- [ ] **Limite de escopo:** jornada com mais de 10 etapas é dividida em sub-jornadas com handoff explícito — uma jornada longa demais é um sinal de que o objetivo mapeado é muito amplo para gerar decisões acionáveis
