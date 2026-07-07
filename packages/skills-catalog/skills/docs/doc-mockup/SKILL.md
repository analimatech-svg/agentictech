# Skill: Mockup

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar a especificação visual completa de cada tela do sistema, incluindo cores, tipografia, espaçamento, estados dos componentes e nomenclatura dos elementos reutilizáveis, para guiar a implementação do mockup em ferramenta visual.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| wireframe_aprovado | string (Markdown) | Sim | Wireframe validado contendo a estrutura e hierarquia de cada tela |
| guia_estilo | string (Markdown) | Não | Documento de identidade visual com paleta de cores, tipografia e tokens de design |
| identidade_visual | string | Não | Logotipo, cores da marca e referências visuais do produto ou empresa |
| ferramenta_alvo | enum: Figma | Sketch | Adobe XD | Não | Ferramenta onde o mockup será criado (Figma, Sketch, Adobe XD) |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/mockup-spec.md`
- **Campos obrigatórios:** especificação de cores (hex), tipografia (família, tamanho, peso), espaçamentos (valores em px ou rem), estados de cada componente, catálogo de componentes reutilizáveis

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a high-fidelity mockup specification based on the approved wireframe and visual guidelines.

For each screen, specify:
1. Color palette: background, surface, primary action, secondary action, text (hex values)
2. Typography: font family, sizes in px/rem, weights for each text role (heading, body, label, caption)
3. Spacing: margins, paddings and gaps in concrete values (8px, 16px, 24px) — never "generous" or "comfortable"
4. Component states: normal, hover, focus, active, disabled, error, empty
5. Reusable components: name, visual description, variants and usage rules

Reusable component catalog format:
- Component name
- Description
- Variants (primary, secondary, destructive...)
- States (normal, hover, disabled...)
- Usage rules (when to use each variant)

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Todos os estados de cada componente estão especificados (normal, hover, foco, erro, vazio, desabilitado)
- [ ] Espaçamentos são valores concretos em pixels ou rem, sem descrições subjetivas como "espaço generoso"
- [ ] Componentes reutilizáveis estão nomeados, descritos e documentados com suas variantes
- [ ] Cores estão em formato hexadecimal e mapeadas por papel semântico (primária, superfície, perigo, sucesso)
- [ ] Tipografia especifica família, tamanho e peso para cada papel textual (título, corpo, legenda, label)

## Boas práticas verificadas

- [ ] Design tokens são usados para cores e espaçamentos, garantindo consistência entre telas
- [ ] Componentes seguem nomenclatura do sistema de design adotado (Atomic Design, Material, etc.)
- [ ] Acessibilidade verificada: contraste de cores atende WCAG AA (razão mínima 4.5:1 para texto)
- [ ] Estados de erro têm mensagens de feedback claras, não apenas mudança de cor
- [ ] Especificação está versionada e vinculada ao wireframe aprovado
