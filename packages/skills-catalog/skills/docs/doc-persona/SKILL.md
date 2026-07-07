# Skill: Persona

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Discovery |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar uma persona que representa um tipo de usuário do sistema, baseada em dados reais de entrevistas ou pesquisa quantitativa — não em suposições ou estereótipos. Uma persona sem fonte de dado explícita, com frustrações genéricas ou com comportamentos inventados não serve de base para decisões de produto e deve ser bloqueada.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `dados_pesquisa` | string | Obrigatório | Fonte dos dados que embasam esta persona: transcrições de entrevistas, resultados de surveys, dados de analytics, observações de usabilidade ou logs de suporte. Deve incluir o tipo de fonte e o volume (ex: "6 entrevistas de 45 min com consultores de TI plenos") |
| `contexto_produto` | string | Obrigatório | O que o sistema faz, quem são os usuários esperados e qual problema ele resolve — para calibrar as frustrações e objetivos ao contexto real do produto |
| `segmento_alvo` | string | Opcional | Critério de segmentação que define este grupo específico de usuários (ex: nível de senioridade, contexto de uso, frequência de uso, tipo de contrato) — necessário quando há múltiplas personas e elas precisam ser distinguíveis |
| `dados_quantitativos` | string | Opcional | Dados complementares de analytics ou surveys: frequência de uso de funcionalidades, tempo na tarefa, taxa de abandono, NPS por segmento |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/persona-[nome].md`
- **Campos obrigatórios:**
  1. Nome fictício e resumo de uma linha
  2. Perfil demográfico e contexto de uso
  3. Objetivos principais (o que esta persona quer alcançar usando o sistema)
  4. Frustrações específicas com contexto (quando e por que ocorrem)
  5. Comportamentos observados (com referência à fonte dos dados)
  6. Citação representativa (soa como pessoa real, não como marketing)
  7. Fonte dos dados (tipo + volume + data)
  8. O que esta persona NÃO é (anti-persona: o que não representa este segmento)

## Prompt base

```
You are a UX researcher synthesizing qualitative and quantitative research into a persona that will be used by the entire product team to make design and prioritization decisions. The persona must be grounded in data — not invented.

You will receive:
- Research data: interview transcripts, survey results, analytics, support logs (must be provided — if missing, halt and request before generating)
- Product context: what the system does and for whom
- Target segment definition (optional)
- Quantitative data (optional)

Produce a Persona Document in Markdown with the following sections:

1. NAME AND SUMMARY
   - Fictional but realistic name (not "User A" or "Persona 1").
   - One-sentence summary: who this person is and what they use the system for.

2. DEMOGRAPHIC PROFILE AND USAGE CONTEXT
   Table:
   | Attribute | Value |
   |---|---|
   | Age range | [e.g., 28–40 years] |
   | Role / Title | [e.g., Mid-level IT Consultant] |
   | Industry / Sector | [e.g., Financial Services] |
   | Experience with similar tools | [e.g., 3–5 years using Jira, Confluence] |
   | Usage context | [e.g., uses the system 3× per week during project kick-offs] |
   | Technical proficiency | [Low / Medium / High — with what evidence] |
   All values must reference the provided research, not be invented.

3. MAIN GOALS
   What this persona is trying to achieve when using the system. Maximum 3 goals.
   Format: numbered list, each goal in one sentence describing the outcome they want — not the feature they want.
   - Anti-pattern: ❌ "Wants a dashboard with filters" → ✅ "Needs to quickly assess whether a project's skills gap is a blocker before the kickoff meeting (typically 15–20 minutes before the call starts)"

4. SPECIFIC FRUSTRATIONS
   What makes this persona's current experience harder than it should be. Maximum 5 frustrations.
   Each frustration must include:
   - What the friction is (specific, not generic)
   - When it occurs (in what context or workflow step)
   - Evidence (reference to the research source)
   Format: "Frustration: [specific friction]. Context: [when it happens]. Evidence: [source reference]."
   - Anti-pattern ❌: "The system is slow" → ✅ "Frustration: Search results take 8–12 seconds to load. Context: During live client calls when looking up a consultant's skills. Evidence: Observed in 4 of 6 usability sessions; participants vocalized frustration in 3 sessions."

5. OBSERVED BEHAVIORS
   What this persona actually does (not what they say they do). Maximum 5 behaviors.
   Each behavior must reference the data source.
   - Anti-pattern ❌: "They prefer simple interfaces" (opinion, not observation) → ✅ "Opens a separate spreadsheet alongside the system to cross-reference data — observed in 5/6 sessions (Evidence: screen recording analysis)"
   - Anti-pattern ❌: "They use the system daily" (frequency claim without data) → ✅ "Logs in 3.2 times per week on average (Evidence: analytics — last 90 days, N=47 active users in this segment)"

6. REPRESENTATIVE QUOTE
   A realistic quote that captures this persona's mindset, as if spoken by a real person in an interview.
   - Must sound like something a real person actually said — not a polished marketing sentence.
   - If a real quote from the research is available, use it (paraphrased to remove identifying information).
   - Anti-pattern: ❌ "I need a tool that empowers me to work smarter and deliver exceptional results." → ✅ "Half the time I just open a spreadsheet because I can't find what I need fast enough in the system."

7. DATA SOURCE
   | Source type | Volume | Date |
   |---|---|---|
   | [e.g., User interviews] | [e.g., 6 sessions, 45 min each] | [e.g., June 2026] |
   | [e.g., Support tickets analysis] | [e.g., 120 tickets reviewed] | [e.g., Q2 2026] |
   - If data is thin (e.g., only 2 interviews), document it as a limitation and flag that the persona is provisional.

8. WHAT THIS PERSONA IS NOT (Anti-Persona)
   2–3 bullet points describing who this persona does NOT represent, to prevent overgeneralization.
   Example: "This persona does not represent: (1) senior consultants with 10+ years of experience who have internalized the process; (2) clients who use the system for viewing reports only (no creation); (3) team leads responsible for staffing decisions."

Rules:
- BLOCKED if: no research data is provided. Do not generate a persona from assumptions — this produces a fictional character, not a research artifact. Halt and request data.
- BLOCKED if: any frustration is generic ("the system is slow", "the process is complex") without specific context, frequency, or evidence.
- BLOCKED if: any behavior is an assumption or stated preference ("they prefer X") without observational or quantitative evidence.
- BLOCKED if: the representative quote sounds like marketing copy or a product endorsement.
- When data is thin: generate the persona with explicit provisional flags on each section where data is insufficient. Do not silently fill gaps with assumptions.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Cada frustração é específica com contexto (quando ocorre) e referência à fonte dos dados
- [ ] Cada comportamento observado referencia a fonte dos dados (entrevista, analytics, observação)
- [ ] A citação representativa soa como algo que uma pessoa real diria — não linguagem de marketing
- [ ] A seção "O que esta persona NÃO é" está presente e distingue este segmento de outros perfis
- [ ] A fonte dos dados está documentada com tipo, volume e data — e limitações são sinalizadas quando a base é pequena

## Boas práticas verificadas

- [ ] **Governança:** persona baseada em dado documentado — qualquer decisão de produto que referencie esta persona pode ser auditada até a pesquisa que a originou; persona provisional tem flags explícitas onde os dados são insuficientes
- [ ] **DDD:** objetivos e frustrações descritos em linguagem do domínio do usuário, não em terminologia do produto ou da engenharia; a persona reflete o contexto de uso real, não o contexto ideal imaginado pelo time
- [ ] **Observabilidade:** comportamentos têm fonte rastreável (número de sessões, período de analytics, volume de tickets); frequência de uso tem dado quantitativo quando disponível
- [ ] **Documentação Markdown:** tabela de perfil demográfico para leitura rápida; anti-persona em bullets para prevenção de overgeneralization; data source em tabela para auditabilidade
- [ ] **Segmentação defensável:** a seção "O que esta persona NÃO é" previne que a persona seja usada para justificar decisões para segmentos que ela não representa — anti-padrão comum em times que têm uma persona para "todos os usuários"
