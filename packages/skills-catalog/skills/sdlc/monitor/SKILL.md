# Skill: Monitor

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Monitor |
| Nível mínimo Maestro | Regente |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Observar o sistema em produção, rastrear métricas DORA, identificar incidentes e degradações de performance, e propor ações de melhoria contínua, produzindo dashboards, alertas configurados e postmortems quando há incidentes.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `sistema_producao` | string | Obrigatório | Nome e descrição do sistema em produção a ser monitorado |
| `runbook` | string (Markdown) | Obrigatório | Runbook operacional com procedimentos de resposta a incidentes |
| `metricas_baseline` | objeto | Obrigatório | Valores de referência: latência p95, taxa de erro, disponibilidade, throughput |
| `dados_metricas` | string | Opcional | Dump ou resumo de métricas coletadas no período analisado |
| `logs_incidente` | string | Opcional | Logs ou timeline de um incidente ocorrido para análise postmortem |
| `periodo_analise` | string | Opcional | Período de tempo coberto pela análise (ex: "últimos 7 dias", "incidente de 2026-07-01") |

## Output

Pacote de monitoramento contendo:

- Dashboard de monitoramento: métricas DORA e métricas técnicas por serviço
- Estado atual do sistema versus baseline (com indicação de degradação)
- Alertas configurados com thresholds e canais de notificação
- Análise de tendências: melhoria ou degradação ao longo do período
- Postmortem quando há incidente: timeline, causa raiz, impacto, lições aprendidas
- Recomendações de melhoria contínua priorizadas por impacto

## Prompt base

```
You are a senior site reliability engineer (SRE) and observability specialist responsible for monitoring a production system and driving continuous improvement.

You will receive:
- System description
- Operational Runbook
- Metrics baseline (latency p95, error rate, availability, throughput)
- Metrics data for the analysis period (optional)
- Incident logs for postmortem analysis (optional)
- Analysis period

Produce a Monitoring Package in Markdown with the following sections:

1. DORA Metrics Summary (table: metric | current value | target | status: ON-TARGET/AT-RISK/OFF-TARGET)
   - Deployment Frequency: how often deployments happen
   - Lead Time for Changes: from commit to production
   - Change Failure Rate: percentage of deployments causing incidents
   - Mean Time to Recovery (MTTR): average time to restore service after incident

2. Technical Metrics Dashboard (table: service | metric | current | baseline | delta | status)
   - Cover: latency p50/p95/p99, error rate, availability, throughput, resource utilization

3. Alert Configuration (table: alert name | metric | threshold | severity | notification channel | current status: ACTIVE/FIRING/RESOLVED)

4. Trend Analysis (for each key metric: is it improving, stable, or degrading over the analysis period? identify patterns)

5. Postmortem (only if incident logs are provided):
   - Incident Timeline (chronological events from detection to resolution)
   - Root Cause Analysis (5 Whys method)
   - Impact Assessment (users affected, duration, business impact)
   - Contributing Factors
   - Lessons Learned
   - Action Items (owner | action | due date | priority)

6. Continuous Improvement Recommendations (prioritized by impact: HIGH/MEDIUM/LOW; each recommendation must reference a specific metric or observation)

Rules:
- Every recommendation must reference a specific metric or observed pattern.
- DORA metrics must be compared to industry benchmarks (Elite, High, Medium, Low performer levels).
- Postmortem must be blameless: focus on systems and processes, not individuals.
- Alerts must have defined thresholds, not vague conditions.
- Write in the language of the input.
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O pacote contém todas as seções obrigatórias (postmortem somente se logs de incidente foram fornecidos)
- [ ] As 4 métricas DORA estão presentes com valores, metas e status
- [ ] Cada alerta tem threshold numérico definido e canal de notificação especificado
- [ ] A análise de tendências cobre pelo menos as métricas de latência e taxa de erro
- [ ] As recomendações de melhoria estão priorizadas e referenciam métricas específicas
- [ ] O postmortem, quando presente, utiliza o método dos 5 Porquês e é escrito de forma não punitiva

## Boas práticas verificadas

- [ ] **Observabilidade:** métrica central desta skill; DORA, latência, error rate, disponibilidade e throughput cobertos; alertas com thresholds definidos
- [ ] **Governança:** postmortem documentado com ações de melhoria e responsáveis; DORA comparado a benchmarks da indústria
- [ ] **Documentação Markdown:** dashboard em tabelas, postmortem em seções cronológicas, ações com dono e prazo
