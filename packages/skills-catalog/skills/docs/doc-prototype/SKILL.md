# Skill: Protótipo

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar o roteiro de prototipagem navegável, especificando quais fluxos cobrir, o que deve ser interativo, o comportamento de cada interação e as animações e transições necessárias para validar as hipóteses centrais do produto com usuários reais.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| mockup_aprovado | string (Markdown) | Sim | Especificação visual completa das telas, com componentes e estados definidos |
| user_stories_priorizadas | lista de strings | Sim | Lista de User Stories ordenadas por prioridade para determinar quais fluxos o protótipo deve cobrir |
| cenarios_validacao | lista de strings | Sim | Cenários que serão testados com usuários: hipóteses a validar e métricas de sucesso |
| ferramenta_prototipagem | enum: Figma | InVision | Marvel | Axure | Não | Ferramenta onde o protótipo será construído (Figma, InVision, Marvel, Axure) |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/prototype-script.md`
- **Campos obrigatórios:** lista de fluxos cobertos, mapa de interatividade por tela (o que é clicável e o que é estático), comportamento de cada interação, especificação de animações e transições, roteiro de sessão de teste com usuários

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a prototyping script based on the approved mockup and prioritized User Stories.

Structure the output in three parts:

1. INTERACTION MAP (per screen):
   - Clickable elements: what happens when each is clicked
   - Static elements: what is display-only
   - Transitions: animation type (fade, slide, push) and duration
   - Scroll and gestures: when applicable

2. COVERED FLOWS:
   - Flow name
   - Start screen
   - Steps sequence
   - End screen
   - Hypothesis being validated

3. TEST SESSION SCRIPT:
   - Context presented to the participant
   - Tasks to complete (without revealing the expected path)
   - Observation criteria: what to note during the session
   - Success metrics: how to determine if the hypothesis was validated

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Os fluxos críticos do produto estão cobertos, priorizando as User Stories de maior valor
- [ ] Cada interação tem comportamento definido: o que acontece ao clicar, digitar ou deslizar
- [ ] O protótipo é suficiente para validar as hipóteses centrais sem precisar implementar código
- [ ] O roteiro de sessão de teste tem tarefas formuladas sem induzir o caminho esperado
- [ ] Animações e transições têm tipo e duração especificados (não apenas "com animação suave")

## Boas práticas verificadas

- [ ] O protótipo cobre fluxos de erro além dos fluxos de sucesso (happy path)
- [ ] Elementos estáticos estão claramente distinguidos dos interativos no mapa de interatividade
- [ ] Hipóteses são declaradas antes da sessão, não inferidas após observação
- [ ] Roteiro de teste é neutro e não direciona o participante para a solução esperada
- [ ] Métricas de sucesso são observáveis e mensuráveis, não subjetivas
