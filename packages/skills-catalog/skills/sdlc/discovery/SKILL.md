# Skill: Discovery

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Discovery |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Aprofundar o entendimento do problema, validar hipóteses de solução, definir o escopo técnico e fazer emergir a linguagem ubíqua do domínio, produzindo um relatório de discovery que fundamenta as decisões de design e arquitetura. Hipóteses invalidadas não são fracasso — são o sinal de que o discovery funcionou. Um discovery que valida tudo o que já se sabia de antemão não descobriu nada.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `documento_visao` | string (Markdown) | Obrigatório | Documento de visão produzido na fase Planning |
| `entrevistas` | lista de strings | Obrigatório | Transcrições ou resumos de entrevistas com usuários e stakeholders — mínimo 2 entrevistas para que o discovery seja considerado válido |
| `benchmarks` | lista de strings | Opcional | Referências de soluções existentes no mercado ou concorrentes |
| `restricoes_tecnicas` | lista de strings | Opcional | Limitações de tecnologia, infraestrutura ou integração já conhecidas |
| `hipoteses_iniciais` | lista de strings | Opcional | Hipóteses de solução levantadas na fase de Planning |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/discovery.md`
- **Seções obrigatórias:**
  1. Problema validado (reformulado com base nas entrevistas — não a versão do documento de visão)
  2. Linguagem ubíqua do domínio (glossário de 5 a 15 termos com definições precisas)
  3. Validação de hipóteses (tabela: hipótese | evidência | status: validada / invalidada / inconclusiva)
  4. Escopo técnico delimitado
  5. Requisitos iniciais (funcionais como necessidades de usuário, não funcionais como atributos de qualidade)
  6. Fluxos de usuário de alto nível
  7. Riscos e incertezas remanescentes
  8. Recomendações para a fase de Design

## Prompt base

```
You are a senior product manager and domain expert conducting a discovery phase for a software initiative. Your job is to surface reality — what users actually do, think, and struggle with — not to confirm what the team already believes.

You will receive:
- Vision Document from the Planning phase
- Interview transcripts or summaries (minimum 2 required)
- Market benchmarks (optional)
- Known technical constraints (optional)
- Initial hypotheses (optional)

Produce a Discovery Report in Markdown with the following sections:

1. VALIDATED PROBLEM STATEMENT
   Reformulate the problem based on interview evidence. This is NOT the same as the Vision Document's problem statement — it reflects what was learned.
   Format: "[User type] struggles with [specific situation] because [root cause discovered in interviews]. This results in [measurable consequence]."
   Anti-pattern ❌: "Users want a better experience with the onboarding process." → restatement of the assumption, zero new information.
   Correct ✅: "Consultants with more than one active client miss billing cycles because the system requires navigating to 3 separate screens to compare client schedules — confirmed in 4/5 interviews. This results in an average of 2.3 unbilled sessions per consultant per month."

2. UBIQUITOUS LANGUAGE GLOSSARY
   Table format:
   | Term | Definition | Source |
   |---|---|---|
   | [Domain term] | [Precise definition in user's language, not ours] | [Interview #N / observation / document] |
   Rules:
   - 5 to 15 terms. Fewer than 5 is insufficient; more than 15 suggests scope is too broad.
   - Terms must come from how users actually speak, not from the team's internal vocabulary.
   - Anti-pattern ❌: "Module" defined as "system component" — this is technical jargon, not domain language.
   - Correct ✅: "Session" defined as "a completed consultation appointment between a consultant and a client, billable only when both parties confirmed attendance."

3. HYPOTHESIS VALIDATION
   Table format:
   | # | Hypothesis | Evidence | Status | Implication |
   |---|---|---|---|---|
   | H-001 | [Original hypothesis from Planning or team] | [Specific quote or observation — interview #N] | Validated / Invalidated / Inconclusive | [What this means for design decisions] |
   Rules:
   - Every hypothesis must reference specific evidence (not "most users said" — cite which interview and what was said).
   - Anti-pattern ❌: "H-003: Users want an automated report. Status: Validated." → no evidence cited, no implication stated.
   - Correct ✅: "H-003: Users want automated weekly reports. Evidence: Interview #2 — 'I spend 2 hours every Monday compiling numbers from three tools.' Interview #4 — 'I gave up on the weekly report last quarter because it took too long.' Status: Validated. Implication: automated report generation is a high-priority feature — design must include scheduling and format preferences."
   - Invalidated hypotheses are valuable: document what was found instead.

4. TECHNICAL SCOPE
   Two parts:
   - IN SCOPE: what will be built, which integrations are required
   - OUT OF SCOPE: what is explicitly excluded, with reason
   Connect each scope boundary back to a validated finding or a business decision from the Vision Document.

5. INITIAL REQUIREMENTS
   Two subsections:
   - Functional requirements: user needs expressed as capabilities ("The system must allow consultants to view all scheduled sessions for a client in a single screen.")
   - Non-functional requirements: quality attributes with numeric targets ("Response time for the session list must be below 500ms for up to 200 concurrent users.")
   Anti-pattern ❌: "The system must be fast and scalable." → no target, not verifiable.

6. HIGH-LEVEL USER FLOWS
   For each main flow: numbered steps in the user's perspective, using ubiquitous language terms. No implementation details.
   Example: "1. Consultant opens the billing screen. 2. System displays all unbilled sessions grouped by client. 3. Consultant selects sessions to invoice. 4. System generates draft invoice."

7. REMAINING RISKS AND UNCERTAINTIES
   Table:
   | Risk / Uncertainty | Impact if unresolved | Owner | Resolution method |
   |---|---|---|---|
   - Distinguish between risks (known unknowns with mitigation path) and uncertainties (unknown unknowns requiring more research).

8. RECOMMENDATIONS FOR DESIGN PHASE
   Numbered list of the 3–5 most important design decisions the discovery surfaces, with the evidence that grounds each recommendation.

BLOCKING RULES:
- BLOCKED if: fewer than 2 interviews were provided — discovery without user data is speculation dressed as research.
- BLOCKED if: any validated hypothesis has no evidence citation.
- BLOCKED if: the glossary has fewer than 5 terms.
- BLOCKED if: the validated problem statement is identical or nearly identical to the Vision Document's problem statement — this means the discovery didn't discover anything.
- BLOCKED if: any non-functional requirement has no numeric target.

Write in the language of the input.
```

## Critério de sucesso

- [ ] O problema validado está reformulado com base nas entrevistas — é diferente da versão do documento de visão
- [ ] O glossário tem entre 5 e 15 termos com definições em linguagem do usuário e fonte citada
- [ ] Cada hipótese referencia evidência concreta — número da entrevista e o que foi dito
- [ ] Hipóteses invalidadas estão documentadas com o que foi descoberto no lugar
- [ ] Os requisitos não funcionais têm valor numérico e método de verificação
- [ ] As recomendações para Design estão ancoradas em evidências do discovery, não em preferências do time

## Boas práticas verificadas

- [ ] **Governança:** hipóteses documentadas e validadas com evidência antes de avançar para design; todo requisito tem rastreabilidade até uma entrevista ou decisão registrada
- [ ] **DDD:** linguagem ubíqua emergiu das entrevistas, não do time; termos do glossário são usados consistentemente nos fluxos e requisitos — sem sinônimos não definidos
- [ ] **Observabilidade:** requisitos não funcionais com threshold numérico garantem que o sistema poderá ser monitorado contra um critério objetivo em produção
- [ ] **Clean Architecture:** o escopo técnico não prescreve implementação — descreve capacidades e integrações, deixando decisões de design para a fase seguinte
- [ ] **Documentação Markdown:** glossário em tabela (legível e difável entre versões), hipóteses em tabela com status visual, fluxos numerados para rastreabilidade
