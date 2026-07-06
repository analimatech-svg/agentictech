# Indicadores: Fase Deploy

## Objetivo

Medir a eficiência e a confiabilidade do processo de deploy, cobrindo frequência de entrega, tempo total do pipeline, taxa de deploys sem rollback e velocidade de detecção de falhas após o deploy.

## Indicadores

| Indicador | Descrição | Fórmula | Meta sugerida |
|---|---|---|---|
| Deployment Frequency (DORA) | Com que frequência o time realiza deploys em produção | Deploys por dia, semana ou mês | Elite: múltiplos por dia; Alto: diário a semanal |
| Tempo de deploy | Tempo total desde o commit que disparou o pipeline até o código estar disponível em produção | Mediana do tempo entre trigger do pipeline e conclusão do deploy de produção | Elite: abaixo de 1 hora; Alto: abaixo de 1 dia |
| Taxa de deploys sem rollback | Percentual de deploys que não precisaram de rollback ou hotfix imediato | (Deploys sem rollback / Total de deploys) x 100 | Mínimo 95% (meta Elite DORA: acima de 95%) |
| Tempo de detecção de falha pós-deploy | Tempo entre o deploy e o momento em que a equipe detecta que algo falhou em produção | Mediana do tempo entre deploy e abertura do primeiro alerta ou incidente relacionado | Meta: abaixo de 15 minutos |

## Detalhamento dos Indicadores

### Deployment Frequency (DORA)

A frequência de deploy é um dos quatro indicadores centrais do framework DORA para medir a performance de times de engenharia. Times com alta frequência de deploy têm menor risco por mudança (cada deploy contém menos alterações), mais oportunidades de feedback do usuário e maior capacidade de resposta a problemas.

**Como medir:** registrar cada deploy em produção com timestamp. Calcular a frequência média no período. Usar a classificação DORA:
- Elite: múltiplos deploys por dia
- Alto: entre diário e semanal
- Médio: entre semanal e mensal
- Baixo: menos de uma vez por mês

**Contexto de fase:** durante a fase de deploy inicial de um produto, a frequência pode ser baixa enquanto o processo está sendo estabelecido. O importante é que o processo seja automatizado e repetível desde o início.

**Sinal de alerta:** frequência decrescente ao longo do tempo indica acúmulo de mudanças entre deploys, o que aumenta o risco e a complexidade de cada entrega.

### Tempo de deploy

O tempo total de deploy inclui: execução do pipeline de CI (testes, build, análise estática), publicação do artefato, provisionamento de infraestrutura (quando aplicável) e ativação em produção. Tempos altos indicam gargalos que tornam o processo lento e custoso.

**Como medir:** registrar o tempo de início do pipeline (trigger) e o tempo de conclusão do deploy em produção para cada deploy. Calcular a mediana dos tempos no período. Usar mediana em vez de média para evitar distorção por outliers (deploys com problemas que demoraram muito mais que o normal).

**Referência DORA:**
- Elite: menos de 1 hora
- Alto: entre 1 dia e 1 semana
- Médio: entre 1 semana e 1 mês
- Baixo: mais de 1 mês

**Sinal de alerta:** aumento progressivo no tempo de deploy indica acúmulo de testes lentos, steps desnecessários no pipeline ou crescimento não controlado do artefato de build.

### Taxa de deploys sem rollback

Mede a proporção de deploys que chegaram a produção sem precisar ser revertidos. Um rollback é sempre um sinal de que algo passou pelo processo de validação que não deveria ter passado, seja um bug, uma incompatibilidade de configuração ou uma dependência não testada.

**Como medir:** registrar cada deploy e marcar se resultou em rollback nas primeiras 24 horas. Calcular o percentual de deploys sem rollback sobre o total de deploys do período.

**O que conta como rollback:** reversão manual para versão anterior, execução de pipeline de rollback automatizado, ou hotfix de emergência aplicado em menos de 2 horas após o deploy para corrigir falha crítica introduzida por ele.

**Sinal de alerta:** taxa abaixo de 90% indica problemas sistêmicos no processo de validação que precisam ser investigados como processo, não tratados caso a caso.

### Tempo de detecção de falha pós-deploy

O tempo entre o deploy e a detecção de um problema em produção é crítico: quanto mais tempo leva para detectar, mais usuários são impactados e mais difícil fica isolar a causa. A meta de detecção em menos de 15 minutos é atingível com monitoramento e alertas bem configurados.

**Como medir:** para cada deploy que resultou em incidente, registrar o timestamp do deploy e o timestamp da abertura do primeiro alerta ou incidente relacionado. Calcular a mediana dos tempos de detecção.

**Pré-requisito:** monitoramento ativo com alertas configurados para métricas de erro (taxa de erro HTTP, exceções não tratadas, latência acima do threshold) e métricas de negócio (queda em transações, conversões ou uso de funcionalidades críticas).

**Sinal de alerta:** tempo de detecção acima de 1 hora indica que o monitoramento está insuficiente ou que os alertas não estão calibrados para detectar os tipos de falha mais comuns no sistema.

## Referências SDLC

- Fase: Deploy
- Entradas típicas: artefato de build validado pela fase Validator, configurações de ambiente, scripts de migração de banco de dados
- Saídas esperadas: sistema em produção, pipeline de deploy documentado, alertas de monitoramento configurados, runbook de rollback disponível
- Referência DORA: State of DevOps Report (Google / DORA Research Program)
