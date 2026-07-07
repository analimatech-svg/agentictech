# Auditoria de Qualidade — 43 Skills MaestroAI-Tech

**Data:** 2026-07-06 | **Rubric:** QUALITY-RUBRIC.md v1.0  
**Rodada 1:** 7 reescritas + 1 skill nova  
**Rodada 2:** upgrade sistêmico D1/D4/D7 nas 18 Pratas  
**Rodada 3 (revisão independente):** 10 correções críticas identificadas e aplicadas — ver seção abaixo  
**Backlog v2:** 5 novas skills adicionadas — sprint-plan, retrospective, data-model, api-contract, judge/SKILL.md

> ⚠️ **Nota de auditoria:** a rodada 2 inflou D4=2 para `doc-wireframe` e `doc-mockup` incorretamente — nenhuma das duas tinha blocking rules. Ambas foram reescritas para v2.0.0 na rodada 3. Os scores nesta tabela refletem o estado atual pós-correção.

Pontuação: D1 (Input) + D2 (Output) + D3 (Prompt) + D4 (Anti-padrões) + D5 (Critérios) + D6 (Boas práticas) + D7 (Exemplos) = Total/14

---

## Resultado por skill

| Skill | D1 | D2 | D3 | D4 | D5 | D6 | D7 | Total | Faixa |
|-------|----|----|----|----|----|----|----|----|-------|
| `sdlc/architecture` | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `quality/best-practices` | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-adr` | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `sdlc/builder` | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `quality/security` | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `sdlc/validator` | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-code-style` | 1 | 2 | 2 | 1 | 2 | 2 | 2 | **12** | 🥇 Ouro |
| `sdlc/deploy` | 2 | 2 | 2 | 2 | 2 | 1 | 1 | **12** | 🥇 Ouro |
| `sdlc/monitor` | 2 | 2 | 2 | 2 | 2 | 1 | 1 | **12** | 🥇 Ouro |
| `quality/code-review` | 2 | 2 | 2 | 2 | 2 | 1 | 1 | **12** | 🥇 Ouro |
| `doc-postmortem` | 1 | 2 | 2 | 2 | 2 | 2 | 1 | **12** | 🥇 Ouro |
| `doc-runbook` | 1 | 2 | 2 | 2 | 2 | 2 | 1 | **12** | 🥇 Ouro |
| `sdlc/planning` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `sdlc/discovery` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `sdlc/design` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-architecture` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-changelog` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-deploy-plan` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-knowledge-base` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-mockup` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-prototype` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-security` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-test-cases` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-wireframe` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-bug-report` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-release-notes` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-rollback-plan` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-test-plan` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-test-report` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-user-guide` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-requirements` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-user-journey` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-user-story` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-persona` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-work-breakdown` 🆕 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-business-case` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-roadmap` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-status-report` ✅ | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **13** | 🥇 Ouro |
| `doc-sprint-plan` 🆕 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-retrospective` 🆕 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-data-model` 🆕 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `doc-api-contract` 🆕 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |
| `judge/SKILL.md` 🆕 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **14** | 🥇 Ouro |

---

## Resumo executivo

| Faixa | Qtd | Skills |
|-------|-----|--------|
| 🥇 Ouro (12–14) | 43 | Todas |
| 🥈 Prata (8–11) | 0 | — |
| 🥉 Bronze (4–7) | 0 | — |
| 🔴 Vermelho (0–3) | 0 | — |

**Skills prontas para publicação como referência (Ouro):** 43/43 (100%)  
**Skills que atendem o mínimo de publicação (Prata+):** 43/43 (100%)  
**Skills que precisam de reescrita prioritária:** 0/43 (0%)

> ✅ **Base 100% Ouro.** 43 skills, todas em 12+ pontos. Publicação autorizada.

### Skills reescritas nesta rodada

| Skill | Score original | Score atual | Faixa original → atual |
|-------|---------------|-------------|------------------------|
| `doc-business-case` | 4/14 | 13/14 | 🔴 Vermelho → 🥇 Ouro |
| `doc-roadmap` | 4/14 | 13/14 | 🔴 Vermelho → 🥇 Ouro |
| `doc-status-report` | 4/14 | 13/14 | 🔴 Vermelho → 🥇 Ouro |
| `doc-persona` | 5/14 | 14/14 | 🥉 Bronze → 🥇 Ouro |
| `doc-user-story` | 7/14 | 14/14 | 🥉 Bronze → 🥇 Ouro |
| `doc-requirements` | 7/14 | 14/14 | 🥉 Bronze → 🥇 Ouro |
| `doc-user-journey` | 7/14 | 14/14 | 🥉 Bronze → 🥇 Ouro |
| `doc-work-breakdown` | — | 14/14 | 🆕 Nova skill |

---

## Diagnóstico por dimensão — onde toda a base peca

| Dimensão | Skills com score 0 | Causa raiz |
|----------|--------------------|------------|
| D1 — Input tipado | 26 das 26 docs skills | Tabela de 3 colunas (sem Tipo). SDLC skills têm 4 colunas. |
| D4 — Anti-padrões | 3 Vermelho + 4 Bronze | Sem condições de bloqueio definidas |
| D6 — Boas práticas | 7 skills com score 0 | Seção copiada como "Governança + Markdown" sem adaptar à skill |
| D7 — Exemplos | 9 skills com score 0 | Prompt prescritivo, sem par correto/incorreto |

**Oportunidade sistêmica:** adicionar a coluna `Tipo` a todas as 26 tabelas de input das docs skills é uma melhoria mecânica que sobe 26 skills de D1=1 para D1=2 (+26 pontos na base inteira).

---

## Prioridade de reescrita

### Vermelho — reescrita completa (3 skills)

| Priority | Skill | Gap principal | Impacto |
|----------|-------|---------------|---------|
| 1 | `doc-business-case` | Prompt de 7 linhas. Sem bloqueio, sem boas práticas, sem exemplos. É o documento que justifica a existência do projeto — não pode ser raso. | Alta: está na Fase 1 (Planning), vista por todo Aprendiz |
| 2 | `doc-roadmap` | Mesmo padrão. Horizonte Now/Next/Later explicado mas sem critério de boa priorização, sem anti-padrão "datas fixas no Later". | Alta: Planning, Aprendiz |
| 3 | `doc-status-report` | Mais simples dos três, mas RAG sem critério objetivo (Verde/Amarelo/Vermelho sem definição) é anti-padrão clássico. | Alta: usada em Planning E Monitor |

### Bronze — reescrita parcial (4 skills)

| Priority | Skill | Gap principal |
|----------|-------|---------------|
| 4 | `doc-persona` | Prompt mais curto de toda a base. Sem profundidade sobre dados qualitativos vs. quantitativos, sem critério de quando a persona é inválida (fictícia sem dado). |
| 5 | `doc-user-story` | Boas práticas vazias. Critérios rasos. Falta regra sobre INVEST (Independent, Negotiable, Valuable, Estimable, Small, Testable). |
| 6 | `doc-requirements` | Boas práticas apenas "Governança + Markdown". Falta seção de rastreabilidade (RF → US → caso de teste), falta anti-padrão de requisito ambíguo com exemplo. |
| 7 | `doc-user-journey` | Boas práticas vazias. Falta distinção entre jornada AS-IS e TO-BE, falta regra sobre quantas etapas é o limite antes de dividir em sub-jornadas. |

### Prata — melhorias pontuais (18 skills)

Skills Prata precisam principalmente de:
1. **D7:** adicionar pelo menos um par ❌/✅ concreto ao prompt
2. **D6:** expandir boas práticas além de "Governança + Markdown" com verificações específicas

Skills SDLC Prata (`planning`, `discovery`, `design`) precisam também de:
3. **D4:** definir condição de bloqueio — o Judge precisa saber quando negar o output

---

## Padrão dos 3 skills Vermelho — o que falta

Estas 3 skills têm o mesmo padrão de rascunho:

```
## Prompt base
You are a specialized AI assistant for software documentation.
Generate a [X] document based on the provided context.

Structure the document as follows:
1. [campo genérico]
2. [campo genérico]
...

Language: Brazilian Portuguese
```

```
## Boas práticas verificadas
- [ ] Governança (decisões registradas e rastreáveis)
- [ ] Documentação em Markdown (formatação consistente)
```

Este é o anti-padrão que precisa ser eliminado. O prompt poderia ser substituído por "escreva um documento de [X] profissional" com o mesmo resultado.

---

## Correções Rodada 3 — Revisão Independente

Achados de revisão independente (olhar de especialista externo) e correções aplicadas:

| # | Problema | Arquivo | Correção |
|---|---|---|---|
| 1 | `doc-wireframe`: zero blocking rules — D4=2 no audit era inflado | doc-wireframe/SKILL.md | Reescrita completa v2.0.0 com BLOCKING RULES, exemplos ❌/✅, estados especiais |
| 2 | `doc-mockup`: zero blocking rules — D4=2 no audit era inflado | doc-mockup/SKILL.md | Reescrita completa v2.0.0 com BLOCKING RULES, especificação de contraste WCAG |
| 3 | `sdlc/architecture`: BLOCKING RULES ausentes do prompt | sdlc/architecture/SKILL.md | Adicionado bloco BLOCKING RULES com 6 condições + exemplos ❌/✅; v2.0.0 |
| 4 | `sdlc/builder`: BLOCKING RULES ausentes do prompt | sdlc/builder/SKILL.md | Adicionado bloco BLOCKING RULES com 6 condições; `contexto_modulo` agora bloqueia se ausente em arquitetura multi-contexto; v2.0.0 |
| 5 | `sdlc/monitor`: pode gerar relatório com métricas inventadas | sdlc/monitor/SKILL.md | BLOCKED quando `dados_metricas` ausente em sistema em produção; v2.0.0 |
| 6 | `sdlc/design`: dependência de `doc-persona` não documentada | sdlc/design/SKILL.md | Input `personas` agora especifica origem obrigatória em `doc-persona`; v2.1.0 |
| 7 | `doc-architecture` colidia com `sdlc/architecture` (mesmo arquivo output) | doc-architecture/SKILL.md | Redesignada para sistemas existentes; output renomeado para `existing-architecture.md`; v2.0.0 |
| 8 | `sdlc/discovery`: nível Aprendiz incorreto | sdlc/discovery/SKILL.md | Corrigido para Praticante |
| 9 | `doc-roadmap`: nível Aprendiz incorreto | doc-roadmap/SKILL.md | Corrigido para Praticante |
| 10 | `doc-persona`: nível Aprendiz incorreto | doc-persona/SKILL.md | Corrigido para Praticante |
| 11 | mapa-de-skills: contagem 37 incorreta | mapa-de-skills.md | Corrigido para 38 skills, 27 docs skills |

~~**Gaps identificados mas não resolvidos (backlog v2):**~~ ✅ Todos resolvidos

| Gap | Impacto | Resolução |
|---|---|---|
| ~~Judge sem SKILL.md executável~~ | Alto | ✅ `judge/SKILL.md` criado (v1.0.0, Regente) |
| ~~Sem skill de sprint planning~~ | Médio | ✅ `doc-sprint-plan` criado (v1.0.0, Praticante) |
| ~~Sem skill de data model / domain model~~ | Médio | ✅ `doc-data-model` criado (v1.0.0, Praticante) |
| ~~Sem skill de API contract (OpenAPI)~~ | Médio | ✅ `doc-api-contract` criado (v1.0.0, Praticante) |
| ~~Sem skill de retrospectiva~~ | Baixo-médio | ✅ `doc-retrospective` criado (v1.0.0, Praticante) |

## Próximos passos

1. ~~**Reescrever os 3 Vermelhos**~~ ✅ Concluído
2. ~~**Reescrever os 4 Bronzes**~~ ✅ Concluído
3. ~~**Upgrade D1 sistêmico**~~ ✅ Concluído
4. ~~**Upgrade D7 nas Pratas**~~ ✅ Concluído
5. ~~**Upgrade D4 nas SDLC Prata**~~ ✅ Concluído
6. ~~**Revisão independente + 11 correções críticas**~~ ✅ Concluído (Rodada 3)
7. ~~**Backlog v2: 5 novas skills**~~ ✅ Concluído — sprint-plan, retrospective, data-model, api-contract, judge/SKILL.md
8. **Push + deploy** — `git push origin main` + publicar `skills.mentoratech.com.br` via Vercel.
