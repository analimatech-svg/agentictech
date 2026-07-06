# Skill: Roadmap

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Planning |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar um roadmap de produto ou projeto organizado por fases (Now/Next/Later ou por sprints/trimestres), mostrando o que está em execução, o que vem a seguir e o horizonte de longo prazo.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| business case aprovado | Sim | Resumo do problema e dos objetivos que o projeto deve atingir |
| escopo definido | Sim | Lista de funcionalidades, entregas ou épicos priorizados |
| restrições de prazo | Não | Datas fixas, marcos contratuais ou janelas de lançamento conhecidas |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/roadmap.md`
- **Campos obrigatórios:** seção Now (em execução), seção Next (próximo ciclo), seção Later (horizonte longo), responsável ou squad por item, critério de conclusão por fase

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a product or project roadmap based on the provided context.

Structure the document using three horizons:
- Now: what is currently in progress, with responsible team or squad and expected completion criteria.
- Next: what comes after the current cycle, with enough detail to plan but not over-committed.
- Later: long-term intentions that are intentionally vague — no fixed dates, no specific scope commitments.

Rules:
- Every item must have a responsible owner (person, squad, or team).
- Every phase must have a clear completion criterion.
- Items in the Later horizon must remain vague on purpose — avoid fixed dates or detailed scope for this horizon.
- Do not add items without an identified owner.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada item do roadmap tem responsável ou squad identificado
- [ ] Cada fase tem critério de conclusão explícito
- [ ] Os itens do horizonte Later são intencionalmente vagos, sem datas fixas ou escopo detalhado

## Boas práticas verificadas

- [ ] Governança (decisões registradas e rastreáveis)
- [ ] Documentação em Markdown (formatação consistente)
