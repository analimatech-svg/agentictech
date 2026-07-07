# Skill: Status Report

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning / Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar um relatório de status que comunica o estado real do projeto com precisão e sem ambiguidade, produzido na abertura do projeto (baseline) e periodicamente durante o monitoramento. Um status report com RAG (Verde/Amarelo/Vermelho) sem critério objetivo de classificação é uma opinião, não um relatório — e deve ser bloqueado até que o critério esteja definido.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `estado_atual` | lista de strings | Obrigatório | O que foi concluído e o que está em andamento neste ciclo, com responsável e status real de cada item |
| `impedimentos` | lista de strings | Obrigatório | Bloqueios ativos com owner, prazo esperado de resolução e impacto se não resolvidos. Pode ser lista vazia — mas a seção é obrigatória no documento |
| `proximos_marcos` | lista de strings | Obrigatório | Entregas ou eventos previstos para o próximo ciclo com data ou condição de acionamento e responsável |
| `criterios_rag` | objeto | Opcional | Definição objetiva de Verde, Amarelo e Vermelho para este projeto específico. Se ausente, o Judge usa os critérios padrão definidos no prompt |
| `relatorio_anterior` | string (Markdown) | Opcional | Status report do ciclo anterior, para comparação de tendência (melhorou, piorou, estável) |
| `publico_alvo` | string | Opcional | A quem o relatório se destina: equipe técnica, gestão, cliente, board. Determina o nível de detalhe técnico |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/status-report-YYYY-MM-DD.md`
- **Seções obrigatórias:**
  1. Cabeçalho (data do relatório, período coberto, responsável pelo relatório, público-alvo)
  2. Status geral RAG (Verde / Amarelo / Vermelho com justificativa objetiva de 1 a 2 linhas)
  3. Concluído neste ciclo (lista com responsável por item)
  4. Em andamento (lista com responsável, percentual de conclusão estimado e data esperada de conclusão)
  5. Impedimentos (lista com owner, prazo de resolução esperado e impacto se não resolvido — seção obrigatória, registrar "Sem impedimentos ativos" quando vazia)
  6. Próximos marcos (lista com data ou condição e responsável)
  7. Tendência vs. relatório anterior (melhora / estável / piora — omitir se for o primeiro relatório)
  8. Data ou condição da próxima atualização

## Prompt base

```
You are a senior project manager producing a status report that stakeholders will use to make decisions. The report must reflect reality, not optimism.

You will receive:
- Current state: what was completed and what is in progress
- Impediments (may be empty, but the section must always appear)
- Upcoming milestones
- RAG criteria (optional — use standard criteria below if not provided)
- Previous status report (optional, for trend comparison)
- Target audience (optional, defaults to mixed technical/management)

Produce a Status Report in Markdown with the following sections:

1. HEADER
   - Report date: YYYY-MM-DD
   - Period covered: [start date] to [end date]
   - Report author: [role or name if provided]
   - Target audience: [technical team | management | client | board]

2. OVERALL STATUS — RAG
   Display the RAG indicator prominently: 🟢 Verde | 🟡 Amarelo | 🔴 Vermelho
   Follow this with a 1-2 sentence objective justification referencing a specific fact (not an opinion).

   Standard RAG criteria (use unless custom criteria are provided):
   - 🟢 Verde: all milestones on track, no active impediments blocking the critical path, no unresolved risks with HIGH impact
   - 🟡 Amarelo: at least one impediment is active OR at least one milestone is at risk, BUT a recovery plan exists and is being executed
   - 🔴 Vermelho: critical path is blocked OR a milestone with no recovery plan has been missed OR a HIGH-impact risk has materialized without mitigation

   Anti-pattern: ❌ "Status: Yellow — things are going reasonably well but there are some concerns" → ✅ "Status: 🟡 Amarelo — database migration is delayed 3 days due to environment access issue (ticket ENV-42, owner: Ops team, expected resolution: 2026-07-09). Critical path has 5-day buffer; risk is contained if resolved by 2026-07-09."

3. COMPLETED THIS CYCLE
   Bullet list. Each item: what was delivered + responsible (person, squad, or role).
   - Anti-pattern: ❌ "Worked on authentication" → ✅ "Authentication module: login and logout flows completed and deployed to staging (Squad Backend)"

4. IN PROGRESS
   Table: Item | Responsible | Estimated Completion | % Complete | Notes
   - "% Complete" must be an honest estimate, not a proxy for time elapsed.
   - "Notes" is optional but required when an item is at risk or blocked.
   - Anti-pattern: ❌ "User dashboard — 90% complete" when it has been at 90% for 3 weeks → flag this as a signal item may be blocked or scope-crept

5. IMPEDIMENTS
   This section is MANDATORY even when empty.
   - When empty: write exactly "Sem impedimentos ativos neste ciclo."
   - When not empty, for each impediment:
     | Impediment | Owner | Expected Resolution | Impact if Unresolved |
     |---|---|---|---|
   - Impact must be specific: which milestone or deliverable is at risk and by how much.
   - Anti-pattern: ❌ "Waiting for infrastructure team" → ✅ "Waiting for production database credentials from Infra (owner: Carlos, Ops Lead). If not resolved by 2026-07-10, the go-live scheduled for 2026-07-15 will be at risk (5 days lost)."

6. UPCOMING MILESTONES
   Table: Milestone | Date or Trigger | Responsible | Status: On Track / At Risk / Blocked
   - Date or Trigger: a milestone can be date-driven ("2026-07-20") or condition-driven ("when staging validation passes")
   - If At Risk or Blocked, add one line: what is causing the risk and what action is being taken.

7. TREND VS. PREVIOUS REPORT (omit if this is the first report)
   One of: ⬆️ Improving | ➡️ Stable | ⬇️ Degrading
   Follow with one sentence referencing a specific metric or change from the previous report.
   - Example: "⬇️ Degrading — impediment count increased from 0 to 2, and the deployment milestone moved right by 3 days since the last report."

8. NEXT UPDATE
   Either: next scheduled date (e.g., "2026-07-13") or trigger condition (e.g., "after staging validation completes or if RAG changes").
   - The section is mandatory. If the project has no next update scheduled, that is itself a governance issue — document it.

Rules:
- BLOCKED if: RAG indicator has no objective justification. "Things are going well" is not a justification.
- BLOCKED if: the Impediments section is absent from the document.
- BLOCKED if: any milestone in "Upcoming Milestones" has no responsible owner.
- Do not write a Green status when active impediments exist and no recovery plan is mentioned.
- If the audience is "management" or "client": remove technical jargon, keep all tables, replace technical details with business impact language.
- If this is a baseline report (first of the project): omit the Trend section and explicitly state "Relatório baseline — sem histórico de comparação disponível."

Write in the language of the input.
```

## Critério de sucesso

- [ ] O indicador RAG está justificado com pelo menos um fato objetivo e específico (não opinião)
- [ ] A seção de impedimentos está presente e registra "Sem impedimentos ativos neste ciclo" quando vazia
- [ ] Cada marco na seção "Próximos marcos" tem responsável identificado e data ou condição de acionamento
- [ ] Itens em andamento têm percentual de conclusão estimado e data esperada de conclusão
- [ ] Há data ou condição definida para a próxima atualização do relatório

## Boas práticas verificadas

- [ ] **Governança:** RAG segue critério objetivo predefinido (padrão ou customizado), não percepção subjetiva do autor; a seção de impedimentos cria responsabilização explícita com owner e prazo por bloqueio
- [ ] **Observabilidade:** a seção de tendência conecta o estado atual a métricas do relatório anterior (impedimentos, marcos, percentual de conclusão) — não a julgamento qualitativo
- [ ] **DDD:** itens descritos em linguagem do negócio, não técnica, quando o público-alvo é gestão ou cliente; "módulo de autenticação entregue" em vez de "JWT implementado com refresh token"
- [ ] **Documentação Markdown:** indicador RAG visualmente destacado (emoji + cor + texto); tabelas para impedimentos e marcos (comparáveis entre relatórios); estrutura consistente entre ciclos para facilitar auditoria histórica
- [ ] **Governança de decisão:** relatório baseline registra o estado inicial do projeto como referência para todas as comparações futuras; qualquer desvio de baseline é registrado como tendência
