# Indicadores: Fase Planning

## Objetivo

Medir a qualidade e a completude do trabalho de planejamento antes do início do desenvolvimento, garantindo que o escopo esteja claro, os stakeholders estejam mapeados e as restrições estejam documentadas.

## Indicadores

| Indicador | Descrição | Fórmula | Meta sugerida |
|---|---|---|---|
| Cobertura de critérios de aceite | Percentual de requisitos que têm pelo menos um critério de aceite definido | (Requisitos com critério de aceite / Total de requisitos) x 100 | 100% |
| Cobertura de stakeholders | Percentual de stakeholders identificados que foram formalmente mapeados com papel e nível de influência | (Stakeholders mapeados / Stakeholders identificados) x 100 | 100% |
| Cobertura de restrições | Percentual de restrições documentadas em relação ao total identificado em workshops e entrevistas | (Restrições documentadas / Restrições identificadas) x 100 | 100% |
| Aderência ao prazo de baseline | Diferença em dias entre a data planejada para fechamento do escopo (baseline) e a data real | Data real de fechamento menos data de baseline | Zero ou negativo |

## Detalhamento dos Indicadores

### Cobertura de critérios de aceite

Requisitos sem critério de aceite são fonte direta de retrabalho: o time desenvolve algo e o stakeholder diz que não é o que esperava. Essa métrica precisa chegar a 100% antes de qualquer trabalho de arquitetura ou desenvolvimento começar.

**Como medir:** contar todos os requisitos do backlog ou documento de escopo. Para cada um, verificar se existe pelo menos uma condição testável que define quando o requisito está satisfeito. Requisitos sem essa condição entram no numerador de cobertura incompleta.

**Sinal de alerta:** cobertura abaixo de 80% ao final da fase indica que a equipe está prestes a desenvolver com escopo ambíguo.

### Cobertura de stakeholders

Stakeholders não mapeados tomam decisões sem visibilidade do projeto ou são impactados sem aviso, gerando resistência e solicitações de mudança tardias. O mapeamento deve incluir papel, nível de influência e forma de envolvimento esperada.

**Como medir:** registrar todos os stakeholders identificados em reuniões de kick-off ou entrevistas. Comparar com os formalmente documentados no plano de stakeholders com papel e influência registrados.

**Sinal de alerta:** qualquer stakeholder com poder de veto não mapeado é risco crítico de projeto.

### Cobertura de restrições

Restrições não documentadas surgem como surpresas no meio do desenvolvimento. Exemplos: restrições regulatórias, limitações de orçamento, tecnologias que não podem ser usadas, prazo imutável por contrato.

**Como medir:** levantar restrições em workshops de escopo. Documentar cada uma com origem, impacto e responsável por monitorar. A cobertura mede quantas das restrições levantadas foram formalmente registradas.

**Sinal de alerta:** cobertura abaixo de 100% ao fechar a fase de planejamento é risco direto de retrabalho nas fases seguintes.

### Aderência ao prazo de baseline

Mede se o planejamento foi executado dentro do tempo previsto. Atrasos no planejamento comprimem as fases seguintes e aumentam a pressão sobre o time de desenvolvimento.

**Como medir:** registrar a data de fechamento planejada do escopo no início da fase. Registrar a data real ao fechar. A diferença em dias é o indicador.

**Sinal de alerta:** atraso superior a 20% do tempo planejado para a fase indica problemas de processo ou de engajamento dos stakeholders que precisam ser tratados antes de avançar.

## Referências SDLC

- Fase: Planning
- Entradas típicas: objetivos de negócio, restrições contratuais, lista preliminar de stakeholders
- Saídas esperadas: documento de escopo com critérios de aceite, mapa de stakeholders, registro de restrições, cronograma de baseline
