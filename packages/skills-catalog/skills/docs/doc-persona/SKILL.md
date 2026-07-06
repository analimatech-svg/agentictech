# Skill: Persona

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Discovery |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar uma persona que representa um tipo de usuário do sistema, baseada em dados reais de entrevistas ou pesquisa, descrevendo perfil, objetivos, frustrações, comportamentos e uma citação representativa.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| entrevistas com usuários ou dados de pesquisa | Sim | Transcrições, notas de entrevista ou dados quantitativos que embasam a persona |
| contexto do produto | Sim | O que o sistema faz e quem são os usuários esperados |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/persona-[nome].md`
- **Campos obrigatórios:** nome fictício, perfil demográfico, cargo ou contexto de uso, objetivos principais, frustrações específicas, comportamentos observados, citação representativa, fonte dos dados (entrevista, pesquisa quantitativa, observação direta)

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a user persona based on the provided research data.

Structure the document as follows:
1. Name and thumbnail description: a realistic fictional name and a one-sentence summary.
2. Demographic profile: age range, role, context of system use.
3. Goals: what this user type is trying to achieve when using the system.
4. Frustrations: specific pain points observed in research — not generic complaints.
5. Observed behaviors: how this user type actually acts, based on the provided data.
6. Representative quote: a realistic quote that captures this persona's mindset, written as if spoken by a real person.
7. Data source: cite the type of research or observation that grounds this persona.

Rules:
- Every frustration must be specific — "the search takes too long to return results" instead of "the system is slow."
- Behaviors must be observed or inferred from data — not invented.
- The representative quote must sound like a real person, not a marketing tagline.
- Do not create a persona based on assumptions without data to support it.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] A persona tem pelo menos uma dor específica e não genérica, com contexto de quando ela ocorre
- [ ] Os comportamentos descritos são observados ou inferidos de dados reais, não inventados
- [ ] A citação representativa soa como algo que uma pessoa real diria, sem linguagem de marketing

## Boas práticas verificadas

- [ ] Governança (decisões registradas e rastreáveis)
- [ ] Documentação em Markdown (formatação consistente)
