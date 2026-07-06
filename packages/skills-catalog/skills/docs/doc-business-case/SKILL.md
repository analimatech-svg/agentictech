# Skill: Business Case

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar um documento que justifica a existência de um projeto, apresentando o problema identificado, a oportunidade de negócio, o custo estimado, o benefício esperado, o ROI simplificado e as alternativas descartadas com motivo.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| contexto do problema | Sim | Descrição do problema atual que o projeto pretende resolver |
| objetivo do projeto | Sim | O que o projeto entrega e qual resultado mensurável ele produz |
| restrições conhecidas | Não | Restrições de prazo, orçamento, tecnologia ou equipe já identificadas |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/business-case.md`
- **Campos obrigatórios:** problema identificado, oportunidade de negócio, custo estimado, benefício esperado, ROI simplificado, alternativas descartadas (com motivo por alternativa), critério mensurável de sucesso

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a business case document based on the provided context.

Structure the document as follows:
1. Problem statement: describe the current problem clearly and specifically, avoiding generic descriptions.
2. Business opportunity: explain the value of solving this problem now.
3. Estimated cost: include team effort, infrastructure, and opportunity cost.
4. Expected benefit: quantify the benefit wherever possible (time saved, revenue gained, risk reduced).
5. Simplified ROI: present a straightforward calculation or qualitative comparison.
6. Discarded alternatives: list at least one alternative approach and explain why it was rejected.
7. Success criteria: define at least one measurable criterion to evaluate project success.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] O problema descrito é específico (não genérico), com contexto real e impacto identificável
- [ ] Há pelo menos uma alternativa descartada com motivo explícito
- [ ] O critério de sucesso é mensurável (tem número, prazo ou condição verificável)

## Boas práticas verificadas

- [ ] Governança (decisões registradas e rastreáveis)
- [ ] Documentação em Markdown (formatação consistente)
