# Indicadores: Fase Builder

## Objetivo

Medir a qualidade técnica do código produzido e a eficiência do processo de desenvolvimento, combinando métricas de qualidade interna (cobertura de testes, complexidade, lint) com métricas DORA de velocidade de entrega.

## Indicadores

| Indicador | Descrição | Fórmula | Meta sugerida |
|---|---|---|---|
| Cobertura de testes unitários | Percentual de linhas ou branches de código cobertos por testes unitários | (Linhas cobertas / Total de linhas executáveis) x 100 | Mínimo 80% |
| Complexidade ciclomática média | Número médio de caminhos de execução independentes por módulo ou função | Soma da complexidade de todos os módulos / Número de módulos | Meta: abaixo de 10 por função |
| Violações de lint por commit | Número médio de violações de regras de lint introduzidas por commit | Total de violações no período / Total de commits no período | Meta: 0 violações por commit |
| Deployment Frequency (DORA) | Com que frequência o time coloca código em produção (ou no ambiente de staging equivalente) | Deploys por dia, semana ou mês | Elite: múltiplos por dia; Alto: diário a semanal |
| Lead Time for Changes (DORA) | Tempo desde o primeiro commit de uma mudança até ela estar em produção | Mediana do tempo entre primeiro commit e deploy de produção | Elite: menos de 1 hora; Alto: 1 dia a 1 semana |

## Detalhamento dos Indicadores

### Cobertura de testes unitários

Código sem testes unitários é código que o time tem medo de alterar. A cobertura não garante qualidade dos testes por si só, mas é o pré-requisito para ter uma rede de segurança mínima durante refatorações e adições de funcionalidade.

**Como medir:** configurar a ferramenta de cobertura do ecossistema (Jest para JavaScript/TypeScript, pytest-cov para Python, JaCoCo para Java, go test -cover para Go). Medir cobertura de linhas e de branches. Integrar ao pipeline de CI com gate de qualidade que bloqueia merge abaixo da meta.

**O que NÃO mede:** qualidade dos testes. É possível ter 100% de cobertura com testes que não verificam o comportamento correto. Revisar a qualidade dos casos de teste nas revisões de código.

**Sinal de alerta:** queda de cobertura entre sprints indica que novas funcionalidades estão sendo adicionadas sem cobertura correspondente.

### Complexidade ciclomática média

A complexidade ciclomática mede o número de caminhos de execução independentes em uma função ou módulo. Funções de alta complexidade são difíceis de testar, difíceis de ler e propensas a bugs em casos de borda.

**Como medir:** usar ferramentas de análise estática integradas ao pipeline (ESLint com plugin de complexidade para JS/TS, Radon para Python, SonarQube para múltiplas linguagens). Medir por função ou método. A complexidade média por módulo é a média das complexidades de todas as funções do módulo.

**Referência:** complexidade abaixo de 10 por função é aceitável. Entre 10 e 20: candidata a refatoração. Acima de 20: refatoração obrigatória antes de merge.

**Sinal de alerta:** aumento da complexidade média ao longo das sprints indica que o código está ficando difícil de manter, independentemente da cobertura de testes.

### Violações de lint por commit

Violações de lint indicam que o código não segue os padrões acordados pelo time. Cada violação acumulada é dívida técnica que vai precisar ser paga. A meta de zero violações por commit é atingível quando o lint está configurado como gate de CI e como hook de pré-commit na máquina dos desenvolvedores.

**Como medir:** configurar lint no pipeline de CI para rodar em cada push. Registrar o número de violações por commit ao longo do tempo. Violações existentes antes da implantação do indicador podem ser registradas como dívida técnica e tratadas em sprint dedicada.

**Sinal de alerta:** aumento de violações após um período de estabilidade indica que o processo de revisão ou os hooks de pre-commit foram desabilitados ou contornados.

### Deployment Frequency (DORA)

A frequência de deploy mede a capacidade do time de entregar valor em incrementos pequenos. Times com alta frequência de deploy têm menor risco por mudança e maior capacidade de resposta a feedback do usuário.

**Como medir:** registrar cada deploy em produção (ou no ambiente de referência). Calcular a frequência média por semana ou por mês. Usar a classificação DORA: Elite (múltiplos por dia), Alto (entre diário e semanal), Médio (entre semanal e mensal), Baixo (menos de uma vez por mês).

**Contexto:** para projetos em fase inicial de desenvolvimento, o ambiente de referência pode ser staging. O importante é que o pipeline de deploy esteja sendo exercitado regularmente.

### Lead Time for Changes (DORA)

O lead time mede a velocidade do fluxo de desenvolvimento: do momento em que um desenvolvedor escreve o primeiro commit de uma mudança até essa mudança estar disponível em produção. Tempos altos indicam gargalos no processo de revisão, testes ou aprovações.

**Como medir:** registrar o timestamp do primeiro commit associado a cada mudança (feature, bug fix, melhoria). Registrar o timestamp do deploy de produção correspondente. A diferença é o lead time de cada mudança. Calcular a mediana (não a média, para evitar distorção por outliers).

**Sinal de alerta:** lead time crescente ao longo das sprints indica acúmulo de trabalho em progresso, processo de revisão lento ou pipeline de CI com tempo de execução alto.

## Referências SDLC

- Fase: Builder
- Entradas típicas: User Stories com critérios Gherkin, wireframes aprovados, ADRs de arquitetura
- Saídas esperadas: código com cobertura de testes, pipeline de CI funcionando, deploys frequentes em ambiente de staging ou produção
- Referência DORA: State of DevOps Report (Google / DORA Research Program)
