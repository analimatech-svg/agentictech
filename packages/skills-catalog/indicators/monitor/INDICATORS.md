# Indicadores: Fase Monitor

## Objetivo

Medir a saúde operacional do sistema em produção, cobrindo velocidade de recuperação de incidentes, disponibilidade, qualidade da experiência do usuário e sustentabilidade da operação ao longo do tempo.

## Indicadores

| Indicador | Descrição | Fórmula | Meta sugerida |
|---|---|---|---|
| MTTR (Mean Time To Recovery) | Tempo médio entre a detecção de uma falha e a restauração completa do serviço | Soma dos tempos de recuperação / Número de incidentes | Elite DORA: abaixo de 1 hora; Alto: abaixo de 1 dia |
| Disponibilidade do sistema | Percentual do tempo em que o sistema está operacional e acessível aos usuários | (Tempo disponível / Tempo total do período) x 100 | 99,9% (três noves) ou conforme SLA contratado |
| Taxa de erro em produção | Percentual de requisições que resultam em erro (HTTP 5xx ou exceção não tratada) | (Requisições com erro / Total de requisições) x 100 | Abaixo de 1% em condições normais |
| Número de incidentes por mês | Total de incidentes registrados no mês, classificados por severidade | Contagem absoluta por severidade no período | Meta definida por baseline histórico e tendência de queda |
| MTBF (Mean Time Between Failures) | Tempo médio entre falhas consecutivas do sistema | Soma dos tempos entre falhas / Número de intervalos | Quanto maior, melhor; acompanhar tendência de crescimento |
| Net Promoter Score técnico | Satisfação do time de engenharia com a experiência de operar o sistema, medida em pesquisa periódica | Escala NPS adaptada: (Promotores - Detratores) / Total de respondentes x 100 | Acima de 30; acompanhar tendência ao longo do tempo |

## Detalhamento dos Indicadores

### MTTR (Mean Time To Recovery)

O MTTR é o quarto indicador central do framework DORA e mede a resiliência operacional do time. Um MTTR baixo indica que o time tem bons processos de detecção, diagnóstico e recuperação de incidentes, e que o sistema foi projetado com recuperabilidade em mente (rollback rápido, circuit breakers, failover automático).

**Como medir:** registrar cada incidente com timestamp de início (quando o sistema falhou ou o alerta foi gerado) e timestamp de recuperação (quando o serviço foi restaurado ao funcionamento normal para todos os usuários). O MTTR é a média dos tempos de recuperação no período.

**Componentes do tempo de recuperação:** tempo de detecção (alerta gerado) + tempo de diagnóstico (causa identificada) + tempo de correção (fix ou rollback aplicado) + tempo de verificação (confirmação de restauração).

**Referência DORA:**
- Elite: MTTR abaixo de 1 hora
- Alto: MTTR abaixo de 1 dia
- Médio: MTTR entre 1 dia e 1 semana
- Baixo: MTTR acima de 1 semana

**Sinal de alerta:** MTTR crescente ao longo dos meses indica deterioração dos processos operacionais ou aumento da complexidade do sistema sem investimento correspondente em observabilidade e runbooks.

### Disponibilidade do sistema

A disponibilidade é o percentual do tempo em que o sistema está operacional e acessível. O valor de 99,9% (três noves) corresponde a aproximadamente 8,7 horas de indisponibilidade por ano, que é o nível mínimo esperado para sistemas de uso profissional. O SLA contratado define a meta formal; a disponibilidade real deve ficar acima dela.

**Como medir:** somar os períodos de indisponibilidade (incidentes confirmados onde o serviço estava inacessível ou com degradação severa) no mês. Calcular o percentual do tempo em que o sistema estava disponível sobre o tempo total do mês.

**O que conta como indisponibilidade:** inacessibilidade total, taxa de erro acima de 50% por mais de 5 minutos, latência acima de 3x o baseline por mais de 10 minutos.

**Sinal de alerta:** disponibilidade abaixo do SLA contratado por dois meses consecutivos indica problema sistêmico que precisa ser tratado como projeto de confiabilidade, não como incidente isolado.

### Taxa de erro em produção

A taxa de erro mede a proporção de requisições que terminam com falha do sistema. Uma taxa alta impacta diretamente a experiência do usuário e é o sinal mais precoce de degradação sistêmica. A meta de abaixo de 1% é referência para sistemas maduros em condições normais; estabelecer o baseline dos primeiros meses de operação antes de definir a meta específica.

**Como medir:** configurar métricas de taxa de erro no sistema de monitoramento (Datadog, New Relic, Prometheus + Grafana ou equivalente). Medir a taxa de erros HTTP 5xx sobre o total de requisições. Medir separadamente a taxa de exceções não tratadas no backend. Calcular a taxa média e o percentil 99 para cada endpoint crítico.

**Sinal de alerta:** pico de taxa de erro acima de 5% por mais de 2 minutos deve gerar alerta crítico automático. Taxa persistentemente acima de 1% por mais de 30 minutos indica problema que precisa de intervenção imediata.

### Número de incidentes por mês

A contagem de incidentes por mês, separada por severidade, é o termômetro da saúde operacional do sistema. A meta não é uma quantidade fixa, mas uma tendência de queda mês a mês à medida que as ações corretivas dos postmortems são implementadas.

**Definição de severidade:**
- Crítico (P0): sistema inacessível ou dados corrompidos para todos os usuários.
- Alto (P1): funcionalidade principal indisponível para a maioria dos usuários.
- Médio (P2): funcionalidade comprometida com impacto parcial.
- Baixo (P3): degradação de performance ou problema cosmético sem impacto funcional.

**Como medir:** registrar cada incidente no sistema de gestão de incidentes com categoria de severidade, timestamp de início, timestamp de recuperação e link para o postmortem (para incidentes P0 e P1). Gerar relatório mensal por severidade e acompanhar a tendência.

**Sinal de alerta:** aumento no número de incidentes P0 ou P1 por dois meses consecutivos indica regressão na confiabilidade que precisa de investigação como processo.

### MTBF (Mean Time Between Failures)

O MTBF mede o tempo médio que o sistema opera sem falhas. Quanto maior o MTBF, mais confiável o sistema. O acompanhamento da tendência é mais importante que o valor absoluto: um MTBF crescente indica que as ações de melhoria estão funcionando; um MTBF decrescente indica degradação da confiabilidade.

**Como medir:** registrar o tempo entre o término de um incidente e o início do próximo. O MTBF é a média desses intervalos no período. Para sistemas novos, os primeiros meses estabelecem o baseline. A meta de melhoria é um MTBF percentualmente maior a cada trimestre.

**Relação com MTTR:** MTBF e MTTR são complementares. Um sistema com MTBF alto e MTTR baixo é o objetivo: falha raramente e se recupera rapidamente quando falha.

**Sinal de alerta:** MTBF decrescente por três meses consecutivos, mesmo com MTTR estável, indica aumento na frequência de falhas que precisa ser investigado nas camadas de código, infraestrutura e dependências externas.

### Net Promoter Score técnico (NPS técnico)

O NPS técnico mede a satisfação do time de engenharia com a experiência de operar e manter o sistema. Times insatisfeitos com a operação desenvolvem mais lentamente, cometem mais erros e têm maior rotatividade. A pergunta central da pesquisa é: "Em uma escala de 0 a 10, o quanto você recomendaria trabalhar na operação deste sistema para um colega de time?"

**Como medir:** aplicar pesquisa anônima ao time de engenharia, devops e suporte de primeiro nível a cada trimestre. Calcular o NPS: percentual de promotores (notas 9 e 10) menos percentual de detratores (notas 0 a 6), multiplicado por 100.

**O que investigar junto com a nota:** coletar comentários qualitativos sobre os principais pontos de atrito operacional (processos manuais repetitivos, alertas com muito ruído, runbooks desatualizados, falta de observabilidade). Esses pontos alimentam o backlog de melhoria de engenharia.

**Referência:** NPS técnico acima de 30 é considerado saudável. Abaixo de 0 indica que o time está mais insatisfeito do que satisfeito com a operação, o que é sinal de risco de burnout e deterioração da qualidade da entrega.

**Sinal de alerta:** NPS técnico abaixo de 0 por dois trimestres consecutivos deve acionar uma retrospectiva focada exclusivamente em melhoria da experiência operacional do time.

## Referências SDLC

- Fase: Monitor
- Entradas típicas: sistema em produção, registros de incidentes, alertas de monitoramento, postmortems, pesquisa de satisfação do time
- Saídas esperadas: relatório mensal de indicadores operacionais, ações corretivas priorizadas no backlog, postmortems publicados, runbooks atualizados
- Referências: DORA State of DevOps Report, SRE Book (Google), ISO/IEC 25010 (confiabilidade de software)
