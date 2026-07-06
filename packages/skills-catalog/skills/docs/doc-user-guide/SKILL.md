# Skill: Guia do Usuário

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar um guia do usuário que explica como usar o sistema passo a passo, com descrições visuais de cada funcionalidade, exemplos de uso real e respostas para as dúvidas mais comuns, escrito para o usuário final e não para o time técnico.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| funcionalidades | Sim | Lista de funcionalidades do sistema a documentar, com descrição de cada uma |
| user_stories | Sim | User Stories entregues na versão, usadas como base para os exemplos |
| perfil_usuario | Sim | Perfil (persona) do usuário final: nível técnico, contexto de uso, objetivos |
| capturas_tela | Não | Descrições textuais ou referências de telas para compor descrições visuais |
| duvidas_frequentes | Não | Perguntas reais levantadas em suporte, testes de usabilidade ou entrevistas |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/guia-do-usuario-v{versao}.md`
- **Campos obrigatórios:** introdução com propósito do guia, índice de funcionalidades, passo a passo por funcionalidade, exemplo de uso real para cada funcionalidade, glossário de termos técnicos, seção de perguntas frequentes com no mínimo cinco itens

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a user guide based on the provided context.

Rules:
- Write for the end user described in the user profile. Match their technical level — if they are non-technical, avoid all jargon.
- Every technical term must be explained in plain language at its first occurrence. Add a glossary at the end.
- Each feature section must include a real usage example (not hypothetical): describe who does what, in which context, and what the result looks like.
- Descriptions of screens and interactions must be textual but visual: describe what the user sees, where to click or type, and what happens next.
- The FAQ section must contain at least five real questions (sourced from support, user testing, or interviews when available).
- Structure: Introduction, Table of contents, Feature sections (each with steps + example), Glossary, FAQ.

Context:
- Features: {funcionalidades}
- User Stories delivered: {user_stories}
- User profile: {perfil_usuario}
- Screen descriptions: {capturas_tela}
- Frequently asked questions: {duvidas_frequentes}

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada funcionalidade tem um exemplo de uso real, não hipotético
- [ ] Todos os termos técnicos são explicados na primeira ocorrência do documento
- [ ] A seção de perguntas frequentes contém no mínimo cinco perguntas reais
- [ ] O guia pode ser lido e aplicado por um usuário do perfil descrito sem consultar suporte
- [ ] O índice de funcionalidades está presente e navegável

## Boas práticas verificadas

- [ ] Linguagem compatível com o nível técnico do perfil de usuário definido
- [ ] Passo a passo numerado para cada funcionalidade, com o que o usuário vê em cada etapa
- [ ] Exemplos baseados em situações reais de uso, não em cenários genéricos
- [ ] Glossário ao final com termos relevantes e definições em linguagem simples
- [ ] Perguntas da seção FAQ extraídas de fontes reais (suporte, testes, entrevistas)
