# Skill: Casos de Teste

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Validator |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar os casos de teste derivados dos critérios de aceitação Gherkin das User Stories, cobrindo fluxos felizes, cenários de erro e edge cases.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| user_stories | Sim | User Stories com critérios de aceitação no formato Gherkin (Given/When/Then) |
| plano_testes | Sim | Plano de testes com escopo e tipos de teste definidos |
| sistema_descricao | Não | Descrição do comportamento atual do sistema, para identificar regressões possíveis |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/test-cases.md`
- **Campos obrigatórios:** ID do caso, descrição, pré-condição, passos de execução numerados, resultado esperado, resultado real (coluna para preenchimento durante execução), status (Passou / Falhou / Bloqueado)

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate test cases derived from the provided Gherkin acceptance criteria.

For each Gherkin scenario, produce at least one test case.
Also generate additional cases for:
- Error flows (invalid inputs, system unavailable, unauthorized access)
- Edge cases (empty values, maximum limits, special characters)
- Regression scenarios for adjacent features

Each test case must include:
- ID: TC-NNN (sequential)
- Description: one sentence explaining what is being tested
- Pre-condition: system state required before execution
- Steps: numbered, deterministic actions (no ambiguity)
- Expected result: observable and verifiable outcome
- Actual result: empty field for execution time
- Status: empty field (Passou / Falhou / Bloqueado)

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada critério Gherkin tem pelo menos um caso de teste correspondente identificado pelo ID da User Story
- [ ] Os casos incluem cenários de erro (entrada inválida, falha de sistema, acesso não autorizado)
- [ ] Os casos incluem edge cases (campos vazios, valores no limite máximo e mínimo, caracteres especiais)
- [ ] Os passos de execução são determinísticos: qualquer testador consegue reproduzir o mesmo resultado seguindo os passos
- [ ] O formato permite automatização futura (passos são ações específicas, sem ambiguidade)

## Boas práticas verificadas

- [ ] IDs são sequenciais e únicos em todo o documento
- [ ] Pré-condições descrevem o estado do sistema e dos dados, não apenas "estar logado"
- [ ] Resultado esperado é observável (o que aparece na tela, o que é retornado pela API, o que é gravado no banco)
- [ ] Casos de teste de performance e segurança são separados dos funcionais por seção
