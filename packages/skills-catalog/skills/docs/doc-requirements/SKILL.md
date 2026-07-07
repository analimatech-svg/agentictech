# Skill: Requisitos

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Discovery |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar o documento de requisitos funcionais e não funcionais do sistema, garantindo que cada requisito seja testável, rastreável até uma fonte (entrevista, regra de negócio, regulação) e que os requisitos não funcionais contenham valores numéricos verificáveis. Requisitos ambíguos, sem critério de aceite ou sem rastreabilidade são vetores de retrabalho — e devem ser bloqueados antes de entrar no backlog.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `relatorio_discovery` | string (Markdown) | Obrigatório | Relatório de discovery com problema validado, linguagem ubíqua e fluxos de usuário identificados |
| `personas` | lista de strings | Opcional | Personas definidas no discovery — usadas para associar cada requisito funcional ao usuário que ele serve |
| `entrevistas_notas` | string | Opcional | Transcrições ou resumos de entrevistas que fundamentam os requisitos, para rastreabilidade |
| `restricoes_nao_funcionais` | lista de strings | Opcional | SLAs, regulações, metas de performance ou segurança já conhecidas que devem virar RNFs (ex: "LGPD", "resposta abaixo de 500ms", "disponibilidade 99,5%") |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/requirements.md`
- **Seções obrigatórias:**
  1. Requisitos funcionais (tabela RF-001, RF-002… com ID, descrição, critério de aceite e fonte)
  2. Requisitos não funcionais (tabela RNF-001, RNF-002… com ID, categoria, valor alvo numérico, método de verificação e fonte)
  3. Rastreabilidade RF → User Story (tabela vinculando cada RF às User Stories que o implementam, a ser preenchida à medida que o backlog evolui)
  4. Requisitos em aberto (lista de requisitos que precisam de mais informação antes de serem formalizados)

## Prompt base

```
You are a senior business analyst writing a requirements document that will serve as the source of truth for User Stories, test cases, and acceptance criteria throughout the project.

You will receive:
- Discovery Report (validated problem, ubiquitous language, user flows)
- Personas (optional)
- Interview notes (optional, for traceability)
- Known non-functional constraints (optional)

Produce a Requirements Document in Markdown with the following sections:

1. FUNCTIONAL REQUIREMENTS
   Table format:
   | ID | Description | Acceptance Criterion | Persona | Source |
   |---|---|---|---|---|
   | RF-001 | [What the system must do] | [Testable condition confirming the requirement is met] | [Persona name or "All"] | [Interview #N / Discovery section / Business rule] |

   Rules:
   - Description: what the system must do, written from the user's perspective, using ubiquitous language from the discovery report.
   - Acceptance criterion: a testable condition. Must answer "how do we know this requirement is satisfied?" — not restate the description.
   - Source: every requirement must trace to an artifact (interview, discovery report section, regulatory rule, stakeholder decision). Requirements without a source are assumptions — flag them.
   - Anti-pattern ❌: "RF-004: The system must be user-friendly. Criterion: The interface is intuitive." → unverifiable and untestable. BLOCKED.
   - Anti-pattern ❌: "RF-007: The system must handle concurrent users. Criterion: Works with many users." → no threshold. BLOCKED.
   - Correct ✅: "RF-007: The system must support concurrent access by multiple users without degradation. Criterion: Response time for search queries remains below 800ms with 50 concurrent active sessions (measured in staging with load test). Source: NFR established in planning — performance baseline."

2. NON-FUNCTIONAL REQUIREMENTS
   Table format:
   | ID | Category | Description | Target Value | Verification Method | Source |
   |---|---|---|---|---|---|
   | RNF-001 | Performance | Response time for search queries | p95 ≤ 500ms under 100 concurrent users | Load test with k6 in staging | Planning baseline |
   | RNF-002 | Availability | System uptime | ≥ 99.5% monthly | Monitoring dashboard (uptime check every 60s) | SLA with client |
   | RNF-003 | Security | Data classification | PII fields encrypted at rest and in transit | Security audit before go-live | LGPD compliance |

   Categories: Performance | Availability | Security | Scalability | Maintainability | Compliance | Usability
   Rules:
   - Every RNF must have a numeric target. "Fast", "reliable", "secure" are not requirements — they are aspirations.
   - Anti-pattern ❌: "RNF-002: The system must be secure." → no threshold, not verifiable. BLOCKED.
   - Anti-pattern ❌: "RNF-003: Response time should be acceptable." → "acceptable" is subjective. BLOCKED.
   - The verification method must be specific enough that a tester can execute it without additional information.

3. TRACEABILITY — RF → USER STORY
   Table (to be filled as the backlog evolves):
   | RF ID | RF Description (brief) | User Story IDs | Status |
   |---|---|---|---|
   | RF-001 | [brief description] | US-001, US-003 | Covered |
   | RF-005 | [brief description] | — | Not yet mapped |

   Note: this table starts sparse and fills over time. Its purpose is to make visible any RF that has no User Story coverage — a gap that creates untested requirements.

4. OPEN REQUIREMENTS
   List requirements that cannot be formalized yet because key information is missing:
   | Open Item | What is missing | Who can resolve it | Target resolution date |
   |---|---|---|---|
   | [Description of open requirement] | [What data or decision is needed] | [Stakeholder or squad] | [Date or condition] |
   If no open requirements exist, write: "No open requirements at this time."

Rules:
- BLOCKED if: any functional requirement has no acceptance criterion.
- BLOCKED if: any non-functional requirement has no numeric target (words like "fast", "reliable", "secure" without a threshold are not requirements).
- BLOCKED if: any requirement has no source (interview, document, decision, regulation). Requirements without a source are assumptions masquerading as requirements.
- When a requirement is ambiguous: document it in "Open Requirements" instead of formalizing it. An ambiguous formalized requirement is worse than an open item — it creates false confidence.
- Use the ubiquitous language from the discovery report consistently. Do not introduce synonyms or technical terms not present in the discovery glossary.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Cada requisito funcional tem critério de aceite mensurável e testável — não uma reafirmação da descrição
- [ ] Cada requisito não funcional tem valor numérico (tempo de resposta em ms, disponibilidade em %, taxa de erro em %) e método de verificação específico
- [ ] Cada requisito tem rastreabilidade até uma fonte (entrevista, discovery, decisão de stakeholder, regulação)
- [ ] Requisitos sem informação suficiente para formalizar estão na seção "Requisitos em aberto" com responsável e prazo de resolução
- [ ] A linguagem ubíqua do discovery é usada consistentemente — sem sinônimos ou termos técnicos não definidos no glossário

## Boas práticas verificadas

- [ ] **Governança:** rastreabilidade RF → User Story permite auditar quais requisitos têm cobertura no backlog e quais estão descobertos; requisitos sem fonte são tratados como pressupostos explícitos, não verdades implícitas
- [ ] **DDD:** requisitos escritos na linguagem ubíqua do domínio; termos do glossário de discovery são usados sem variação; requisitos técnicos de implementação (qual API, qual framework) não aparecem como RFs — eles são decisões de arquitetura
- [ ] **Observabilidade:** RNFs de performance e disponibilidade têm método de verificação que pode ser instrumentado em um ambiente de monitoramento (não apenas testado manualmente uma vez)
- [ ] **Clean Architecture:** RFs descrevem comportamento observável pelo usuário, não implementação interna — "o sistema deve calcular o score" é RF; "o módulo ScoreService deve chamar o repositório SkillRepository" é detalhe de implementação
- [ ] **Documentação Markdown:** tabelas para RF e RNF (navegáveis e comparáveis entre versões); seção de rastreabilidade como artefato vivo que cresce com o backlog; open requirements com responsável e prazo evitam que ambiguidades fiquem invisíveis
