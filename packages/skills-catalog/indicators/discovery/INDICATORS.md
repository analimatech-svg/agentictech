# Indicadores: Fase Discovery

## Objetivo

Medir a qualidade e a cobertura do processo de descoberta, garantindo que as hipóteses foram testadas com usuários reais, que as decisões estão baseadas em evidências e que as personas representam os segmentos de usuário relevantes para o produto.

## Indicadores

| Indicador | Descrição | Fórmula | Meta sugerida |
|---|---|---|---|
| Cobertura de entrevistas | Percentual de entrevistas realizadas em relação às planejadas | (Entrevistas realizadas / Entrevistas planejadas) x 100 | 100% |
| Taxa de hipóteses validadas | Percentual de hipóteses que foram confirmadas com evidências de usuário | (Hipóteses validadas / Total de hipóteses) x 100 | Sem meta fixa |
| Taxa de hipóteses invalidadas | Percentual de hipóteses que foram refutadas com evidências de usuário | (Hipóteses invalidadas / Total de hipóteses) x 100 | Positivo: sinal de discovery funcionando |
| Cobertura de personas por segmento | Percentual de segmentos de usuário relevantes que têm pelo menos uma persona documentada | (Segmentos com persona / Total de segmentos identificados) x 100 | 100% |

## Detalhamento dos Indicadores

### Cobertura de entrevistas

Entrevistas canceladas ou não realizadas deixam lacunas de evidência que são preenchidas por suposições do time, aumentando o risco de construir algo que o usuário não precisa.

**Como medir:** registrar o número de entrevistas planejadas no início da fase. Ao fechar a fase, contar as entrevistas efetivamente realizadas com usuários representativos do perfil definido. Entrevistas com participantes fora do perfil não contam para o numerador.

**Sinal de alerta:** cobertura abaixo de 80% indica que as conclusões da fase de discovery têm base amostral insuficiente.

### Taxa de hipóteses validadas

Indica o percentual de suposições que o time tinha sobre o produto, o usuário ou o mercado e que foram confirmadas por evidências obtidas em campo. Não é um indicador de sucesso isolado: uma taxa de 100% pode significar que as hipóteses eram óbvias demais ou que o processo não testou hipóteses arriscadas.

**Como medir:** registrar todas as hipóteses no início da fase em um mapa de hipóteses. Para cada uma, registrar o resultado ao fim das entrevistas: validada, invalidada ou inconclusiva.

### Taxa de hipóteses invalidadas

Uma taxa de hipóteses invalidadas alta é sinal positivo: o processo de discovery funcionou e evitou que o time construísse funcionalidades desnecessárias. Projetos que chegam ao final da fase sem nenhuma hipótese invalidada quase sempre não testaram hipóteses arriscadas o suficiente.

**Como medir:** mesmo processo do indicador anterior. Registrar hipóteses invalidadas separadamente. Uma hipótese é invalidada quando a evidência de campo contradiz diretamente a suposição original.

**Sinal de alerta:** taxa de 0% de hipóteses invalidadas ao final da fase é sinal de que as hipóteses testadas eram seguras demais ou que o processo não foi rigoroso.

### Cobertura de personas por segmento

Personas que não representam todos os segmentos relevantes geram produtos que servem bem a um perfil e mal a outros. A cobertura garante que o time de design e desenvolvimento tenha contexto suficiente para tomar decisões para cada grupo de usuário.

**Como medir:** identificar todos os segmentos de usuário relevantes para o produto (ex: administradores, usuários finais iniciantes, usuários avançados, usuários móveis). Para cada segmento, verificar se existe ao menos uma persona documentada com necessidades, comportamentos e contexto de uso.

**Sinal de alerta:** segmento sem persona documentada ao fechar a fase indica risco de o design ignorar as necessidades desse grupo.

## Referências SDLC

- Fase: Discovery
- Entradas típicas: objetivos de negócio, lista de hipóteses, lista de segmentos de usuário
- Saídas esperadas: mapa de hipóteses com resultados, personas por segmento, insights de entrevistas, backlog inicial de oportunidades
