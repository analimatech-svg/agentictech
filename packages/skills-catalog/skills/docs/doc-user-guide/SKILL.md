# Skill: Guia do Usuário

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar um guia do usuário que explica como usar o sistema passo a passo, com descrições visuais de cada funcionalidade, exemplos de uso real e respostas para as dúvidas mais comuns, escrito para o usuário final e não para o time técnico.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| funcionalidades | lista de strings | Sim | Lista de funcionalidades do sistema a documentar, com descrição de cada uma |
| user_stories | string (Markdown) | Sim | User Stories entregues na versão, usadas como base para os exemplos |
| perfil_usuario | string | Sim | Perfil (persona) do usuário final: nível técnico, contexto de uso, objetivos |
| capturas_tela | lista de strings | Não | Descrições textuais ou referências de telas para compor descrições visuais |
| duvidas_frequentes | lista de strings | Não | Perguntas reais levantadas em suporte, testes de usabilidade ou entrevistas |

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

ANTI-PATTERNS — apply blocking rules:
- BLOCKED if: any feature section describes only what the feature does without a concrete usage example showing a real person in a real context
  ❌ "A funcionalidade de exportação permite baixar dados em formato CSV."
  ✅ "Exemplo de uso: Maria, analista financeira, precisa enviar o relatório de vendas de junho para o contador. Ela acessa o módulo Relatórios, seleciona o período de 01/06 a 30/06, clica em 'Exportar' e escolhe 'CSV'. O arquivo é baixado automaticamente na pasta Downloads com o nome 'relatorio-vendas-2026-06.csv'."
- BLOCKED if: steps are written in passive voice or from the system's perspective instead of the user's perspective
  ❌ "O sistema exibe um formulário de cadastro. Os campos são preenchidos e o botão de confirmar é acionado."
  ✅ "1. Na tela inicial, clique em 'Criar conta'. 2. Preencha seu nome, e-mail e uma senha com no mínimo 8 caracteres. 3. Clique em 'Confirmar'. Você receberá um e-mail de verificação em até 2 minutos."
- BLOCKED if: a technical term appears in the body without an explanation at first occurrence and without a glossary entry
  ❌ "Clique em 'Autenticar via OAuth' para conectar sua conta."
  ✅ "Clique em 'Autenticar via OAuth' (OAuth é um padrão de segurança que permite conectar contas sem compartilhar sua senha) para conectar sua conta. Veja também: Glossário — OAuth."
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
