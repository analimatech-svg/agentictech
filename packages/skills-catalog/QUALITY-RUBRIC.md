# Rubric de Qualidade — Skills MaestroAI-Tech

**Versão:** 1.0 | **Data:** 2026-07-06

Critério de avaliação para garantir que toda skill do registry MaestroAI-Tech tenha profundidade profissional.  
Cada skill é pontuada em 7 dimensões (0–2 cada). Máximo: 14 pontos.

---

## Por que este rubric existe

Skills genéricas são "apenas um chat com passo extra" (anti-padrão registrado no escopo do MaestroAI-Tech).  
Uma skill profissional ensina o Consultor de IA a estruturar o problema, dirigir a IA com precisão e validar o resultado — não apenas a descrever o que quer.

O padrão de referência interno são as skills `sdlc/architecture`, `quality/best-practices` e `doc-adr`.  
O padrão externo de referência é a skill de acessibilidade da TechLeads Club (WCAG, tabelas de severidade, ❌/✅ exemplos concretos).

---

## As 7 Dimensões

### D1 — Tabela de Input (0–2)

| Pontos | Critério |
|--------|---------|
| 2 | Tabela com 4 colunas: Campo / Tipo / Obrigatoriedade / Descrição. Campos têm nomes específicos (não genéricos como "contexto"). |
| 1 | Tabela presente mas sem coluna Tipo, ou com 3 colunas, ou com campo genérico "contexto" sem nome específico. |
| 0 | Sem tabela de input, ou apenas lista de parâmetros sem estrutura. |

---

### D2 — Definição de Output (0–2)

| Pontos | Critério |
|--------|---------|
| 2 | Output lista as seções obrigatórias do documento final com nomes concretos, OR descreve os campos exatos esperados com formato definido (tabela, checklist, etc.). |
| 1 | Output tem formato e nome de arquivo sugerido, mas os campos são vagos ou apenas listados sem estrutura. |
| 0 | Output diz apenas "documento em Markdown" sem especificar seções ou campos. |

---

### D3 — Profundidade do Prompt (0–2)

| Pontos | Critério |
|--------|---------|
| 2 | Prompt tem seções numeradas com requisitos por seção, regras explícitas de formatação e instruções para casos específicos (ex: o que fazer quando um campo está vazio, como nomear IDs, como estruturar a tabela de severidade). |
| 1 | Prompt tem alguma estrutura mas as seções são curtas, genéricas ou não especificam o formato exato de cada seção. |
| 0 | Prompt curto sem estrutura, apenas descreve o objetivo em prosa. |

---

### D4 — Anti-padrões e Condições de Bloqueio (0–2)

| Pontos | Critério |
|--------|---------|
| 2 | Prompt ou critérios incluem condições explícitas de BLOQUEIO (ex: "se X está ausente, retorne BLOCKED"), OU lista pelo menos 3 anti-padrões com o que fazer em vez (❌ → ✅). |
| 1 | Tem algumas regras negativas ("nunca X", "não inclua Y") mas sem critério objetivo de bloqueio e sem exemplos de correção. |
| 0 | Sem regras negativas, sem condições de bloqueio, sem menção a anti-padrões. |

---

### D5 — Especificidade dos Critérios de Sucesso (0–2)

| Pontos | Critério |
|--------|---------|
| 2 | 4 ou mais critérios, todos mensuráveis e objetivos (ex: "ao menos 2 cenários Gherkin", "valor numérico em ms ou %", "nenhum hash de commit visível"). |
| 1 | 2–3 critérios, ao menos um mensurável. |
| 0 | 0–1 critério, ou todos vagos (ex: "o documento está completo"). |

---

### D6 — Cobertura das Boas Práticas (0–2)

| Pontos | Critério |
|--------|---------|
| 2 | 4 ou mais itens específicos referenciando princípios concretos (DDD, Clean Architecture, SOLID, Event-Driven, Governança, Observabilidade, Markdown) com verificação operacional (ex: "ADRs referenciados estão vinculados às decisões citadas"). |
| 1 | 2–3 itens além de "Governança" e "Documentação em Markdown". |
| 0 | Apenas "Governança" + "Documentação em Markdown" (ou menos). Indica que a seção não foi pensada para a skill. |

---

### D7 — Exemplos Concretos (0–2)

| Pontos | Critério |
|--------|---------|
| 2 | Prompt inclui exemplos de correto vs. incorreto (❌/✅), template completo do output, ou pares antes/depois concretos. |
| 1 | Prompt inclui alguma estrutura de template mas sem contraste explícito de correto vs. incorreto. |
| 0 | Sem exemplos. Tudo prescritivo em prosa. |

---

## Faixas de Qualidade

| Faixa | Pontos | Diagnóstico | Ação |
|-------|--------|-------------|------|
| **Ouro** | 12–14 | Profundidade profissional. Pode ser publicada como referência. | Manutenção pontual |
| **Prata** | 8–11 | Estrutura correta, falta profundidade em 2–3 dimensões. | Melhoria direcionada |
| **Bronze** | 4–7 | Estrutura básica, prompt raso, boas práticas genéricas. | Reescrita parcial |
| **Vermelho** | 0–3 | Skill de primeiro rascunho. Não atinge padrão profissional. | Reescrita completa |

---

## O que distingue Ouro de Prata

Uma skill Prata tem estrutura correta mas falha em pelo menos dois destes pontos:

1. **D4 = 0:** não define quando o output deve ser bloqueado ou rejeitado
2. **D6 = 0:** boas práticas são só "Governança" + "Markdown", sem verificação operacional
3. **D7 = 0:** prompt não tem nenhum exemplo concreto — tudo prescritivo em prosa
4. **D1 = 1:** tabela de input sem coluna Tipo (não diferencia `string` de `lista` de `objeto`)

Uma skill Vermelho ou Bronze é aquela onde o prompt pode ser substituído por "faça um documento profissional sobre X" sem perda de qualidade.

---

## Como aplicar ao criar uma skill nova

Antes de commitar uma skill nova, verifique:

- [ ] D1: a tabela de input tem 4 colunas (Campo / Tipo / Obrigatoriedade / Descrição)
- [ ] D2: o output lista as seções ou campos obrigatórios com nomes concretos
- [ ] D3: o prompt tem seções numeradas e regras por seção (não só em prosa)
- [ ] D4: existe pelo menos uma condição de bloqueio ou 3 anti-padrões com correção
- [ ] D5: há pelo menos 4 critérios de sucesso mensuráveis
- [ ] D6: as boas práticas cobrem pelo menos 4 dos 7 princípios com verificação específica
- [ ] D7: o prompt tem pelo menos um exemplo concreto de correto vs. incorreto

**Score mínimo para publicação: 10 pontos (Prata).**
Skills com score abaixo de 10 entram em revisão antes de integrar ao registry.
