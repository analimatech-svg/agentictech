# Template de Contexto de Skill

> Use este template para criar o contexto especializado de qualquer skill nova.
> Contexto especializado é o que diferencia uma skill genérica de uma skill útil.

---

## Nome da skill

[nome-da-skill]

## Fase do SDLC

[planning | discovery | design | architecture | builder | validator | deploy | monitor | quality | docs]

## Nível Maestro mínimo

[Aprendiz | Praticante | Regente | Maestro]

---

## Contexto do domínio

[Descreva o domínio de conhecimento que a skill precisa ter para funcionar bem.
Exemplo para `doc-adr`: "Arquitetura de software orientada a decisões. O ADR registra o contexto,
as opções consideradas, a decisão tomada e as consequências esperadas.
Referências: Michael Nygard (ADRs), ThoughtWorks Technology Radar."]

---

## Input esperado

[O que o Consultor de IA precisa fornecer para a skill funcionar]

| Campo | Obrigatório | Descrição |
|---|---|---|
| [campo1] | Sim / Não | [descrição] |
| [campo2] | Sim / Não | [descrição] |

---

## Output gerado

[O que a skill entrega]

- Formato: [Markdown | JSON | código | HTML]
- Arquivo de destino: [caminho sugerido]
- Extensão: [.md | .json | .ts | .html]

---

## Critério de sucesso

[Como o Judge valida que o output está correto antes de apresentar ao Consultor de IA]

- [ ] [critério 1]
- [ ] [critério 2]
- [ ] [critério 3]

---

## Boas práticas aplicadas

[Quais das boas práticas de referência esta skill verifica ou aplica]

- [ ] DDD
- [ ] Clean Architecture
- [ ] SOLID
- [ ] Event-Driven Architecture
- [ ] Governança
- [ ] Observabilidade
- [ ] Documentação em Markdown

---

## Exemplos de uso

[Exemplos reais de como esta skill seria invocada pelo Judge]

**Exemplo 1 (Aprendiz):**
Input: "Quero registrar por que escolhemos o Supabase em vez do Firebase"
Output: ADR-001 com contexto, opções e consequências em linguagem acessível

**Exemplo 2 (Regente):**
Input: "Documente todas as decisões de arquitetura do projeto de autenticação"
Output: Série de ADRs numerados com rastreabilidade cruzada

---

## Referências

[Links, documentos ou padrões de mercado usados como base]

- [referência 1]
- [referência 2]
