# Skill: User Story

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar uma User Story no formato "Como [persona], quero [ação], para [benefício]" acompanhada de critérios de aceite em Gherkin (Dado que / Quando / Então), cobrindo o caminho feliz e pelo menos um cenário de exceção realista. Uma User Story cujo benefício é vago, cujos critérios de aceite exigem julgamento humano para verificar, ou que não cabe em um único sprint deve ser bloqueada antes de entrar no backlog.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `persona` | string | Obrigatório | Perfil do usuário que a história representa — deve ser uma persona definida no documento de discovery, não um papel genérico como "usuário" ou "admin" |
| `funcionalidade` | string | Obrigatório | A capacidade do sistema que a história deve capturar, descrita do ponto de vista do usuário (não da implementação) |
| `contexto_sistema` | string | Opcional | Regras de negócio, fluxos existentes ou restrições técnicas relevantes para gerar critérios de aceite precisos |
| `historias_relacionadas` | lista de strings | Opcional | IDs de User Stories que esta depende ou que dependem dela — para verificar que ela é independente ou documenta a dependência explicitamente |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/user-story-[id].md`
- **Campos obrigatórios:**
  1. ID e título (US-NNN: título descritivo em linguagem de negócio)
  2. Declaração no formato "Como / Quero / Para" com benefício específico
  3. Critérios de aceite em Gherkin (mínimo 2 cenários: caminho feliz + 1 exceção realista)
  4. Estimativa de tamanho (Story Points ou P/M/G com justificativa)
  5. Checklist INVEST (Independent, Negotiable, Valuable, Estimable, Small, Testable)
  6. Dependências identificadas (outras stories, sistemas ou decisões)

## Prompt base

```
You are a senior product manager and agile practitioner writing User Stories that will serve as the contract between product, design, and engineering. Every story you write must be ready for a sprint — not a draft to refine later.

You will receive:
- Persona (must be a named persona from discovery, not a generic role)
- Feature to describe (from the user's perspective)
- System context: business rules, existing flows, technical constraints (optional)
- Related stories (optional)

Produce a User Story Document in Markdown with the following sections:

1. ID AND TITLE
   Format: US-NNN: [Descriptive title in business language]
   - The title must describe the outcome, not the implementation.
   - Anti-pattern: ❌ "US-001: Implement JWT authentication" → ✅ "US-001: Sign in with email and password"

2. STORY STATEMENT
   Format: "As a [specific persona], I want to [specific action], so that [meaningful benefit]."
   - Persona: must be a named persona (e.g., "Ana, a new consultant"), not "a user" or "an admin."
   - Action: must describe what the user does, not what the system does internally.
   - Benefit: must explain WHY the user cares — not just a restatement of the action.
   - Anti-pattern ❌: "As a user, I want to log in, so that I can access the system."
   - Correct ✅: "As Ana, a new consultant joining a project, I want to sign in with my work email so that I can access my team's skills registry without waiting for IT to create a separate account."

3. ACCEPTANCE CRITERIA (Gherkin)
   Minimum 2 scenarios. Format for each:
   ```
   Scenario [N]: [Scenario name]
   Given [precondition — system state before the action]
   When [specific action the user takes]
   Then [observable, verifiable outcome]
   And [additional verifiable outcome if needed]
   ```
   Rules:
   - Every "Then" must be automatable without human judgment. If a tester needs to decide "does this look right?" — rewrite it.
   - Scenario 1: the happy path (standard successful flow).
   - Scenario 2+: realistic exceptions — validation errors, system failures, unauthorized access, edge cases (empty fields, maximum limits, special characters).
   - Anti-pattern ❌: "Then the user should see a confirmation" → ✅ "Then the system displays the message 'Login successful. Welcome, Ana.' and redirects to /dashboard within 2 seconds"

4. SIZE ESTIMATE
   Story Points (1, 2, 3, 5, 8) or T-shirt size (S/M/L/XL) with a 2-3 line rationale.
   - If the estimate is 8 or XL: flag that the story may need to be split before entering a sprint.

5. INVEST CHECKLIST
   Verify each property:
   - [ ] Independent: can be built and delivered without depending on another unstarted story (or: dependency is explicit)
   - [ ] Negotiable: the how is not fixed — only the outcome is defined
   - [ ] Valuable: delivers value to the persona or the business on its own, not just as infrastructure
   - [ ] Estimable: the team has enough information to estimate it
   - [ ] Small: fits within a single sprint (typically ≤ 5 story points)
   - [ ] Testable: every acceptance criterion can be verified without subjective judgment
   If any INVEST property fails: document why and flag it for review before accepting into the sprint.

6. DEPENDENCIES
   Table: Dependency | Type (Story / System / Decision / External) | Status (Resolved / Pending / Blocking)
   If no dependencies: "No dependencies identified."

Rules:
- BLOCKED if: the benefit in the story statement is a restatement of the action (e.g., "so that I can log in" after "I want to log in").
- BLOCKED if: any acceptance criterion requires subjective judgment to verify ("looks good", "works correctly", "is fast enough" without a threshold).
- BLOCKED if: the story is not Small and no split recommendation is provided.
- BLOCKED if: the persona is generic ("a user", "an admin", "the system") without a named persona from discovery.
- Write acceptance criteria in the same language as the input. Code blocks for Gherkin scenarios.
```

## Critério de sucesso

- [ ] O benefício na declaração "Como / Quero / Para" explica por que o usuário se importa — não repete a ação com outras palavras
- [ ] Há pelo menos dois cenários Gherkin: o caminho feliz e um cenário de exceção realista e não trivial
- [ ] Todos os critérios de aceite são automatizáveis sem julgamento subjetivo
- [ ] O checklist INVEST está preenchido — falhas identificadas estão documentadas com motivo
- [ ] A estimativa tem justificativa; stories com 8+ pontos têm recomendação de split

## Boas práticas verificadas

- [ ] **Governança:** story só entra no sprint após o INVEST ser verificado; critérios de aceite registrados antes da implementação, não depois de discussão oral durante o desenvolvimento
- [ ] **DDD:** persona, ação e benefício usam a linguagem ubíqua do domínio definida no discovery; termos técnicos de implementação (JWT, API, endpoint) não aparecem na declaração da story
- [ ] **Observabilidade:** os "Then" do Gherkin descrevem estados observáveis do sistema (o que aparece na tela, o que a API retorna, o que é gravado no banco) — não intenções internas do código
- [ ] **Clean Architecture:** a story não descreve decisões de implementação (qual camada executa o quê, qual biblioteca é usada); ela descreve o comportamento observável pelo usuário
- [ ] **Documentação Markdown:** cada cenário Gherkin está em bloco de código com sintaxe consistente; tabela de dependências facilita rastreabilidade no backlog
