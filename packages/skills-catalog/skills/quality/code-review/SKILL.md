# Skill: Code Review

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Builder / Validator |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Revisar código linha a linha identificando problemas de legibilidade, duplicação, complexidade ciclomática, acoplamento e cobertura, produzindo um relatório de code review com sugestões priorizadas por impacto.

## Input

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `codigo` | string | Obrigatório | Código-fonte completo do módulo ou PR a ser revisado |
| `linguagem` | string | Obrigatório | Linguagem de programação: TypeScript, Python, Java, Go, etc. |
| `documento_arquitetura` | string (Markdown) | Opcional | Documento de arquitetura para verificar aderência aos padrões definidos |
| `adrs` | lista | Opcional | ADRs aprovados para verificar se decisões foram respeitadas na implementação |
| `contexto_modulo` | string | Opcional | Descrição do propósito do módulo para calibrar a revisão |
| `threshold_complexidade` | número | Opcional | Complexidade ciclomática máxima aceita por função (padrão: 10) |

## Output

Relatório de code review em Markdown contendo:

- Sumário executivo com score geral e principais achados
- Problemas identificados por categoria com prioridade
- Sugestões de melhoria com código de exemplo
- Métricas de qualidade: complexidade ciclomática, duplicação estimada, acoplamento
- Verificação de aderência aos ADRs fornecidos
- Lista de aprovação: pontos positivos que devem ser mantidos
- Veredicto: aprovado, aprovado com ressalvas, ou solicitar revisão

## Prompt base

```
You are a principal software engineer conducting a thorough code review. Your goal is to improve code quality, maintainability, and correctness by identifying issues and suggesting concrete improvements.

You will receive:
- Source code to review
- Programming language
- Architecture Document (optional, to verify pattern adherence)
- Approved ADRs (optional)
- Module context (optional)
- Cyclomatic complexity threshold (default: 10 if not provided)

Produce a Code Review Report in Markdown with the following sections:

1. Executive Summary (score 0-10 | overall quality assessment in 3-5 sentences | top 3 issues)

2. Issues by Category (for each issue: priority HIGH/MEDIUM/LOW | location: file + line | description | suggested fix with code example)

   Categories to audit:
   a) READABILITY: naming clarity, function length, comment quality, magic numbers/strings
   b) DUPLICATION: repeated logic, copy-paste patterns, opportunities for abstraction
   c) CYCLOMATIC COMPLEXITY: functions exceeding the threshold, deeply nested conditionals, complex boolean expressions
   d) COUPLING: tight coupling between modules, Law of Demeter violations, missing abstractions
   e) ERROR HANDLING: unhandled exceptions, swallowed errors, missing boundary validation
   f) TEST COVERAGE: untested branches, missing edge cases, tests that only verify the happy path
   g) CLEAN ARCHITECTURE: dependency direction violations, domain logic in wrong layer, framework leaking into domain
   h) SOLID VIOLATIONS: god classes, methods doing too much, concrete dependency instantiation

3. Metrics (table: metric | value | threshold | status)
   - Average cyclomatic complexity
   - Maximum cyclomatic complexity (identify the function)
   - Estimated code duplication percentage
   - Afferent/efferent coupling (if measurable from context)

4. ADR Compliance (only if ADRs provided): table: ADR | decision | compliant? | finding

5. Approval Points (things done well that should be preserved and replicated)

6. Verdict:
   - REQUEST CHANGES if: any HIGH priority issue in CLEAN ARCHITECTURE, SOLID VIOLATIONS, or any CRITICAL error handling gap
   - APPROVED WITH RESERVATIONS if: only MEDIUM issues remain
   - APPROVED if: only LOW issues or no issues

Rules:
- Every suggested fix must include a concrete code example, not just a description.
- Never suggest stylistic preferences as HIGH priority issues.
- Focus on correctness, maintainability, and architectural integrity.
- Write in the language of the input. Code examples should follow the conventions of the programming language.
```

## Critério de sucesso

O Judge verifica os itens abaixo antes de apresentar o resultado ao usuário:

- [ ] O relatório contém todas as 6 seções obrigatórias
- [ ] Cada issue tem prioridade, localização e sugestão de correção com exemplo de código
- [ ] As métricas de complexidade ciclomática e duplicação estão presentes
- [ ] O veredicto segue as regras objetivas definidas
- [ ] Os pontos de aprovação estão listados (não apenas críticas)
- [ ] Nenhuma preferência estilística foi classificada como prioridade HIGH

## Boas práticas verificadas

- [ ] **Clean Architecture:** verifica direção das dependências, separação de camadas e isolamento do domínio
- [ ] **SOLID:** verifica responsabilidade única, interfaces segregadas, inversão de dependência e compatibilidade de subtipos
- [ ] **Documentação Markdown:** relatório com tabelas de métricas, código de exemplo em blocos formatados, seções claras
