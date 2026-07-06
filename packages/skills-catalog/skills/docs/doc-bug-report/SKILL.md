# Skill: Relatório de Bug

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Validator |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar um relatório de bug estruturado, com passos reproduzíveis, severidade justificada e impacto claro para o usuário.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| descricao_bug | Sim | Descrição do comportamento inesperado observado |
| passos_observados | Sim | O que o testador fez até encontrar o problema |
| ambiente | Sim | Sistema operacional, navegador ou versão do app, ambiente (staging, produção) |
| evidencias | Não | Log de erro, mensagem exibida na tela, screenshot ou gravação de tela |
| requisito_referencia | Não | ID da User Story ou requisito que define o comportamento correto |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/bug-report-NNN.md`
- **Campos obrigatórios:** título descritivo, severidade com justificativa, passos para reproduzir, comportamento esperado, comportamento atual, ambiente, impacto no usuário

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a structured bug report based on the provided information.

Include the following fields:
- Title: descriptive, starting with the affected component (ex: "Login - Usuário não recebe email de confirmação após cadastro")
- Severity: Crítica / Alta / Média / Baixa, with justification based on user impact and occurrence frequency
- Steps to reproduce: numbered, deterministic, starting from a known system state
- Expected behavior: based on requirement or acceptance criteria, not personal opinion
- Current behavior: exactly what happens, with error messages verbatim
- Evidence: logs, screenshots or recordings
- Environment: OS, browser/app version, test environment
- User impact: who is affected and what they cannot do

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Os passos para reproduzir são determinísticos: qualquer pessoa consegue reproduzir o bug seguindo os passos sem informação adicional
- [ ] O comportamento esperado está baseado em um requisito, critério de aceitação ou User Story identificada, não em opinião pessoal
- [ ] A severidade tem justificativa explícita relacionada ao impacto no usuário ou no negócio

## Boas práticas verificadas

- [ ] O título identifica o componente afetado e o comportamento observado sem ambiguidade
- [ ] Os passos começam a partir de um estado conhecido do sistema (ex: "Dado que o usuário está na tela de login, com sessão encerrada")
- [ ] Mensagens de erro são transcritas literalmente, sem paráfrase
- [ ] O campo de impacto no usuário descreve quem é afetado (todos os usuários, usuários com perfil X, usuários em determinado fluxo) e o que eles ficam impedidos de fazer
