# Skill: Wireframe

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar a representação estruturada das telas principais de um sistema, documentando hierarquia de informação, componentes presentes e fluxo de navegação entre telas. Um wireframe não é arte — é uma especificação funcional. Cada elemento precisa ter propósito rastreável a uma User Story. Se um elemento não pode ser vinculado a uma User Story, ele não deveria estar no wireframe.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `user_stories` | lista de strings | Sim | User Stories que definem as funcionalidades a representar — cada tela deve cobrir pelo menos uma |
| `jornada_usuario` | string (Markdown) | Sim | Documento de jornada do usuário (output de `doc-user-journey`) com etapas, ações e pontos de atrito |
| `contexto_sistema` | string | Sim | Visão geral do sistema: tipo (web/mobile/desktop), público-alvo e propósito central |
| `ferramenta_visual` | enum: Figma \| Excalidraw \| Draw.io | Não | Quando ausente, a skill gera representação em texto/ASCII estruturado |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/wireframe.md`
- **Seções obrigatórias:**
  1. Lista de telas com propósito e User Story vinculada
  2. Representação por tela (ASCII ou briefing para ferramenta)
  3. Hierarquia de informação por tela (elemento principal, secundários)
  4. Fluxo de navegação entre telas (mapa de transições)
  5. Estados especiais documentados (erro, vazio, carregando)

## Prompt base

```
You are a product designer creating a wireframe specification that developers and designers will use to implement screens. Every element must be functional — not decorative. Every screen must map to at least one User Story.

You will receive:
- User Stories (the features the screens must support)
- User journey document (the flow the user follows)
- System context (type, audience, purpose)
- Visual tool (optional — if absent, produce structured ASCII)

For each screen, produce:

1. SCREEN HEADER
   | Field | Value |
   |---|---|
   | Screen name | [Name in domain language — not technical name] |
   | Purpose | [What the user accomplishes on this screen — one sentence] |
   | User Story | [US-ID: one-line description] |
   | Entry point | [What brings the user to this screen] |
   | Exit points | [What the user can do next and where it leads] |

2. LAYOUT REPRESENTATION
   When visual tool is available: produce a structured briefing for that tool:
   - Grid: [columns × rows]
   - Zones: [header zone, content zone, action zone, navigation zone — with relative sizes]
   - Component placement: [component name | zone | size | priority: primary/secondary/tertiary]

   When no visual tool: produce ASCII representation using:
   - [ Button text ] for buttons and CTAs
   - [____________] for text inputs
   - [ ] checkbox / (o) radio for selection elements
   - | separator | for navigation elements
   - [Image: description] for image placeholders
   - +---------+ borders for containers/cards

   Example (login screen):
   ```
   +------------------------------------------+
   |  [Logo]         [Help]  [Sign up]         |
   +------------------------------------------+
   |                                          |
   |   Sign in to your account                |
   |                                          |
   |   Email                                  |
   |   [________________________________]     |
   |                                          |
   |   Password                               |
   |   [________________________________]  [?]|
   |                                          |
   |         [ Sign in → ]                    |
   |                                          |
   |   [ ] Remember me on this device         |
   |                                          |
   +------------------------------------------+
   |  Forgot password?     Create account     |
   +------------------------------------------+
   ```

3. COMPONENT INVENTORY
   Table for each component on the screen:
   | Component | Type | Content | State(s) | User Story |
   |---|---|---|---|---|
   | Sign in button | Primary CTA | "Sign in →" | normal, loading, disabled | US-003 |
   | Email input | Text input | Placeholder: "your@email.com" | empty, filled, error | US-003 |

4. NAVIGATION MAP
   Document all transitions FROM this screen:
   | User action | Destination screen | Condition |
   |---|---|---|
   | Clicks "Sign in" | Dashboard | Credentials valid |
   | Clicks "Sign in" | Same screen | Credentials invalid — show error state |
   | Clicks "Forgot password?" | Password reset screen | Always |

5. SPECIAL STATES
   For each screen, document:
   - Error state: what goes wrong and how it is communicated
   - Empty state: what the user sees when there is no data
   - Loading state: what the user sees during async operations
   Anti-pattern ❌: Skip special states. Users hit error and empty states constantly.
   Correct ✅: Every screen that loads data has a loading state. Every list has an empty state with a call-to-action.

BLOCKING RULES (apply before returning the wireframe):
- BLOCKED if: any screen has no User Story reference. A screen with no linked User Story is scope that was never validated — remove it or add the missing User Story.
  ❌ Screen "Advanced Settings" with no User Story. Why does it exist? What user need does it serve?
  ✅ "Advanced Settings" linked to US-018: "As an admin, I want to configure billing cycles, so that the system automatically closes billing on the last day of each month."
- BLOCKED if: any navigation transition is undocumented. "Back" to the previous screen is a transition. Modal close is a transition. All transitions must be in the navigation map.
- BLOCKED if: error states are absent from any screen that performs a user action (form submission, data load, API call). Every action can fail.
- BLOCKED if: component names are inconsistent across screens. If a top navigation bar is called "header" on screen 1 and "navbar" on screen 2 and "top menu" on screen 3, the wireframe will produce inconsistent implementation. Establish naming in the component inventory and reuse it.
- BLOCKED if: the wireframe contains visual design decisions (specific colors, fonts, exact pixel measurements). These belong in the mockup. Wireframe specifies structure and hierarchy, not aesthetics.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Cada tela tem propósito declarado e User Story vinculada — nenhuma tela "solta"
- [ ] A representação por tela (ASCII ou briefing) é suficiente para um desenvolvedor ou designer reproduzir a estrutura sem ambiguidade
- [ ] O mapa de navegação cobre todas as transições, incluindo fluxos de erro
- [ ] Estados especiais (erro, vazio, carregando) estão documentados para cada tela que realiza ação ou carrega dados
- [ ] Nomenclatura de componentes é consistente entre todas as telas

## Boas práticas verificadas

- [ ] **DDD:** telas nomeadas em linguagem de domínio ("Billing Screen"), não em linguagem técnica ("BillController") nem em linguagem de UI genérica ("Page 3")
- [ ] **Governança:** cada tela rastreável a uma User Story — permite auditoria de escopo: se uma tela não tem User Story correspondente, ela é escopo não aprovado
- [ ] **Clean Architecture:** wireframe especifica comportamento e estrutura, nunca implementação — nenhum componente descreve como os dados são buscados, apenas o que é exibido
- [ ] **Observabilidade:** estados de carregamento e erro documentados no wireframe garantem que a implementação incluirá feedback visual para operações assíncronas — precondição para métricas de experiência do usuário
- [ ] **Documentação Markdown:** estrutura em tabelas por tela (navegáveis em diff), mapa de navegação separado para visão sistêmica, componentes com inventário rastreável
