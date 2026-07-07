# Skill: Arquitetura Técnica

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Architecture |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Documentar a arquitetura técnica de um **sistema existente** — aquele que já está em produção ou em desenvolvimento avançado e nunca foi formalmente documentado. É a skill de auditoria e reverse-engineering arquitetural, não de design. Para definir a arquitetura de um sistema novo, use `skills/sdlc/architecture`.

O output desta skill é `docs/existing-architecture.md` — arquivo separado do `docs/architecture.md` produzido pelo pipeline SDLC, para evitar sobreposição.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| requisitos | string (Markdown) | Sim | Requisitos funcionais e não funcionais do sistema |
| stack_tecnologica | string | Sim | Linguagens, frameworks, bancos de dados, serviços de nuvem e ferramentas adotadas |
| contexto_negocio | string | Sim | Domínio de negócio, bounded contexts e principais entidades do sistema |
| adrs_existentes | string (Markdown) | Não | ADRs já registrados que registram decisões de arquitetura relevantes |
| restricoes | lista de strings | Não | Restrições de performance, segurança, custo ou compliance que moldam a arquitetura |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/existing-architecture.md` (distinto de `docs/architecture.md` produzido por `sdlc/architecture`)
- **Campos obrigatórios:** visão geral, diagrama de componentes (ASCII ou referência externa), responsabilidade de cada componente, fluxo de dados principal, dependências externas, separação de camadas, trade-offs documentados

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a technical architecture document based on the provided context.

Structure:
1. OVERVIEW: System purpose, architectural style (monolith, microservices, event-driven, serverless) and key quality attributes (scalability, reliability, maintainability)

2. COMPONENT DIAGRAM: ASCII diagram or reference to external tool (Mermaid, C4 Model, PlantUML). Each component in a box with its name and one-line responsibility.

3. COMPONENTS: For each component:
   - Name
   - Responsibility (Single Responsibility Principle: one clear, bounded job)
   - Technology
   - Interfaces (what it exposes and what it consumes)

4. DATA FLOW: Step-by-step narrative of how data moves through the system for the main use case

5. EXTERNAL DEPENDENCIES: Third-party services, APIs and databases with justification for each

6. LAYER SEPARATION: How the system separates concerns (presentation, application, domain, infrastructure)

7. TRADE-OFFS: What was sacrificed for what (consistency vs availability, simplicity vs flexibility)

Apply Clean Architecture, DDD and Event-Driven principles where applicable. Reference existing ADRs when relevant.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Há um diagrama de componentes (ASCII ou referência a ferramenta externa como Mermaid ou C4)
- [ ] Cada componente tem responsabilidade definida seguindo o princípio da responsabilidade única
- [ ] As dependências entre componentes são explícitas, justificadas e representadas no diagrama
- [ ] As camadas do sistema estão claramente separadas com regras de dependência definidas
- [ ] Os trade-offs arquiteturais estão documentados: o que foi sacrificado e por quê

## Boas práticas verificadas

- [ ] Clean Architecture: dependências apontam para dentro (domínio não depende de infraestrutura)
- [ ] DDD: bounded contexts estão identificados quando o domínio é complexo
- [ ] Event-Driven: eventos de domínio estão documentados quando o sistema usa mensageria
- [ ] Observabilidade: logging, métricas e tracing estão mencionados como requisitos arquiteturais
- [ ] O documento referencia os ADRs que justificam as decisões arquiteturais relevantes
