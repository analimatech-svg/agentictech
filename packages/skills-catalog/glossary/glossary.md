# Glossário Central — MaestroAI-Tech

> Fonte de verdade para toda nomenclatura usada na plataforma MaestroAI-Tech.
> Cada conceito tem um nome único. Termos sinônimos são proibidos para evitar confusão.

## Regra de uso

Antes de criar qualquer skill, documento ou pipeline: consulte este glossário.
Se um conceito não está aqui, registre antes de usar.

---

## A

**ADR (Architecture Decision Record)**
Documento que registra uma decisão de arquitetura relevante: contexto, opções consideradas, decisão tomada e consequências. Produzido pela skill `doc-adr`.

**Agente**
Componente de software que recebe uma tarefa, usa um LLM e ferramentas para executá-la e retorna um resultado. No contexto da MaestroAI-Tech, o Consultor de IA aprende a criar e dirigir agentes.

**API Key (Chave de API)**
Credencial de acesso a um provedor de LLM (Claude, GPT-4o, Gemini). Cada aluno conecta a própria chave. A plataforma não fornece nem armazena chaves.

**Arquitetura**
Fase do SDLC entre Design e Desenvolvimento. Define estrutura técnica, padrões, dependências e decisões de alto nível antes da implementação.

---

## B

**Business Case**
Documento que justifica a existência de um projeto: problema, oportunidade, custo, benefício esperado e alternativas descartadas. Produzido pela skill `doc-business-case`.

**BYOK (Bring Your Own Key)**
Modelo onde cada usuário conecta a própria chave de API do LLM escolhido. A MaestroAI-Tech opera exclusivamente nesse modelo.

---

## C

**Changelog**
Registro cronológico de todas as mudanças relevantes de um produto ou serviço, organizado por versão. Produzido pela skill `doc-changelog`.

**Clean Architecture**
Padrão de arquitetura que separa responsabilidades em camadas concêntricas (entidades, casos de uso, adaptadores, frameworks). Referência obrigatória nas skills de arquitetura e validação.

**Code Style Guide**
Documento que define convenções de codificação do time: nomenclatura, formatação, padrões de commit, regras de lint. Produzido pela skill `doc-code-style`.

**Consultor de IA**
Identidade do usuário da plataforma. Não é um executor de tarefas: estrutura o problema, dirige a IA e valida o resultado. O nome evoca autonomia técnica e responsabilidade pelo output.

**Context**
Pasta do repositório que armazena templates reutilizáveis de contexto para as skills. Contexto especializado é o que diferencia uma skill genérica de uma skill útil.

---

## D

**DDD (Domain-Driven Design)**
Abordagem de design de software centrada no domínio do negócio. Conceitos centrais: entidade, agregado, repositório, serviço de domínio, linguagem ubíqua. Referência obrigatória nas skills de arquitetura e design.

**Deploy Plan**
Documento que descreve como o software será publicado em produção: ambiente, sequência de passos, validações, responsáveis e critérios de rollback. Produzido pela skill `doc-deploy-plan`.

**Discovery**
Segunda fase do SDLC. Aprofunda o entendimento do problema, valida hipóteses do planejamento e define o escopo técnico antes do design.

**DORA Metrics**
Quatro métricas de performance de engenharia: Deployment Frequency, Lead Time for Changes, Change Failure Rate, MTTR. Rastreadas na fase Monitor.

---

## E

**Escala Maestro**
Sistema de progressão do Consultor de IA: Aprendiz, Praticante, Regente, Maestro. Cada nível representa mais autonomia conquistada. Ver `judge/maestro-scale.md`.

**Event-Driven Architecture**
Padrão onde componentes se comunicam via eventos assíncronos em vez de chamadas diretas. Referência obrigatória nas skills de arquitetura.

---

## G

**Gherkin**
Formato de critérios de aceite legível por humanos e máquinas. Estrutura: `Dado que... / Quando... / Então...`. Usado em todas as User Stories da plataforma.

**Glossário**
Este documento. Fonte de verdade para toda nomenclatura do projeto.

---

## J

**Judge**
Componente central de roteamento da MaestroAI-Tech. Interpreta input do Consultor de IA, identifica nível Maestro, seleciona skill ou pipeline e propõe antes de executar. Ver `judge/routing-rules.md`.

---

## K

**Knowledge Base**
Base de conhecimento interna do produto: artigos, tutoriais, FAQs, troubleshooting. Produzida pela skill `doc-knowledge-base`.

---

## L

**LLM (Large Language Model)**
Modelo de linguagem de grande escala usado como motor de inteligência das skills (Claude, GPT-4o, Gemini e futuramente modelos locais como Ollama).

**Lead Time for Changes**
Tempo entre o commit de uma mudança e sua chegada em produção. Uma das quatro DORA Metrics.

---

## M

**Maestro**
Quarto nível da Escala Maestro. Governa a própria stack agêntica, cria skills novas de forma autônoma. Ver `judge/maestro-scale.md`.

**Monitor**
Oitava fase do SDLC. Observa o sistema em produção: métricas, alertas, incidentes, retrospectivas e melhoria contínua.

**MTTR (Mean Time To Recovery)**
Tempo médio para restaurar o serviço após uma falha. Uma das quatro DORA Metrics.

---

## P

**Persona**
Documento que representa um tipo de usuário do sistema: perfil, necessidades, comportamentos, frustrações e objetivos. Produzido pela skill `doc-persona`.

**Pipeline**
Sequência de skills encadeadas para entregar um resultado end-to-end. Exemplos: `sdlc-completo`, `mvp-rapido`, `analise-codebase`.

**Planning**
Primeira fase do SDLC. Define objetivos, escopo inicial, stakeholders, restrições e critérios de sucesso antes de qualquer execução.

**Postmortem**
Análise estruturada de um incidente ou falha: o que aconteceu, impacto, causa raiz, ações corretivas. Produzido pela skill `doc-postmortem`.

---

## R

**RFC (Request for Comments)**
Documento que propõe uma mudança significativa e abre para revisão do time antes de decidir. Produzido como parte da skill de arquitetura.

**Release Notes**
Comunicado formal sobre uma nova versão: o que mudou, o que foi corrigido, o que está em alpha ou beta. Produzido pela skill `doc-release-notes`.

**Rollback Plan**
Plano de reversão caso o deploy falhe: critérios de acionamento, passos de reversão, responsáveis. Produzido pela skill `doc-rollback-plan`.

**Runbook**
Guia operacional para uma tarefa recorrente: passos, comandos, decisões e contatos de escalonamento. Produzido pela skill `doc-runbook`.

---

## S

**SDLC (Software Development Life Cycle)**
Ciclo de vida de desenvolvimento de software. As 8 fases da plataforma: Planning, Discovery, Design, Architecture, Builder, Validator, Deploy, Monitor.

**Skill**
Unidade básica de execução da plataforma. Autocontida, com contexto, prompt e critério de sucesso definidos. O Consultor de IA aprende criando skills.

**SOLID**
Cinco princípios de design orientado a objetos: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion. Referência obrigatória nas skills de arquitetura e validação.

**Status Report**
Relatório de acompanhamento do projeto: o que foi feito, o que está em andamento, impedimentos e próximos passos. Produzido pela skill `doc-status-report`.

**Story Mapping (SM)**
Técnica visual de organização de User Stories em jornadas do usuário para identificar prioridades e sprints. Referência para estrutura de pipelines.

---

## U

**User Journey**
Mapa da experiência do usuário ao interagir com o sistema: etapas, emoções, pontos de atrito e oportunidades. Produzido pela skill `doc-user-journey`.

**User Story (US)**
Descrição de uma funcionalidade do ponto de vista do usuário, com critérios de aceite em Gherkin. Produzida pela skill `doc-user-story`.

---

## V

**Validator**
Sétima fase do SDLC. Valida o software antes do deploy: testes funcionais, de regressão, de segurança e de performance.
