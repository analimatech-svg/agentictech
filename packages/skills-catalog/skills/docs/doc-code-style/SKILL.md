# Skill: Guia de Estilo de Código

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Architecture |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar o guia de estilo de código do time, definindo convenções de nomenclatura, formatação, estrutura de arquivos, padrão de commits (Conventional Commits), regras de lint e princípios de legibilidade que garantam consistência e manutenibilidade ao longo do projeto.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| stack_tecnologica | Sim | Linguagens e frameworks adotados (ex: TypeScript, React, Node.js, Python, Go) |
| ferramentas_lint | Não | Ferramentas de análise estática disponíveis (ESLint, Prettier, Black, golangci-lint, etc.) |
| preferencias_time | Não | Decisões já tomadas pelo time: aspas simples ou duplas, espaços ou tabs, tamanho de linha |
| contexto_projeto | Não | Tipo de projeto (API REST, frontend, CLI, biblioteca) para ajustar as convenções relevantes |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/code-style.md`
- **Campos obrigatórios:** convenções de nomenclatura, formatação com exemplos corretos e incorretos, estrutura de arquivos e diretórios, padrão de commits (Conventional Commits), regras de lint principais, seção de justificativas para regras restritivas

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a code style guide based on the provided stack and team preferences.

Structure:
1. NAMING CONVENTIONS: Rules for variables, functions, classes, files, constants and interfaces.
   For each: rule description + correct example + incorrect example

2. FORMATTING: Indentation (spaces vs tabs, amount), maximum line length, quotes (single vs double), semicolons, trailing commas.
   For each: rule + correct example + incorrect example

3. FILE AND DIRECTORY STRUCTURE: How to organize files in the project. Include a representative example tree.

4. COMMIT STANDARD (Conventional Commits):
   - Types: feat, fix, docs, refactor, test, chore, perf, ci
   - Format: type(scope): short description in imperative
   - Examples: correct and incorrect
   - Breaking changes: how to flag with BREAKING CHANGE footer

5. MAIN LINT RULES: The 5 to 10 most important rules configured in the linting tool, with rationale

6. RATIONALE SECTION: For the most restrictive rules, explain why the team adopted them. Developers follow rules they understand better than rules they just obey.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada regra de nomenclatura e formatação tem exemplo de código correto E incorreto
- [ ] O padrão de commit está especificado com tipos (fix:, feat:, docs:, refactor:, test:, chore:, perf:)
- [ ] A seção de justificativas explica o "por que" das regras mais restritivas ou menos óbvias
- [ ] A estrutura de diretórios está representada com uma árvore de exemplo comentada
- [ ] As regras de lint estão listadas com o nome da regra na ferramenta e o motivo de sua ativação

## Boas práticas verificadas

- [ ] Conventional Commits: formato está especificado com exemplos para mudanças que quebram compatibilidade (BREAKING CHANGE)
- [ ] SOLID: regras de nomenclatura incentivam nomes que revelam intenção (Clean Code, Ubiquitous Language do DDD)
- [ ] Legibilidade: o guia prioriza código que se lê como prosa, não código que demonstra esperteza
- [ ] Configuração automatizada: regras que podem ser verificadas por lint estão na ferramenta, não apenas no documento
- [ ] O guia está versionado junto ao código e é atualizado quando o time toma novas decisões de estilo
