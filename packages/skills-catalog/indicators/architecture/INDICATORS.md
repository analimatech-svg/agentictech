# Indicadores: Fase Architecture

## Objetivo

Medir a qualidade das decisões de arquitetura, garantindo que as escolhas relevantes foram registradas em ADRs, que os componentes têm responsabilidades claras, que as dependências externas estão mapeadas por camada e que as decisões de segurança foram explicitamente cobertas.

## Indicadores

| Indicador | Descrição | Fórmula | Meta sugerida |
|---|---|---|---|
| Número de ADRs criados | Total de Architecture Decision Records formalizados durante a fase | Contagem absoluta | Mínimo 1 por decisão técnica relevante e irreversível |
| Cobertura de SRP documentada | Percentual de componentes da arquitetura que têm responsabilidade única documentada | (Componentes com SRP documentada / Total de componentes) x 100 | 100% dos componentes principais |
| Dependências externas por camada | Número de dependências externas (bibliotecas, serviços de terceiros, APIs) por camada da arquitetura | Contagem por camada | Camadas internas devem ter 0 dependências externas diretas |
| Cobertura de decisões de segurança | Percentual de decisões de arquitetura com implicações de segurança que foram explicitamente tratadas em ADR ou registro equivalente | (Decisões de segurança documentadas / Decisões com impacto de segurança) x 100 | 100% |

## Detalhamento dos Indicadores

### Número de ADRs criados

Architecture Decision Records registram o contexto, a decisão tomada, as alternativas consideradas e as consequências esperadas. Sem esse registro, o time perde o raciocínio por trás de cada escolha e futuras alterações são feitas sem entender os efeitos colaterais.

**Como medir:** contar todos os ADRs formalizados durante a fase. Um ADR é considerado completo quando tem: título, status, contexto, decisão, alternativas consideradas e consequências. Decisões técnicas relevantes que não têm ADR associado devem ser identificadas como lacuna.

**O que conta como decisão relevante:** escolha de banco de dados, padrão de autenticação, framework principal, estratégia de cache, protocolo de comunicação entre serviços, estratégia de deploy.

**Sinal de alerta:** mais de três decisões relevantes e irreversíveis sem ADR ao fechar a fase indica risco de perda de raciocínio arquitetural no médio prazo.

### Cobertura de SRP documentada (Single Responsibility Principle)

Componentes com responsabilidades mal definidas tendem a crescer e absorver lógica de outros contextos, tornando o sistema difícil de testar, manter e evoluir. Documentar a responsabilidade de cada componente força a clareza sobre seus limites.

**Como medir:** listar todos os componentes principais da arquitetura (módulos, serviços, camadas, bounded contexts). Para cada um, verificar se existe descrição explícita de sua única responsabilidade e de o que está fora do seu escopo. Componentes com descrição vaga ou múltiplas responsabilidades não contam como cobertos.

**Sinal de alerta:** componentes descritos com "e" na definição de responsabilidade (ex: "gerencia usuários e envia emails") indicam violação de SRP que vai gerar acoplamento.

### Dependências externas por camada

Dependências externas introduzidas nas camadas internas (domínio, aplicação) criam acoplamento que torna o sistema difícil de testar e de migrar. Dependências externas devem estar concentradas nas camadas de infraestrutura e adaptadores.

**Como medir:** mapear a arquitetura em camadas. Para cada camada, listar todas as dependências externas (pacotes de terceiros, chamadas a APIs externas, SDKs). Dependências externas presentes em camadas de domínio ou aplicação devem ser registradas como violação.

**Meta de referência:** camada de domínio com 0 dependências externas diretas. Camada de aplicação com no máximo dependências de contratos (interfaces), nunca de implementações externas.

**Sinal de alerta:** qualquer dependência de biblioteca externa importada diretamente na camada de domínio é violação de arquitetura que precisa ser resolvida antes de iniciar o desenvolvimento.

### Cobertura de decisões de segurança

Decisões arquiteturais com impacto de segurança (autenticação, autorização, criptografia, armazenamento de dados sensíveis, comunicação entre serviços) que não são explicitamente tratadas criam vulnerabilidades por omissão: não foi uma decisão de aceitar o risco, foi um esquecimento.

**Como medir:** revisar todos os ADRs e diagramas da fase. Identificar todas as decisões com implicações de segurança. Para cada uma, verificar se existe registro explícito da abordagem de segurança adotada, das alternativas consideradas e dos riscos residuais aceitos.

**Sinal de alerta:** qualquer fluxo de dados sensíveis (credenciais, dados pessoais, dados financeiros) sem decisão de segurança documentada é vulnerabilidade conhecida sendo carregada para o desenvolvimento.

## Referências SDLC

- Fase: Architecture
- Entradas típicas: requisitos funcionais e não funcionais, restrições de tecnologia, requisitos de segurança e conformidade
- Saídas esperadas: ADRs formalizados, diagrama de componentes com responsabilidades, mapa de dependências por camada, decisões de segurança documentadas
