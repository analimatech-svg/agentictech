# Skill: Wireframe

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar a representação estruturada das telas principais de um sistema, documentando hierarquia de informação, componentes presentes e fluxo de navegação entre telas.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| user_stories | lista de strings | Sim | Lista de User Stories que definem as funcionalidades a representar |
| jornada_usuario | string (Markdown) | Sim | Fluxo completo que o usuário percorre para atingir seu objetivo |
| contexto_sistema | string | Sim | Visão geral do sistema: tipo, público-alvo e propósito central |
| ferramenta_visual | enum: Figma | Excalidraw | Draw.io | Não | Nome da ferramenta disponível (Figma, Excalidraw, Draw.io). Quando ausente, a skill gera representação em texto/ASCII |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/wireframe.md`
- **Campos obrigatórios:** lista de telas, hierarquia de informação por tela, componentes presentes, fluxo de navegação entre telas

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a wireframe specification based on the provided context.

When a visual tool (Figma, Excalidraw) is available:
- Generate a structured briefing to guide wireframe creation in that tool
- Include layout grid, component placement and navigation annotations

When no visual tool is available:
- Generate ASCII/text representations of each screen
- Use boxes ([ ]) for buttons, pipes (|) for dividers and indentation for hierarchy

For each screen, document:
1. Screen name and purpose
2. Visual hierarchy: what is most important (largest/most prominent element)
3. Present components: headers, forms, lists, buttons, modals
4. Navigation: which screens can be reached from here and via which action

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada tela tem hierarquia visual clara, identificando qual elemento é o mais importante
- [ ] A navegação entre telas está documentada com as ações que disparam cada transição
- [ ] Cada elemento presente tem propósito identificado e vinculado a uma User Story
- [ ] O fluxo de navegação cobre todos os caminhos críticos da jornada do usuário
- [ ] O documento é suficiente para que um desenvolvedor ou designer reproduza a estrutura sem ambiguidade

## Boas práticas verificadas

- [ ] Hierarquia de informação segue princípios de UX (elemento principal em destaque, secundários subordinados)
- [ ] Componentes são nomeados de forma consistente entre telas (ex: sempre "barra de navegação", não "menu" em uma tela e "navbar" em outra)
- [ ] Fluxos de erro e estados vazios (empty states) estão documentados nas telas relevantes
- [ ] A documentação distingue claramente o que é conteúdo estático e o que é dinâmico
- [ ] Wireframe está versionado e vinculado às User Stories correspondentes
