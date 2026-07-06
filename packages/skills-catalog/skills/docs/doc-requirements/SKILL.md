# Skill: Requisitos

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Discovery |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar um documento de requisitos funcionais e não funcionais do sistema, garantindo que cada requisito tenha critério de aceite mensurável e que os requisitos não funcionais contenham valores numéricos verificáveis.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| relatório de discovery | Sim | Achados da fase de discovery: contexto, problemas identificados e oportunidades |
| entrevistas ou notas de pesquisa | Não | Transcrições ou resumos de entrevistas com usuários e stakeholders |
| personas | Não | Perfis de usuário que consomem ou operam o sistema |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/requirements.md`
- **Campos obrigatórios:** lista de requisitos funcionais (cada um com ID, descrição e critério de aceite), lista de requisitos não funcionais (cada um com ID, categoria, valor numérico alvo e método de verificação)

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a requirements document based on the provided context.

Structure the document as follows:
1. Functional requirements: numbered list (RF-001, RF-002, ...) where each item includes:
   - Description: what the system must do.
   - Acceptance criterion: a testable condition that confirms the requirement is met.
2. Non-functional requirements: numbered list (RNF-001, RNF-002, ...) where each item includes:
   - Category: performance, security, scalability, maintainability, or other.
   - Description: what quality attribute must be achieved.
   - Target value: a numeric threshold (response time in ms, availability in %, error rate in %, etc.).
   - Verification method: how to confirm the requirement is satisfied.

Rules:
- No requirement may be ambiguous. Each must be testable as written.
- Non-functional requirements must contain numeric values — no vague terms like "fast" or "reliable."
- Each functional requirement must have exactly one acceptance criterion.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada requisito funcional tem critério de aceite mensurável e testável
- [ ] Os requisitos não funcionais têm valores numéricos (tempo de resposta em ms, disponibilidade em %, etc.)
- [ ] Nenhum requisito contém termos ambíguos que admitam interpretações diferentes

## Boas práticas verificadas

- [ ] Governança (decisões registradas e rastreáveis)
- [ ] Documentação em Markdown (formatação consistente)
