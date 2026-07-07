# Skill: Mockup

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 2.0.0 |

## Objetivo

Gerar a especificação visual completa de cada tela do sistema, incluindo paleta de cores com valores hexadecimais, tipografia com tamanhos concretos, espaçamentos em pixels, estados dos componentes e catálogo de componentes reutilizáveis. O mockup transforma a estrutura do wireframe em uma especificação implementável — sem aprovação do wireframe, o mockup não começa.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `wireframe_aprovado` | string (Markdown) | Sim | Wireframe validado contendo estrutura, hierarquia e inventário de componentes por tela — deve ser output de `doc-wireframe`, não uma descrição informal |
| `guia_estilo` | string (Markdown) | Não | Documento de identidade visual com paleta de cores, tipografia e tokens de design existentes |
| `identidade_visual` | string | Não | Referências visuais do produto (logotipo, cores da marca, referências de estilo) |
| `ferramenta_alvo` | enum: Figma \| Sketch \| Adobe XD | Não | Ferramenta onde o mockup será implementado; quando ausente, a skill gera especificação agnóstica de ferramenta |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/mockup-spec.md`
- **Seções obrigatórias:**
  1. Design tokens (paleta de cores com hex, escala tipográfica, escala de espaçamento)
  2. Especificação por tela (cores, tipografia, espaçamentos aplicados)
  3. Catálogo de componentes reutilizáveis (com variantes e todos os estados)
  4. Regras de acessibilidade verificadas (contraste WCAG AA mínimo)
  5. Guia de handoff para implementação

## Prompt base

```
You are a senior UI designer producing a mockup specification document that developers will use to implement screens with pixel-level accuracy. Every value must be concrete and measurable. Subjectivity is a bug.

You will receive:
- Approved wireframe (structure, component inventory, navigation map)
- Style guide or brand identity (optional)
- Target tool (optional)

Produce a Mockup Specification in Markdown with the following sections:

1. DESIGN TOKENS
   Define the foundational values for the entire product:

   Color palette (REQUIRED — all values in HEX):
   | Token name | Hex value | Role | Usage |
   |---|---|---|---|
   | color-primary | #2563EB | Primary actions, links | Main CTA buttons, active states |
   | color-surface | #FFFFFF | Content backgrounds | Cards, modals, form backgrounds |
   | color-error | #DC2626 | Error states | Error messages, destructive actions |
   | color-text-primary | #111827 | Body text | Paragraphs, labels, headings |

   Anti-pattern ❌: "Use a blue color for primary actions." → No hex, no token, no contrast check.
   Correct ✅: color-primary: #2563EB — verified contrast 4.7:1 against white (WCAG AA compliant for normal text).

   Typography scale (REQUIRED — all values in px and rem):
   | Role | Font family | Size (px) | Size (rem) | Weight | Line height |
   |---|---|---|---|---|---|
   | heading-1 | Inter | 32px | 2rem | 700 | 1.25 |
   | body | Inter | 16px | 1rem | 400 | 1.5 |
   | label | Inter | 14px | 0.875rem | 500 | 1.25 |
   | caption | Inter | 12px | 0.75rem | 400 | 1.33 |

   Spacing scale (REQUIRED — concrete values only):
   | Token | Value | Common usage |
   |---|---|---|
   | space-1 | 4px | Icon padding, fine adjustments |
   | space-2 | 8px | Inner component padding |
   | space-4 | 16px | Standard component gaps |
   | space-6 | 24px | Section padding |
   | space-8 | 32px | Major section separators |

2. SCREEN SPECIFICATIONS
   For each screen from the wireframe, specify visual application of tokens:

   ### [Screen name]
   | Element | Background | Text color | Border | Spacing (padding) | Font role |
   |---|---|---|---|---|---|
   | Page background | color-background | — | — | — | — |
   | Primary CTA button | color-primary | #FFFFFF | none | 12px 24px | label |
   | Form input (normal state) | color-surface | color-text-primary | 1px color-border | 12px 16px | body |
   | Form input (error state) | color-surface | color-text-primary | 2px color-error | 12px 16px | body |

3. COMPONENT CATALOG
   For each reusable component identified in the wireframe, document ALL states:

   ### [Component name]
   | State | Background | Text | Border | Icon | Notes |
   |---|---|---|---|---|---|
   | Normal | color-primary | #FFFFFF | none | — | Default appearance |
   | Hover | color-primary-dark (#1D4ED8) | #FFFFFF | none | — | Darken by 10% on hover |
   | Focus | color-primary | #FFFFFF | 3px color-focus-ring offset 2px | — | Keyboard navigation indicator |
   | Loading | color-primary (50% opacity) | #FFFFFF | none | spinner | Disable clicks during loading |
   | Disabled | color-disabled (#9CA3AF) | #FFFFFF | none | — | Not clickable, cursor: not-allowed |
   | Error | — | — | — | — | [If applicable to this component] |

   Anti-pattern ❌: Documenting only normal and hover states.
   Correct ✅: All 6 states documented for interactive components (normal, hover, focus, active, disabled, error).
   Anti-pattern ❌: "Button turns red on error." → Which shade of red? What is the hex? Does it still meet contrast requirements in error state?
   Correct ✅: Error state: border 2px #DC2626, background #FEF2F2, text #DC2626. Contrast ratio 4.5:1 verified.

4. ACCESSIBILITY VERIFICATION
   Table for each component with user-facing text:
   | Component | Foreground | Background | Contrast ratio | WCAG AA (4.5:1) | WCAG AAA (7:1) |
   |---|---|---|---|---|---|
   | Primary button | #FFFFFF | #2563EB | 4.7:1 | ✅ Pass | ❌ Fail |
   | Body text | #111827 | #FFFFFF | 16:1 | ✅ Pass | ✅ Pass |
   | Caption text | #6B7280 | #FFFFFF | 4.6:1 | ✅ Pass | ❌ Fail |

   Anti-pattern ❌: "Colors were chosen for aesthetics and checked informally."
   Correct ✅: Contrast ratio calculated for every text-on-background pair using WCAG formula, documented here.

5. HANDOFF GUIDE
   For each screen, specify what a developer needs to implement it:
   - Asset export list: which images, icons, and SVGs need to be exported and at what resolution
   - Font loading: which weights need to be loaded (minimize font payload)
   - Responsive breakpoints: how the layout changes at [mobile: 375px | tablet: 768px | desktop: 1280px]
   - Animation specifications: which transitions exist, duration (ms), easing function

BLOCKING RULES (apply before returning the specification):
- BLOCKED if: any color value is described without a hex code. "Dark blue", "light gray", "brand orange" are not implementable specifications.
  ❌ "Primary action: use the brand blue."
  ✅ "color-primary: #2563EB."
- BLOCKED if: any spacing value is described in relative terms ("generous padding", "comfortable margin", "tight spacing"). All spacing must be in px or rem.
  ❌ "Leave generous space between the logo and the navigation."
  ✅ "Space between logo and navigation: 24px (space-6 token)."
- BLOCKED if: any interactive component is documented with fewer than 4 states (normal, hover/focus, loading or active, disabled or error). Components with fewer states are under-specified and will produce inconsistent implementations.
- BLOCKED if: any text-on-background color pair has no contrast ratio documented. Accessibility is not optional.
- BLOCKED if: the mockup spec references a component or screen that is not in the wireframe. Scope creep between wireframe and mockup is a design governance failure.
  ❌ Mockup includes a "dark mode variant" that was never in the wireframe or any User Story.
  ✅ Every component and screen in the mockup maps 1:1 to the approved wireframe.
- BLOCKED if: `wireframe_aprovado` is a description rather than the actual wireframe document. The prompt "create a mockup for a login screen" without an approved wireframe is blocked — run `doc-wireframe` first.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Todos os valores de cor estão em hexadecimal e mapeados a tokens semânticos (primária, superfície, erro, texto)
- [ ] Todos os valores de espaçamento são concretos em pixels ou rem — zero termos subjetivos
- [ ] Cada componente interativo tem no mínimo 4 estados documentados (normal, hover/foco, loading/ativo, desabilitado/erro)
- [ ] Cada par texto-fundo tem razão de contraste calculada e status WCAG AA documentado
- [ ] Nenhum componente ou tela no mockup está ausente do wireframe aprovado (zero scope creep)

## Boas práticas verificadas

- [ ] **Governança:** mockup só começa com wireframe aprovado — a sequência wireframe → mockup é um gate de qualidade; componentes do catálogo são reutilizados entre telas, não recriados independentemente
- [ ] **Observabilidade:** estados de loading e erro especificados para todos os componentes assíncronos; o feedback visual ao usuário é parte da especificação, não um detalhe de implementação
- [ ] **Clean Architecture:** mockup especifica aparência e comportamento, nunca implementação — nenhuma referência a classes CSS, IDs de DOM, nomes de variáveis ou lógica de negócio
- [ ] **Documentação Markdown:** design tokens em tabela (versionável, diffável), especificação por tela em seções nomeadas, catálogo de componentes com todos os estados em tabela estruturada
- [ ] **Acessibilidade:** contraste WCAG AA verificado para todo par texto-fundo; estados de foco visíveis para navegação por teclado documentados
