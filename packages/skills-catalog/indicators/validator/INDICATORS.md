# Indicadores: Fase Validator

## Objetivo

Medir a qualidade do processo de validação e a confiabilidade do produto antes de ir para produção, cobrindo cobertura de testes automatizados, taxa de aprovação dos casos de teste, volume e severidade de bugs, taxa de falha em produção após deploy e rastreabilidade dos critérios Gherkin.

## Indicadores

| Indicador | Descrição | Fórmula | Meta sugerida |
|---|---|---|---|
| Taxa de casos de teste passando | Percentual de casos de teste que estão passando no momento da avaliação | (Casos passando / Total de casos) x 100 | 100% ao iniciar o processo de aprovação para deploy |
| Cobertura de código total | Percentual de código coberto pelo conjunto completo de testes (unitários + integração + e2e) | (Linhas cobertas / Total de linhas executáveis) x 100 | Mínimo 80% |
| Bugs por severidade | Número de bugs abertos classificados por severidade: crítico, alto, médio, baixo | Contagem absoluta por categoria | 0 críticos e 0 altos ao iniciar aprovação para deploy |
| Change Failure Rate (DORA) | Percentual de mudanças que resultam em falha em produção (incidente, rollback ou hotfix necessário) | (Deploys com falha / Total de deploys) x 100 | Elite: abaixo de 5%; Alto: entre 5% e 10% |
| Cobertura de critérios Gherkin por testes automatizados | Percentual de critérios de aceite escritos em Gherkin que têm pelo menos um teste automatizado correspondente | (Cenários Gherkin com teste / Total de cenários Gherkin) x 100 | 100% dos cenários críticos; 80% do total |

## Detalhamento dos Indicadores

### Taxa de casos de teste passando

Um caso de teste falhando é um comportamento esperado que o sistema não está entregando. A meta de 100% ao iniciar o processo de aprovação para deploy não significa que todos os testes precisam passar durante o desenvolvimento, mas sim que nenhum comportamento crítico esperado pode estar quebrado ao colocar o produto em produção.

**Como medir:** integrar o resultado dos testes ao pipeline de CI. Registrar a taxa por suite (unitário, integração, e2e) e no conjunto total. Casos marcados como "skip" sem justificativa devem ser contabilizados como falha para fins deste indicador.

**Sinal de alerta:** taxa abaixo de 95% ao entrar na fase de homologação indica que o produto vai para validação com comportamentos quebrados conhecidos, o que prolonga o ciclo de validação.

### Cobertura de código total

A cobertura de 80% como meta mínima é referência amplamente adotada na indústria para sistemas em produção. Abaixo desse número, a probabilidade de bugs não detectados em casos de borda cresce significativamente. A cobertura total combina testes unitários, de integração e end-to-end.

**Como medir:** configurar relatório de cobertura agregado no pipeline de CI. Ferramentas como Istanbul (JS), Coverage.py (Python) ou JaCoCo (Java) permitem combinar resultados de diferentes suites. O número reportado deve ser cobertura de branches, não apenas de linhas, para capturar caminhos condicionais.

**Sinal de alerta:** cobertura abaixo de 80% em módulos críticos (autenticação, pagamento, processamento de dados) é risco alto, independentemente da cobertura geral.

### Bugs por severidade

A classificação de severidade define a urgência do tratamento e os critérios de entrada em produção. A meta de zero bugs críticos e altos ao iniciar o processo de aprovação para deploy é o critério mínimo de qualidade.

**Definição por severidade:**
- Crítico: o sistema não funciona, dados são corrompidos ou há vulnerabilidade de segurança exploitável.
- Alto: funcionalidade principal comprometida sem workaround disponível.
- Médio: funcionalidade comprometida com workaround disponível.
- Baixo: problema cosmético ou de usabilidade que não impede o uso.

**Como medir:** registrar cada bug identificado com severidade no momento do registro. Revisar e ajustar severidade durante triagem. Bugs médios e baixos podem ir para produção com risco aceito pelo produto, desde que documentados.

**Sinal de alerta:** aumento no volume de bugs críticos ao longo do ciclo de validação indica que a fase de builder entregou código com qualidade abaixo do esperado.

### Change Failure Rate (DORA)

Mede a proporção de deploys que resultam em problema em produção, seja incidente, rollback ou necessidade de hotfix urgente. Uma taxa alta indica que o processo de validação não está detectando os problemas antes de chegar ao usuário.

**Como medir:** registrar cada deploy em produção. Para cada deploy, registrar se gerou incidente, rollback ou hotfix nos 24 horas seguintes. Calcular o percentual sobre o total de deploys no período.

**Referência DORA:** Elite (abaixo de 5%), Alto (entre 5% e 10%), Médio (entre 10% e 15%), Baixo (acima de 15%).

**Sinal de alerta:** taxa acima de 15% indica problemas sistêmicos no processo de validação ou na qualidade do desenvolvimento que precisam ser investigados como processo, não como exceção.

### Cobertura de critérios Gherkin por testes automatizados

Critérios de aceite escritos em Gherkin que não têm teste automatizado correspondente são critérios validados manualmente em cada ciclo de teste, o que torna o processo lento e propenso a regressões não detectadas.

**Como medir:** usar ferramentas de mapeamento de cenários Gherkin para testes (Cucumber, Behave, SpecFlow, Playwright com integração Gherkin). Gerar relatório de rastreabilidade que mostra quais cenários têm teste implementado e quais não têm.

**Meta diferenciada:** 100% dos cenários críticos (autenticação, fluxos de pagamento, funcionalidades principais) e 80% do total de cenários.

**Sinal de alerta:** cenários Gherkin sem teste automatizado nos fluxos críticos indicam que regressões nessas áreas só serão detectadas por testes manuais, o que aumenta o risco de bugs chegarem à produção.

## Referências SDLC

- Fase: Validator
- Entradas típicas: código entregue pela fase Builder, User Stories com Gherkin, plano de testes
- Saídas esperadas: relatório de cobertura, relatório de bugs por severidade, rastreabilidade Gherkin vs. testes, taxa de aprovação de casos de teste
- Referência DORA: State of DevOps Report (Google / DORA Research Program)
