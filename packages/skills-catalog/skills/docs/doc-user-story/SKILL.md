# Skill: User Story

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Design |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar uma User Story no formato "Como [persona], quero [ação], para [benefício]" acompanhada de critérios de aceite em Gherkin (Dado que / Quando / Então), cobrindo o caminho feliz e pelo menos um cenário de exceção.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| persona | Sim | Perfil do usuário que a história representa |
| funcionalidade a descrever | Sim | A capacidade do sistema que a história deve capturar |
| contexto do sistema | Não | Informações sobre regras de negócio, fluxos existentes ou restrições técnicas relevantes |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/user-story-[id].md`
- **Campos obrigatórios:** ID da história, título, declaração no formato "Como / Quero / Para", critérios de aceite em Gherkin (mínimo 2 cenários: caminho feliz e exceção), estimativa de tamanho (Story Points ou P/M/G), dependências identificadas

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a user story based on the provided persona and feature context.

Structure the document as follows:
1. Story ID and title: a short descriptive title.
2. Story statement: "As a [persona], I want to [action], so that [benefit]." The benefit must be meaningful — not just a restatement of the action.
3. Acceptance criteria in Gherkin format:
   - Scenario 1: happy path — the standard successful flow.
   - Scenario 2: exception — a failure, edge case, or validation error.
   - Additional scenarios as needed.
   Each scenario follows the pattern: Given [precondition] / When [action] / Then [expected outcome].
4. Size estimate: Story Points (1, 2, 3, 5, 8) or T-shirt size (S/M/L/XL) with a brief rationale.
5. Dependencies: other stories, systems, or decisions this story depends on.

Rules:
- The story must be small enough to be completed within a single sprint.
- Every acceptance criterion must be testable and automatable — avoid criteria that require human judgment to verify.
- The exception scenario must cover a realistic failure case, not a trivially unlikely one.
- The benefit in the story statement must explain why the user cares, not just what they want.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] A história é pequena o suficiente para ser entregue em um único sprint
- [ ] Há pelo menos dois cenários Gherkin: o caminho feliz e um cenário de exceção realista
- [ ] Todos os critérios de aceite são testáveis e automatizáveis sem julgamento subjetivo

## Boas práticas verificadas

- [ ] Governança (decisões registradas e rastreáveis)
- [ ] Documentação em Markdown (formatação consistente)
