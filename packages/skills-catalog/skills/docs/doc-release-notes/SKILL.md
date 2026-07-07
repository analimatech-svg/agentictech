# Skill: Release Notes

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Deploy / Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar um comunicado claro e acessível sobre uma nova versão do sistema, destacando o que mudou, o que foi corrigido e o que ainda está em fase experimental, escrito para o usuário final e não para o time técnico.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| changelog_tecnico | lista de strings | Sim | Lista de mudanças na versão, gerada pelo time de desenvolvimento |
| versao | string | Sim | Número da versão no formato semântico (ex: 2.3.1) |
| data_lancamento | string | Sim | Data de lançamento da versão (formato AAAA-MM-DD) |
| publico_alvo | enum: usuário final \| parceiros \| interno | Sim | Destinatário do comunicado: usuário final, parceiros ou interno |
| itens_alpha_beta | lista de strings | Não | Lista de funcionalidades em fase experimental com aviso de estabilidade |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/release-notes-v{versao}.md`
- **Campos obrigatórios:** número da versão, data, seção "O que há de novo", seção "O que foi corrigido", seção "Em desenvolvimento" (quando aplicável), aviso de estabilidade para itens alpha e beta

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a release notes document based on the provided context.

Rules:
- Write for the end user, not for the development team.
- Replace every technical term with plain language. If a technical term is unavoidable, explain it in parentheses.
- For each changed or fixed item, describe the benefit to the user, not just what was done.
- Items labeled alpha or beta must include a clear stability warning (e.g., "Esta funcionalidade está em fase experimental e pode apresentar instabilidades").
- Use three sections: "O que há de novo", "O que foi corrigido", "Em desenvolvimento" (only if there are alpha/beta items).
- Tone: friendly, direct, accessible.

Context:
- Version: {versao}
- Release date: {data_lancamento}
- Target audience: {publico_alvo}
- Technical changelog: {changelog_tecnico}
- Alpha/beta items: {itens_alpha_beta}

Language: Brazilian Portuguese

ANTI-PATTERNS — apply blocking rules:
- BLOCKED if: any item describes what the team did instead of what the user gains
  ❌ "Refatoramos o módulo de autenticação para usar JWT."
  ✅ "O login agora é mais rápido e sua sessão permanece ativa por até 7 dias sem precisar entrar com senha novamente."
- BLOCKED if: a technical term appears without plain-language explanation
  ❌ "Corrigimos a race condition no endpoint de webhooks."
  ✅ "Corrigimos uma falha que causava duplicação de notificações quando dois eventos chegavam ao mesmo tempo (sincronização de dados)."
- BLOCKED if: an alpha or beta item has no explicit stability warning
  ❌ "Nova funcionalidade: exportação para PDF (beta)."
  ✅ "Nova funcionalidade: exportação para PDF — BETA. Esta funcionalidade está em fase experimental e pode apresentar instabilidades. Use em ambientes não críticos e reporte problemas pelo canal de suporte."
```

## Critério de sucesso

- [ ] Nenhum item do comunicado contém jargão técnico incompreensível para o usuário final
- [ ] Cada mudança ou correção menciona o benefício para o usuário, não apenas a ação executada
- [ ] Funcionalidades em alpha ou beta têm aviso explícito de estabilidade
- [ ] O documento é legível em menos de três minutos
- [ ] As três seções estão presentes e organizadas de forma navegável

## Boas práticas verificadas

- [ ] Linguagem acessível, sem siglas ou nomes de componentes internos expostos
- [ ] Benefício do usuário explicitado em cada item (não "corrigimos o bug X", mas "você não verá mais o erro X ao fazer Y")
- [ ] Versão e data visíveis no topo do documento
- [ ] Itens alpha e beta agrupados em seção separada com aviso de estabilidade
- [ ] Público-alvo definido e respeitado ao longo de todo o texto
