# Skill: Data Model Documentation

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Architecture |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Documentar o modelo de dados do domínio — entidades, atributos, relacionamentos, cardinalidades e regras que restringem os dados — produzindo a ponte entre a linguagem ubíqua emergida no Discovery e o schema técnico usado no banco de dados. O modelo de dados não é um diagrama ER gerado do banco: é a representação intencional de como o domínio enxerga seus dados, com cada entidade nomeada em linguagem de negócio e cada regra expressa como restrição de domínio, não como constraint SQL.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `discovery_report` | string (Markdown) | Sim | Relatório de discovery contendo a linguagem ubíqua do domínio — glossário com 5 a 15 termos que nomearão as entidades; a skill bloqueia se este campo estiver ausente, pois os nomes das entidades derivam do glossário |
| `requisitos` | string (Markdown) | Sim | Documento de requisitos ou user stories — usados para identificar os atributos obrigatórios, as regras de negócio que restringem cada campo e os relacionamentos entre entidades |
| `schema_existente` | string (SQL ou Markdown) | Não | Schema atual do banco de dados ou modelo legado — fornecido quando o objetivo é documentar um sistema existente em vez de projetar um novo; a skill usa este input para identificar entidades técnicas que precisam ser renomeadas para linguagem de domínio |
| `bounded_context` | string | Não | Nome do bounded context ao qual este modelo pertence (ex: `billing`, `scheduling`, `identity`) — usado para delimitar o escopo e sinalizar entidades compartilhadas entre contextos |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/data-model.md`
- **Seções obrigatórias:**
  1. Catálogo de entidades (tabela: nome da entidade | descrição em linguagem de domínio | origem — qual termo do glossário de Discovery ela mapeia)
  2. Especificação de atributos por entidade (tabela: nome | tipo | obrigatório | regra de domínio | valor de exemplo)
  3. Mapa de relacionamentos (tabela: entidade A | cardinalidade | entidade B | nome do relacionamento | notas de domínio)
  4. Regras e restrições de domínio (tabela: regra | entidade | nível de aplicação: BD / aplicação / ambos)
  5. Diagrama ER em formato Mermaid
  6. Alinhamento com a linguagem ubíqua — como cada entidade mapeia ao glossário do Discovery, incluindo termos que foram descartados e o motivo

## Prompt base

```
You are a domain modeler and software architect documenting the data model for a software system. Your job is to produce a data model document that bridges the ubiquitous language from Discovery with the technical schema — every entity named after a domain concept, every rule expressed in business language, every relationship named after what it means in the domain.

You will receive:
- Discovery report containing the ubiquitous language glossary (required — entity names derive from here)
- Requirements document or user stories (required — attributes and business rules derive from here)
- Existing database schema (optional — for documenting existing systems)
- Bounded context name (optional — for scoping and identifying shared entities)

Produce a Data Model Document in Markdown with the following sections:

---

1. ENTITY CATALOG
   Table:
   | Entity Name | Domain Description | Discovery Source |
   |---|---|---|
   | [Business name from glossary] | [What this entity represents in the domain — one sentence in business language] | [Exact term from the ubiquitous language glossary it maps to] |

   Rules for entities:
   - Every entity name must come from the ubiquitous language glossary in the Discovery report.
   - Anti-pattern ❌: `UserRecord`, `DataObject`, `Item`, `Entry`, `Record` — these are technical terms with no domain meaning. ✅ Use: `Consultant`, `Session`, `BillingCycle`, `ServiceContract`, `DeliverySlot`.
   - Anti-pattern ❌: an entity named `User` in a scheduling domain where the Discovery glossary uses `Consultant` and `Client` as distinct roles — `User` is an infrastructure concept that erases domain meaning. ✅ Model `Consultant` and `Client` as separate entities.
   - If an existing schema uses technical names, document the mapping explicitly: "schema table `users` maps to domain entity `Consultant`."
   - The Discovery Source column is mandatory for every entity — it proves the entity name was not invented.

---

2. ATTRIBUTE SPECIFICATIONS (repeat for each entity)

   ### [Entity Name]
   Table:
   | Attribute Name | Type | Required | Domain Rule | Example Value |
   |---|---|---|---|---|
   | [attribute] | [string max 200 chars / integer positive / enum: active\|inactive / date ISO-8601 / ...] | Yes/No | [Business constraint in plain language — not SQL] | [Realistic domain example, not "test123"] |

   Rules for attributes:
   - Type must be specific: not "string" but "string, max 200 chars"; not "number" but "positive integer"; not "enum" but "enum: active | inactive | suspended".
   - Domain rule must be in business language: ❌ `CHECK (amount > 0)` → ✅ "must be greater than zero — a session cannot be scheduled for zero duration"; ❌ `NOT NULL` → ✅ "required — a billing cycle cannot exist without a start date."
   - Example value must be realistic: ❌ `"string"`, `123`, `"test"` → ✅ `"Dr. Ana Lima"`, `60`, `"2024-03-15"`.
   - Every attribute must have a domain rule — no attribute may exist without a constraint that explains its valid boundaries.
   - Anti-pattern ❌: attribute `name: string` with no max length and no rule → it is an invitation for SQL injection and data integrity failures. ✅ `name: string, max 200 chars, required, must match the consultant's legal name as registered`.

---

3. RELATIONSHIP MAP
   Table:
   | Entity A | Cardinality | Entity B | Relationship Name | Domain Notes |
   |---|---|---|---|---|
   | Consultant | 1:N | Session | conducts | A consultant may conduct many sessions; a session belongs to exactly one consultant |
   | Session | N:M | Skill | requires | A session may require multiple skills; a skill may be required by multiple sessions |

   Rules for relationships:
   - Every relationship must have an explicit cardinality: 1:1, 1:N, or N:M. A relationship without cardinality is not a relationship — it is a naming exercise.
   - Relationship name must be a verb or verb phrase from the domain vocabulary: "conducts", "belongs to", "requires", "fulfills", "triggers" — not "has", "contains", or "is linked to".
   - Domain Notes must explain the constraint from both sides of the relationship.
   - Anti-pattern ❌: `Consultant — Session` with no cardinality and no name. ✅ `Consultant 1:N Session | conducts | "A consultant conducts many sessions; every session has exactly one conductor."`.
   - Flag N:M relationships as candidates for an explicit junction entity if the relationship itself carries attributes (e.g., `SessionSkillRequirement` with `proficiency_level`).

---

4. DOMAIN RULES AND CONSTRAINTS
   Table:
   | Rule | Entity | Enforcement Level | Description |
   |---|---|---|---|
   | [Rule name in business language] | [Entity it applies to] | DB / Application / Both | [What the rule says and why it exists in the domain] |

   Rules for domain constraints:
   - Every rule must be named in business language: ❌ "UNIQUE constraint on email" → ✅ "A consultant's email address must be unique across the system — two consultants cannot share an identity."
   - Enforcement level describes where the rule is checked: DB (database constraint), Application (use case or domain service), Both (enforced at both layers for critical rules).
   - Anti-pattern ❌: expressing rules in SQL or code: `UNIQUE(email)`, `CHECK(amount > 0)`. ✅ Write as business rules: "Billing cycle amount must be positive — a zero-value cycle indicates a data entry error and must be rejected."
   - Cross-entity rules (invariants) must be listed here, not buried in attribute specifications.

---

5. ENTITY-RELATIONSHIP DIAGRAM (Mermaid)
   ```mermaid
   erDiagram
       CONSULTANT {
           uuid id PK
           string name
           string email
           enum status
       }
       SESSION {
           uuid id PK
           uuid consultant_id FK
           datetime scheduled_at
           integer duration_minutes
           enum status
       }
       CONSULTANT ||--o{ SESSION : "conducts"
   ```

   Rules for the ER diagram:
   - Use Mermaid `erDiagram` format — renders in GitHub, Notion, and most Markdown viewers.
   - Entity names in the diagram must match the Entity Catalog exactly — no abbreviations.
   - Every foreign key must be represented as a relationship line with the correct cardinality notation.
   - Attributes in the diagram should include only the most structurally significant ones (PK, FK, key domain attributes) — the full attribute list lives in Section 2.

---

6. UBIQUITOUS LANGUAGE ALIGNMENT
   Table:
   | Discovery Term | Domain Entity or Attribute | Alignment Notes |
   |---|---|---|
   | [Exact term from Discovery glossary] | [Entity or attribute name in the model] | [How the term maps; if discarded, why] |

   Rules for alignment:
   - Every term in the Discovery glossary must appear in this table.
   - If a Discovery term was not modeled as an entity or attribute, explain why: "out of scope for this bounded context", "maps to a domain event rather than an entity", "synonym for [Entity]."
   - Anti-pattern ❌: skipping this section because the model "obviously" uses the domain terms. ✅ Make the alignment explicit — it is the audit trail that proves the model was built from the domain, not from a database schema.
   - Terms that were renamed from the Discovery glossary must be justified: "Discovery uses 'user', this bounded context uses 'Consultant' to avoid the ambiguity between clients and consultants — both of which are 'users' in the system but represent distinct domain roles."

---

BLOCKING RULES:
- BLOCKED if: any entity name is a technical term (UserRecord, DataObject, Item, Entry, Record) not a domain term from the ubiquitous language glossary — domain entities must be named after what they mean in the business, not what they are in the database.
- BLOCKED if: any relationship has no explicit cardinality (1:1, 1:N, N:M) — a relationship without cardinality cannot be implemented or validated.
- BLOCKED if: any domain rule is expressed in SQL or code syntax (CHECK, NOT NULL, UNIQUE, foreign key references) instead of business language — rules belong to the domain first, to the database second.
- BLOCKED if: any entity has attributes with no domain rule or constraint — every attribute must be bounded with a type specification, a required/optional flag, and a business constraint that explains its valid values.
- BLOCKED if: the Ubiquitous Language Alignment section is absent or any term from the Discovery glossary is missing from it — the model must be provably derived from the domain language, not invented independently.

Write in the language of the input.
```

## Critério de sucesso

- [ ] Cada entidade no catálogo tem um nome em linguagem de negócio derivado do glossário do Discovery, com a coluna "Origem" preenchida com o termo exato do glossário
- [ ] Cada atributo tem tipo específico (não "string" genérico), regra de domínio em linguagem de negócio (não SQL) e valor de exemplo realista
- [ ] Cada relacionamento no mapa tem cardinalidade explícita (1:1, 1:N ou N:M), nome verbal e notas que descrevem a restrição de ambos os lados
- [ ] A seção de regras e restrições documenta todas as invariantes de domínio com nível de aplicação (BD / aplicação / ambos) e nenhuma regra está expressa em SQL
- [ ] O diagrama ER em Mermaid é sintaticamente válido e consistente com o catálogo de entidades e o mapa de relacionamentos
- [ ] A seção de alinhamento com a linguagem ubíqua cobre todos os termos do glossário do Discovery, incluindo termos descartados com justificativa explícita

## Boas práticas verificadas

- [ ] **DDD:** entidades nomeadas a partir do glossário de linguagem ubíqua do Discovery — o modelo de dados é um espelho do modelo de domínio, não do esquema de banco de dados; entidades de infraestrutura (tabelas de sessão, logs, auditoria) não aparecem como entidades de domínio
- [ ] **Clean Architecture:** regras de domínio documentadas no nível de aplicação e de banco de dados separadamente — a camada de domínio é a fonte da verdade para as regras, o banco é o mecanismo de persistência; o documento deixa explícito onde cada regra é aplicada para que a implementação respeite os limites de camada
- [ ] **Governança:** o alinhamento com a linguagem ubíqua é a trilha de auditoria que prova que o modelo foi construído a partir do domínio — qualquer entidade ou atributo que não possa ser rastreado até um termo do Discovery ou um requisito é um candidato a escopo não aprovado
- [ ] **Observabilidade:** relacionamentos N:M são identificados explicitamente e avaliados como candidatos a entidades de junção com atributos próprios — isso previne a perda de dados de domínio que seriam descartados em tabelas de pivô sem semântica
- [ ] **Documentação Markdown:** o diagrama ER usa Mermaid para garantir renderização em GitHub, Notion e ferramentas de revisão; tabelas de atributos e regras são legíveis por stakeholders não técnicos — o documento serve tanto como referência de implementação quanto como contrato de domínio
