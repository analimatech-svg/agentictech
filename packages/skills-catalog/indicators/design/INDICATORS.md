# Indicadores: Fase Design

## Objetivo

Medir a qualidade e a completude do trabalho de design, garantindo que as User Stories têm critérios de aceite em formato Gherkin, que as telas foram aprovadas pelos stakeholders, que o protótipo convergiu com o mínimo de iterações e que os feedbacks de usuário foram endereçados.

## Indicadores

| Indicador | Descrição | Fórmula | Meta sugerida |
|---|---|---|---|
| Cobertura Gherkin | Percentual de User Stories com pelo menos um critério de aceite em formato Gherkin (Given / When / Then) | (USs com Gherkin / Total de USs) x 100 | 100% |
| Cobertura de wireframes aprovados | Percentual de telas previstas no escopo que têm wireframe aprovado pelos stakeholders | (Telas com wireframe aprovado / Total de telas previstas) x 100 | 100% antes de iniciar desenvolvimento |
| Iterações por protótipo | Número de rodadas de revisão realizadas no protótipo até aprovação final | Contagem de iterações por fluxo ou componente | Meta: máx. 3 iterações |
| Cobertura de feedbacks endereçados | Percentual de feedbacks de usuário coletados em testes de usabilidade que foram tratados antes da aprovação final | (Feedbacks endereçados / Total de feedbacks recebidos) x 100 | 100% dos críticos e altos; 80% dos demais |

## Detalhamento dos Indicadores

### Cobertura Gherkin

Critérios de aceite em Gherkin (Given / When / Then) são testáveis por definição: eliminam ambiguidade sobre o que o sistema deve fazer e permitem que os testes automatizados sejam escritos antes da implementação. USs sem Gherkin chegam ao desenvolvimento com escopo vago e geram retrabalho.

**Como medir:** contar o total de User Stories no backlog da fase de design. Para cada uma, verificar se existe pelo menos um cenário em formato Gherkin completo (Given, When e Then presentes). USs em formato de lista de requisitos sem Gherkin não contam como cobertas.

**Sinal de alerta:** cobertura abaixo de 100% ao iniciar o desenvolvimento indica que parte do escopo não tem definição testável.

### Cobertura de wireframes aprovados

Wireframes não aprovados entram no desenvolvimento como suposição do time. A aprovação formal registra que o stakeholder viu, entendeu e concordou com a proposta de interface antes de qualquer linha de código ser escrita.

**Como medir:** mapear todas as telas previstas no escopo. Para cada tela, verificar se existe wireframe aprovado com assinatura ou registro de aprovação (comentário em ferramenta de design, ata, email). Telas em discussão não contam como aprovadas.

**Sinal de alerta:** qualquer tela principal sem wireframe aprovado ao iniciar o desenvolvimento é risco de retrabalho de implementação.

### Iterações por protótipo

Um número alto de iterações indica que o briefing inicial estava incompleto, que os stakeholders não estavam alinhados entre si ou que o processo de feedback não foi estruturado. A meta de até três iterações não é rígida para todos os componentes, mas serve como referência para identificar fluxos problemáticos.

**Como medir:** registrar cada rodada de revisão por fluxo ou componente do protótipo. Uma iteração é contada quando o designer entrega para revisão e recebe feedback que gera alteração. A contagem termina na aprovação final.

**Sinal de alerta:** mais de cinco iterações num único fluxo indica problema de alinhamento que precisa ser resolvido antes de avançar.

### Cobertura de feedbacks endereçados

Feedbacks de testes de usabilidade não tratados chegam ao produto como problemas de experiência que o usuário vai encontrar em produção. Nem todo feedback precisa gerar alteração, mas todos precisam de uma decisão documentada: tratar, descartar com justificativa ou priorizar para versão futura.

**Como medir:** registrar todos os feedbacks recebidos em sessões de usabilidade. Classificar por severidade (crítico, alto, médio, baixo). Ao fechar a fase, verificar quais foram tratados, quais foram descartados com justificativa e quais foram adiados com registro. Feedbacks críticos e altos devem ter 100% de tratamento ou descarte justificado.

**Sinal de alerta:** feedbacks críticos sem decisão ao fechar a fase indicam que o produto vai para o desenvolvimento com problemas conhecidos de usabilidade não resolvidos.

## Referências SDLC

- Fase: Design
- Entradas típicas: personas, mapa de hipóteses validadas, requisitos priorizados
- Saídas esperadas: User Stories com Gherkin, wireframes aprovados, protótipo iterado e aprovado, registro de feedbacks endereçados
