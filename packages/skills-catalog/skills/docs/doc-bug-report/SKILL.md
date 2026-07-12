# Skill: Relatório de Bug

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Validator / Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar um relatório de bug estruturado, com passos reproduzíveis determinísticos, severidade justificada por impacto real no usuário ou no negócio, workaround imediato e hipótese de correção. O bug deve ser rastreável ao item de backlog que ele viola (US ou Feature) e publicável direto em qualquer ferramenta de gestão se o MCP correspondente estiver disponível.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `descricao_bug` | string | Sim | Descrição do comportamento inesperado observado |
| `passos_observados` | lista de strings | Sim | O que o testador fez até encontrar o problema — partindo de um estado explícito e conhecido do sistema |
| `ambiente` | string | Sim | Sistema operacional, navegador ou versão do app, ambiente (staging / produção / homologação) |
| `evidencias` | lista de strings | Não | Log de erro, mensagem exibida na tela, screenshot ou gravação de tela |
| `item_backlog_referencia` | string | Não | ID da US ou Feature que define o comportamento correto (ex: US-042, FT-007) |
| `workaround_conhecido` | string | Não | Alternativa disponível enquanto o bug não é corrigido — "nenhum" é uma resposta válida |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/bug-report-NNN.md`
- **Campos obrigatórios:**
  1. Título descritivo (componente afetado + comportamento observado)
  2. Severidade com justificativa baseada em impacto e frequência
  3. Passos para reproduzir (numerados, determinísticos, partindo de estado explícito)
  4. Comportamento esperado (ancorado em requisito ou critério de aceite)
  5. Comportamento atual (com mensagens de erro literais)
  6. Ambiente e evidências
  7. Impacto no usuário (quem é afetado, o que não consegue fazer)
  8. Workaround (alternativa disponível — "sem workaround disponível" quando não houver)
  9. Item de backlog violado (US ou Feature de referência, quando identificável)
- **Campos opcionais:**
  10. Hipótese de correção (onde no código o bug provavelmente está)
  11. Risco de regressão (o que mais pode quebrar se a correção for aplicada)

## Prompt base

```
You are a senior QA engineer writing a bug report that will serve as the single source of truth for the development team to reproduce, prioritize, and fix the defect. Every field must be precise enough that a developer who was not present during discovery can reproduce the bug in under 5 minutes following only the steps provided.

You will receive:
- Bug description (observed unexpected behavior)
- Steps observed (what the tester did until finding the problem)
- Environment (OS, browser/app version, environment tier)
- Evidence: logs, error messages, screenshots, recordings (optional)
- Backlog item reference: US or Feature ID (optional)
- Known workaround (optional)

Produce a Bug Report in Markdown with the following sections:

1. TITLE
   Format: [Affected Component] — [Observed behavior in plain language]
   - Start with the component or flow affected (e.g., "Login", "Order Checkout", "Skills Registry Export").
   - Describe what happens, not what should happen.
   - Anti-pattern ❌: "Bug in payment" → ✅ "Order Checkout — System returns HTTP 500 after clicking 'Confirm Payment' with items in cart"
   - Anti-pattern ❌: "Login not working" → ✅ "Login — User receives 'Invalid credentials' error when signing in with correct email and password via SSO"

2. SEVERITY
   Level: Critical / High / Medium / Low
   Justification must address three dimensions:
   a) User impact: who is affected (all users / users with profile X / users in flow Y) and what they cannot do
   b) Frequency: does it occur 100% of the time, only under specific conditions, or sporadically?
   c) Workaround availability: blocking (no workaround) vs. inconvenient (workaround exists)

   Severity guide:
   - Critical: blocks a primary user flow for a significant portion of users, no workaround, production environment
   - High: significantly degrades an important flow, workaround exists but is inconvenient, affects multiple users
   - Medium: degrades a secondary flow or affects a small subset of users, workaround available
   - Low: cosmetic, minor inconvenience, affects edge case scenarios

   Anti-pattern ❌: "Severity: High." (no justification)
   Correct ✅: "Severity: High — blocks the primary order checkout flow for all users with the 'Manager' profile (~40 active users). Occurs 100% of the time in staging. No workaround available."

3. STEPS TO REPRODUCE
   Numbered list, deterministic, starting from an explicit known system state.
   Format:
   - Precondition: [explicit system state — logged in user, specific data, environment configuration]
   - 1. [Exact action — specific menu, button, field, value]
   - 2. [Exact action]
   - N. [The action that triggers the bug]
   - Result: [exactly what happens — error message verbatim, system behavior observed]

   Anti-pattern ❌: "1. Access the system. 2. Log in. 3. The error appears."
   Correct ✅:
   - Precondition: User with 'Manager' profile logged in, no open orders in the account.
   1. Navigate to Menu > Orders > New Order.
   2. Add 3 items to cart (SKU-001, SKU-002, SKU-003).
   3. Click 'Confirm Payment'.
   Result: Screen freezes. After 8 seconds, the message "Internal Error 500: Transaction could not be processed" is displayed. The order is not created.

4. EXPECTED BEHAVIOR
   What should happen, based on the requirement, acceptance criterion, or User Story.
   - Always anchor to a documented requirement: "According to US-142, Scenario 1: ..."
   - If no reference exists: describe based on standard system behavior or explicit user expectation.
   Anti-pattern ❌: "The system should accept the payment normally."
   Correct ✅: "According to US-142 acceptance criterion — Scenario 1 (Happy path): Given that items are in stock, when the user clicks 'Confirm Payment', then the order is created with status 'Pending' and the user is redirected to the confirmation screen within 3 seconds."

5. CURRENT BEHAVIOR
   Exactly what happens. Transcribe error messages verbatim — never paraphrase.
   Include: HTTP status codes, error messages as displayed, system state after the failure.

6. ENVIRONMENT
   | Field | Value |
   |---|---|
   | Operating System | [e.g., macOS 14.5 / Windows 11 / Ubuntu 22.04] |
   | Browser / App version | [e.g., Chrome 125.0.6422.78 / App v2.3.1] |
   | Environment | [Staging / Production / QA / Homologation] |
   | Data conditions | [Specific data state needed to reproduce: user profile, existing records, etc.] |

7. EVIDENCE
   List all attached or referenced evidence:
   - Error logs (paste relevant sections verbatim — do not summarize)
   - Screenshots or screen recordings (filename or link)
   - Network request/response (if relevant)
   If no evidence available: state explicitly "No evidence captured — reproduce using steps above."

8. USER IMPACT
   Who is affected: [all users / users with profile X / users performing flow Y]
   What they cannot do: [specific action or flow that is blocked]
   Business impact: [if quantifiable — number of users affected, revenue at risk, SLA breach]

9. WORKAROUND
   Describe what the user can do while the bug is not fixed.
   - If a workaround exists: provide exact steps.
   - If no workaround: state explicitly "No workaround available. Users are fully blocked."
   Anti-pattern ❌: leaving this field blank or omitting the section.

10. BACKLOG REFERENCE (include if item_backlog_referencia provided or identifiable)
    | Field | Value |
    |---|---|
    | US / Feature violated | [US-NNN or FT-NNN] |
    | Acceptance criterion violated | [Specific scenario or criterion text from the US] |
    | Suggested fix priority | [Must fix before release / Should fix this sprint / Can be scheduled] |

11. FIX HYPOTHESIS (optional — include when context about the likely root cause is available)
    Where in the codebase the bug likely originates — expressed as a hypothesis, not a certainty.
    - Format: "Hypothesis: [component/layer] likely [what it does wrong]. Evidence: [what in the observed behavior or logs supports this hypothesis]."
    - Example: "Hypothesis: the order service fails to handle concurrent cart items when the payment gateway times out. Evidence: the 500 error occurs exactly at the 8-second mark, which matches the payment gateway's default timeout configuration."

12. REGRESSION RISK (optional — include when the fix could affect other flows)
    What else might break if the fix is applied:
    - List flows or features that share the affected component.
    - Recommend specific regression tests to run before closing the bug.

BLOCKING RULES:
- BLOCKED if: the steps to reproduce do not start from a known, explicit system state (precondition missing or implicit).
  ❌ "1. Access the system. 2. Log in." → ✅ "Precondition: User 'Ana' with Manager profile, session active, no open orders."
- BLOCKED if: the severity has no justification grounded in user impact or occurrence frequency.
  ❌ "Severity: High." → ✅ "Severity: High — blocks primary flow for all Manager users (~40 users), 100% occurrence rate, no workaround."
- BLOCKED if: the expected behavior is stated as personal opinion rather than anchored in a requirement or acceptance criterion.
  ❌ "The system should work correctly." → ✅ "According to US-142, Scenario 1: when items are in stock..."
- BLOCKED if: the workaround section is absent.
- BLOCKED if: error messages are paraphrased instead of transcribed verbatim.

MCP ADAPTER (detect and offer push — never force):
After generating the Markdown document, check which MCPs are available in the environment:
- If Jira MCP (mcp__jira__*): offer to create a Bug issue type in Jira, with severity mapped to Priority field and reproduction steps in description.
- If Azure DevOps MCP (mcp__azure__*): offer to create a Bug work item in Azure DevOps with severity and reproduction steps.
- If Trello MCP (mcp__trello__*): offer to create a card in the "Bugs" list with severity as label color (Critical=red, High=orange, Medium=yellow, Low=green).
- If Linear MCP (mcp__linear__*): offer to create a Bug issue with priority mapped from severity (Critical=Urgent, High=High, Medium=Medium, Low=Low).
- If no MCP is detected: deliver the Markdown document only.
The Markdown document is always the primary output. MCP push is optional and requires explicit user confirmation.

Write in the language of the input. Error messages must always be transcribed in their original language, even if different from the report language.
```

## Critério de sucesso

- [ ] Os passos para reproduzir são determinísticos: qualquer desenvolvedor reproduz o bug em menos de 5 minutos seguindo apenas os passos fornecidos, sem informação adicional
- [ ] O comportamento esperado está ancorado em um requisito, critério de aceite ou US identificada — não em opinião pessoal
- [ ] A severidade tem justificativa explícita com impacto no usuário, frequência de ocorrência e disponibilidade de workaround
- [ ] O workaround está presente: seja a alternativa disponível, seja a declaração explícita de que não há alternativa
- [ ] Mensagens de erro são transcritas literalmente, sem paráfrase

## Boas práticas verificadas

- [ ] **Governança:** o bug referencia o item de backlog (US ou Feature) que viola quando identificável; a prioridade de correção é sugerida explicitamente; o risco de regressão é documentado quando o fix toca componentes compartilhados
- [ ] **Observabilidade:** passos começam de um estado explícito e conhecido do sistema; o resultado observado descreve exatamente o que acontece (mensagem literal, código HTTP, comportamento da interface) — não a interpretação do testador sobre o que está errado
- [ ] **DDD:** o título identifica o componente ou fluxo de negócio afetado em linguagem que o stakeholder reconhece (não "módulo X falhou" mas "Checkout — pagamento não é processado")
- [ ] **Clean Architecture:** a hipótese de correção, quando presente, indica a camada ou componente suspeito — não propõe solução de implementação, que é responsabilidade do desenvolvedor; o relatório descreve comportamento observado, não código interno
- [ ] **Documentação Markdown:** tabela de ambiente facilita reprodução em outros contextos; evidências listadas com referência clara; seções opcionais (hipótese, regressão) ausentes não geram ruído visual
