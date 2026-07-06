# Skill: Base de Conhecimento

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Monitor |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Gerar artigos estruturados para uma base de conhecimento sobre o sistema, cobrindo como ele funciona, como resolver problemas comuns, como configurar funcionalidades e como tratar erros frequentes, organizados por categoria e com tags para facilitar a busca.

## Input

| Campo | Obrigatório | Descrição |
|---|---|---|
| perguntas_suporte | Sim | Perguntas frequentes recebidas pelo suporte, agrupadas por tema |
| incidentes_registrados | Não | Incidentes resolvidos que geraram aprendizado reutilizável |
| guia_do_usuario | Não | Guia do usuário existente, usado como base para artigos de como usar |
| categorias | Sim | Estrutura de categorias da base (ex: Como usar, Configuração, Resolução de problemas) |
| tags_existentes | Não | Tags já em uso na base, para manter consistência na taxonomia |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/kb/{categoria}/{slug-do-artigo}.md`
- **Campos obrigatórios:** título em forma de dúvida ou tarefa, categoria, tags, introdução de uma linha, corpo do artigo com passo a passo ou explicação, seção de troubleshooting (quando aplicável), artigos relacionados

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate knowledge base articles based on the provided context.

Rules:
- Each article title must reflect a real user question or task (e.g., "Como resetar minha senha", "Por que o relatório não carrega?") — never a technical concept as the title.
- Troubleshooting articles must follow the structure: Symptom, Probable cause, Solution (numbered steps).
- Each article must be self-contained: the reader should not need to read another article to resolve their issue.
- Tag each article with 3 to 5 relevant tags from the existing taxonomy. If a new tag is needed, propose it explicitly.
- Group articles into the defined categories. Suggest a new category only if none of the existing ones fits.
- Structure per article: Title, Category, Tags, One-line summary, Body (steps or explanation), Related articles.

Context:
- Support questions: {perguntas_suporte}
- Registered incidents: {incidentes_registrados}
- User guide: {guia_do_usuario}
- Categories: {categorias}
- Existing tags: {tags_existentes}

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada artigo tem título que corresponde a uma dúvida ou tarefa real do usuário, não a um conceito técnico
- [ ] Artigos de resolução de problemas seguem a estrutura sintoma, causa provável e solução passo a passo
- [ ] A base tem estrutura de categorias navegável e cada artigo está classificado corretamente
- [ ] Cada artigo é autocontido: o usuário resolve o problema sem precisar consultar outro artigo
- [ ] Tags estão presentes em todos os artigos e seguem a taxonomia definida

## Boas práticas verificadas

- [ ] Títulos orientados à tarefa ou dúvida real (não ao componente técnico)
- [ ] Artigos de troubleshooting com os três campos: sintoma, causa provável, solução numerada
- [ ] Artigos relacionados listados ao final para facilitar navegação contextual
- [ ] Taxonomia de tags consistente, sem duplicatas ou variações de grafia
- [ ] Base organizada em categorias claras, com quantidade equilibrada de artigos por categoria
