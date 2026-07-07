# Skill: Relatório de Bug

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Validator |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar um relatório de bug estruturado, com passos reproduzíveis, severidade justificada e impacto claro para o usuário.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| descricao_bug | string | Sim | Descrição do comportamento inesperado observado |
| passos_observados | lista de strings | Sim | O que o testador fez até encontrar o problema |
| ambiente | string | Sim | Sistema operacional, navegador ou versão do app, ambiente (staging, produção) |
| evidencias | lista de strings | Não | Log de erro, mensagem exibida na tela, screenshot ou gravação de tela |
| requisito_referencia | string | Não | ID da User Story ou requisito que define o comportamento correto |

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

ANTI-PATTERNS — apply blocking rules:
- BLOCKED if: the steps to reproduce do not start from a known, explicit system state
  ❌ "1. Acessar o sistema. 2. Fazer login. 3. O erro aparece."
  ✅ "Pré-condição: usuário com perfil 'Gerente' logado, sem pedidos em aberto. 1. Acessar Menu > Pedidos > Novo Pedido. 2. Adicionar 3 itens ao carrinho (SKU-001, SKU-002, SKU-003). 3. Clicar em 'Finalizar Pedido'. Resultado: tela congela e exibe a mensagem 'Erro interno 500' após 8 segundos."
- BLOCKED if: the severity has no justification grounded in user impact or occurrence frequency
  ❌ "Severidade: Alta."
  ✅ "Severidade: Alta — justificativa: bloqueia o fluxo principal de criação de pedidos para todos os usuários com perfil 'Gerente' (aproximadamente 40 usuários ativos). Ocorre em 100% das tentativas no ambiente de staging. Sem workaround disponível."
- BLOCKED if: the expected behavior is stated as personal opinion rather than tied to a requirement or acceptance criterion
  ❌ "Comportamento esperado: o sistema deveria aceitar o pedido normalmente."
  ✅ "Comportamento esperado: conforme critério de aceitação da US-142 — 'Dado que o usuário adiciona itens com estoque disponível, quando clicar em Finalizar Pedido, então o pedido é criado com status Pendente e o usuário é redirecionado para a tela de confirmação'."
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
