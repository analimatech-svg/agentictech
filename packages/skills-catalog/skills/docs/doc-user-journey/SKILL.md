# Skill: Jornada do Usuário

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Discovery |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar o mapeamento da jornada do usuário descrevendo cada etapa da interação com o sistema: ação realizada, pensamento, emoção, ponto de atrito e oportunidade de melhoria.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| personas | Sim | Perfis de usuário cujas jornadas serão mapeadas |
| fluxos do sistema | Sim | Sequências de telas, passos ou interações que o sistema oferece atualmente ou planeja oferecer |
| pontos de contato identificados | Não | Canais, interfaces ou momentos em que o usuário interage com o produto |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/user-journey-[persona].md`
- **Campos obrigatórios:** persona de referência, objetivo da jornada, tabela de etapas (etapa, ação, pensamento, emoção, ponto de atrito, oportunidade), resumo dos principais atritos e oportunidades

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a user journey map based on the provided personas and system flows.

Structure the document as follows:
1. Header: which persona this journey belongs to and what goal the user is trying to accomplish.
2. Journey table with columns: Stage, Action (what the user does), Thought (what the user is thinking), Emotion (how the user feels — use a consistent scale: frustrated, neutral, satisfied, delighted), Friction point (what makes this stage harder than it should be), Improvement opportunity (a specific, actionable change that would reduce friction).
3. Summary: top 3 friction points and top 3 opportunities, ordered by impact.

Rules:
- Every stage must have both an action and an emotion — stages with only actions are incomplete.
- Friction points must be specific: "the confirmation email takes 4 minutes to arrive" instead of "the process is complex."
- Improvement opportunities must be actionable: a team could start working on them immediately based on the description alone.
- Emotions must reflect what the user actually feels, not what we wish they felt.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada etapa da jornada tem ação e emoção associadas, sem exceção
- [ ] Os pontos de atrito são específicos e contextualizados, não genéricos
- [ ] As oportunidades de melhoria são acionáveis: uma equipe consegue agir sobre elas com a descrição fornecida

## Boas práticas verificadas

- [ ] Governança (decisões registradas e rastreáveis)
- [ ] Documentação em Markdown (formatação consistente)
