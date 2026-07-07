# Skill: Changelog de Release

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Builder / Deploy |
| Categoria | docs |
| Nível mínimo Maestro | Aprendiz |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar o changelog de uma release seguindo o padrão Keep a Changelog, com linguagem acessível a não-técnicos e versionamento SemVer.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| lista_commits | lista de strings | Sim | Lista de commits no formato Conventional Commits (feat, fix, chore, refactor, etc.) |
| versao_anterior | string | Sim | Versão que estava em produção antes desta release (ex: 1.4.2) |
| versao_atual | string | Sim | Versão que será publicada nesta release (ex: 1.5.0) |
| data_release | string | Sim | Data de publicação no formato YYYY-MM-DD |
| nome_produto | string | Não | Nome do produto ou sistema, para contextualizar o documento |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/CHANGELOG.md`
- **Campos obrigatórios:** cabeçalho com versão e data, seções preenchidas (Added, Changed, Deprecated, Removed, Fixed, Security), rodapé com link para comparação de versões quando aplicável

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a changelog entry based on the provided commit list.

Rules:
- Follow the Keep a Changelog format (keepachangelog.com)
- Organize entries by section: Added, Changed, Deprecated, Removed, Fixed, Security
- Omit empty sections entirely
- Rewrite each commit message in plain language, readable by non-technical stakeholders
- Remove commit hashes, branch names, and internal jargon
- Version must follow SemVer (MAJOR.MINOR.PATCH)
- Each entry starts with a verb in the past tense (ex: Adicionado, Corrigido, Removido)

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada entrada é compreensível por alguém sem conhecimento técnico do sistema
- [ ] Seções vazias estão omitidas (não aparecem com "nenhuma mudança")
- [ ] A versão segue o padrão SemVer (MAJOR.MINOR.PATCH) sem sufixos informais
- [ ] A data de release está no formato ISO 8601 (YYYY-MM-DD)
- [ ] Nenhum hash de commit, nome de branch ou referência de ticket aparece nas entradas

## Boas práticas verificadas

- [ ] Entradas escritas do ponto de vista do usuário, não do desenvolvedor
- [ ] Mudanças com impacto em segurança aparecem na seção Security, mesmo que também sejam correções
- [ ] Versões mais recentes aparecem no topo do arquivo
- [ ] Há um link para comparação entre a versão anterior e a atual (quando o repositório é acessível)
