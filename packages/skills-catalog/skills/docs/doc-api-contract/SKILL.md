# Skill: API Contract Specification

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Architecture / Builder |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.0.0 |

## Objetivo

Produzir a especificação de contrato de API para interfaces RESTful ou orientadas a eventos — indo além do boilerplate OpenAPI para documentar a estratégia de versionamento, a taxonomia de erros, o modelo de autenticação, os limites de rate limiting e a política de breaking changes. Um contrato de API é um compromisso público: qualquer consumidor (interno ou externo) que ler este documento deve ser capaz de integrar, lidar com falhas e planejar migrações sem precisar ler o código ou abrir um chamado.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `requisitos` | string (Markdown) | Sim | Documento de requisitos, user stories ou ADRs que descrevem as capacidades que a API deve expor — usado para derivar o catálogo de endpoints e os schemas de request/response |
| `arquitetura` | string (Markdown) | Sim | Documento de arquitetura ou design do sistema — usado para determinar a estratégia de versionamento, o modelo de autenticação e os limites de rate limiting compatíveis com a infraestrutura existente |
| `versao_anterior` | string (Markdown ou OpenAPI YAML) | Não | Contrato da versão anterior da API — fornecido para geração do changelog e verificação de breaking changes; obrigatório quando `versao_atual` for maior que `v1` |
| `tipo_api` | string | Sim | Tipo da interface: `rest` (HTTP/JSON RESTful) ou `event-driven` (mensageria assíncrona via Kafka, RabbitMQ, SNS/SQS ou similar) — determina o formato do catálogo de endpoints e dos schemas |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/api-contract.md`
- **Seções obrigatórias:**
  1. Visão geral da API (base URL, estratégia de versionamento, método de autenticação, limites de rate limiting globais)
  2. Catálogo de endpoints (endpoint | método HTTP ou tópico/evento | propósito | autenticação obrigatória | rate limit específico)
  3. Schemas de request/response por endpoint (com tipos de campo, restrições e valores de exemplo)
  4. Taxonomia de erros (códigos HTTP usados + códigos de erro de domínio com descrição + exemplo de resposta de erro)
  5. Política de breaking changes (o que constitui uma breaking change, cronograma de deprecação, mecanismo de notificação aos consumidores)
  6. Changelog (o que mudou nesta versão em relação à anterior)

## Prompt base

```
You are a senior API architect producing a complete API contract specification. Your contract is a public commitment — any consumer reading this document must be able to integrate, handle failures, and plan migrations without reading the source code.

You will receive:
- Requirements document or user stories (required — endpoints and schemas derive from here)
- Architecture document (required — versioning strategy, auth model, and rate limits derive from here)
- Previous version contract (optional — required when current version > v1; used for changelog and breaking change detection)
- API type: rest | event-driven

Produce an API Contract Specification in Markdown with the following sections:

---

1. API OVERVIEW
   | Field | Value |
   |---|---|
   | API Name | [Name in domain language] |
   | Base URL | [e.g., https://api.example.com/v1] |
   | API Type | REST / Event-Driven |
   | Current Version | [e.g., v1, v2.1] |
   | Versioning Strategy | [URL path versioning: /v1/ /v2/ OR Header versioning: Accept: application/vnd.example.v2+json — specify exactly which, never both] |
   | Authentication | [Token type: Bearer JWT / API Key / OAuth2 — specify header name, token expiration, and refresh mechanism] |
   | Rate Limit (global) | [e.g., 1000 requests/minute per API key — specify the window and the counter scope] |
   | Rate Limit Headers | [Headers returned: X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset] |
   | Base Error Format | [JSON structure used for all error responses — defined in Section 4] |

   Rules for the overview:
   - Anti-pattern ❌: versioning described as "we follow REST best practices" — this conveys nothing to a consumer. ✅ Specify the exact mechanism: "URL path versioning. The version is the first path segment after the base domain: /v1/consultants. To upgrade to v2, change the path prefix — no header or query param is needed."
   - Anti-pattern ❌: authentication described as "use a token" or "Bearer token". ✅ Specify: "Bearer JWT. Include the token in the Authorization header: `Authorization: Bearer <token>`. Tokens expire after 1 hour. Refresh using POST /auth/refresh with a valid refresh_token."
   - Rate limit must specify the window (per second / per minute / per hour), the scope (per API key / per IP / per user), and what happens when the limit is exceeded (HTTP 429 with Retry-After header).

---

2. ENDPOINT CATALOG
   Table:
   | Endpoint | Method | Purpose | Auth Required | Rate Limit | Idempotent |
   |---|---|---|---|---|---|
   | /consultants | GET | List all active consultants with pagination | Yes — Bearer JWT | 100 req/min per key | Yes |
   | /consultants/{id} | GET | Retrieve a single consultant by ID | Yes — Bearer JWT | 100 req/min per key | Yes |
   | /consultants | POST | Create a new consultant | Yes — Bearer JWT, scope: write:consultants | 10 req/min per key | No |
   | /sessions/{id}/cancel | POST | Cancel a scheduled session | Yes — Bearer JWT | 20 req/min per key | Yes |

   Rules for the endpoint catalog:
   - For event-driven APIs, replace Method with "Producer / Consumer" and Endpoint with "Topic / Exchange / Queue name".
   - Purpose must describe what the consumer achieves, not what the server does: ❌ "Calls the consultant service and returns the data" → ✅ "Retrieve a single consultant's profile, skills, and availability status by their unique ID."
   - Auth Required must specify the exact scope if OAuth2 is used.
   - Idempotent: document whether repeating the same request produces the same result — critical for retry logic.

---

3. REQUEST/RESPONSE SCHEMAS (repeat for each endpoint)

   ### [Method] [Endpoint] — [Purpose]

   #### Request
   ```json
   {
     "field_name": "example_value"
   }
   ```
   | Field | Type | Required | Constraint | Example |
   |---|---|---|---|---|
   | [field] | [string max 200 chars / integer 1–10000 / enum: active\|inactive / ISO-8601 datetime / uuid / boolean] | Yes/No | [Business constraint in plain language] | [Realistic example] |

   #### Response — 200 OK
   ```json
   {
     "field_name": "example_value"
   }
   ```
   | Field | Type | Description | Example |
   |---|---|---|---|
   | [field] | [specific type] | [What this field means to the consumer] | [Realistic example] |

   #### Response — Error cases
   | Status | Error Code | When it occurs |
   |---|---|---|
   | 400 | VALIDATION_ERROR | Required field missing or field fails constraint |
   | 404 | CONSULTANT_NOT_FOUND | No consultant exists with the provided ID |
   | 429 | RATE_LIMIT_EXCEEDED | Consumer exceeded 100 req/min |

   Rules for schemas:
   - Anti-pattern ❌: field `name: string` with no constraint. ✅ `name: string, max 200 chars, required, must be the consultant's legal name — cannot contain special characters except hyphens and apostrophes for compound names`.
   - Anti-pattern ❌: field `amount: number`. ✅ `amount: positive integer, cents (not dollars), min 1, max 1000000 — represents the session fee in the smallest currency unit to avoid floating-point rounding errors`.
   - Every endpoint that can fail must document how it fails — the error cases table is mandatory for every endpoint. An endpoint with no error documentation is a trap for consumers.
   - Example values must be realistic and domain-consistent: ❌ `"string"`, `0`, `"test@test.com"` → ✅ `"Dr. Ana Lima"`, `9000`, `"ana.lima@maestroai.tech"`.

---

4. ERROR TAXONOMY

   #### HTTP Status Codes Used by This API
   | Status Code | Semantics | When Used |
   |---|---|---|
   | 200 OK | Success with response body | GET, successful POST returning resource |
   | 201 Created | Resource created successfully | POST that creates a new resource |
   | 204 No Content | Success with no response body | DELETE, PUT with no return value |
   | 400 Bad Request | Consumer error — malformed request or validation failure | Missing required field, field fails constraint |
   | 401 Unauthorized | Missing or invalid authentication credentials | No Authorization header, expired token |
   | 403 Forbidden | Valid credentials but insufficient permissions | Consumer authenticated but lacks required scope |
   | 404 Not Found | Resource does not exist | ID not found in the system |
   | 409 Conflict | Operation cannot proceed due to current resource state | Canceling a session that is already completed |
   | 422 Unprocessable Entity | Syntactically valid request but semantically invalid in the domain | Scheduling a session in the past |
   | 429 Too Many Requests | Rate limit exceeded | Consumer exceeded the configured rate limit |
   | 500 Internal Server Error | Unexpected server-side failure | Unhandled exception — consumer should retry with backoff |

   #### Domain Error Codes
   Table:
   | Error Code | HTTP Status | Description | Resolution for Consumer |
   |---|---|---|---|
   | CONSULTANT_NOT_FOUND | 404 | No consultant exists with the provided ID | Verify the ID before sending — list consultants using GET /consultants |
   | SESSION_ALREADY_CANCELLED | 409 | The session was already cancelled | No action needed — the desired state is already reached |
   | INVALID_SCHEDULE_WINDOW | 422 | Session start time is in the past or conflicts with an existing booking | Use GET /consultants/{id}/availability to check open slots |
   | RATE_LIMIT_EXCEEDED | 429 | Consumer has exceeded their rate limit for this endpoint | Inspect the Retry-After header and wait before retrying |

   #### Standard Error Response Format
   All error responses use this JSON structure:
   ```json
   {
     "error": {
       "code": "CONSULTANT_NOT_FOUND",
       "message": "No consultant found with ID 'f47ac10b-58cc-4372-a567-0e02b2c3d479'.",
       "timestamp": "2024-03-15T10:30:00Z",
       "request_id": "req_8f3kL9mNpQ"
     }
   }
   ```
   | Field | Type | Description |
   |---|---|---|
   | error.code | string | Machine-readable domain error code from the taxonomy above |
   | error.message | string | Human-readable explanation — safe to display to end users, contains no internal detail |
   | error.timestamp | ISO-8601 datetime | When the error occurred on the server |
   | error.request_id | string | Correlation ID — include this in support requests to trace the specific call |

---

5. BREAKING CHANGE POLICY

   #### What Constitutes a Breaking Change
   The following changes are **always breaking** — they require a new major version (/v2/) and a deprecation period:
   - Removing an endpoint
   - Removing a required request field
   - Removing a response field that consumers depend on
   - Changing a field's type (e.g., integer to string, string to enum)
   - Changing a field's constraint to be more restrictive (e.g., max length from 500 to 100 chars)
   - Changing an HTTP status code for a specific error condition
   - Changing the authentication mechanism or token format
   - Changing the error code taxonomy (renaming or removing error codes)

   The following changes are **non-breaking** — they may be released in the current version:
   - Adding a new optional request field (consumers that don't send it still work)
   - Adding a new response field (consumers that don't read it still work)
   - Adding a new endpoint
   - Relaxing a field constraint (e.g., max length from 100 to 500 chars)
   - Adding a new HTTP status code for a new error condition not previously handled

   #### Deprecation Timeline
   | Stage | Duration | Consumer Action |
   |---|---|---|
   | Deprecation announced | Day 0 | Deprecation-Date and Sunset headers added to deprecated endpoints; changelog published |
   | Old version supported | 90 days minimum | Consumers migrate to the new version |
   | Old version sunset | Day 90+ | Endpoint returns 410 Gone — migration is mandatory |

   #### Consumer Notification Mechanism
   Breaking changes are announced via:
   - HTTP response headers on the deprecated endpoint: `Deprecation: true`, `Sunset: Sat, 15 Jun 2024 00:00:00 GMT`
   - Changelog entry in this document (Section 6)
   - Direct notification to registered API consumers via email

---

6. CHANGELOG

   | Version | Date | Change Type | Description |
   |---|---|---|---|
   | v1.0.0 | [date] | Initial release | [Summary of what the v1 contract covers] |

   Rules for the changelog:
   - Every version must have at least one changelog entry.
   - Change type: Breaking / Non-breaking / Deprecation.
   - For the initial release (v1), list the capabilities introduced, not "created the contract."

---

BLOCKING RULES:
- BLOCKED if: any endpoint has no error response documented — every endpoint that can fail must document the HTTP status codes and domain error codes it returns; an undocumented failure mode is a contract violation waiting to happen.
- BLOCKED if: the versioning strategy is absent or described as "we follow REST best practices" without specifying the exact mechanism — consumers cannot integrate without knowing whether to change a URL path segment or a header.
- BLOCKED if: authentication is described as "use a token" or "Bearer token" without specifying the exact token type (Bearer JWT, API Key, OAuth2 with scope), the header name, and the token expiration and refresh mechanism.
- BLOCKED if: any field in a request schema has no type and no constraint — `name: string` with no max length and no rule is not a contract; it is an invitation for injection attacks and data integrity failures.
- BLOCKED if: the breaking change policy is absent — consumers cannot plan migrations or set internal SLAs without knowing when and how the API changes.

Write in the language of the input.
```

## Critério de sucesso

- [ ] A visão geral especifica a estratégia de versionamento exata (path ou header), o tipo de token de autenticação com nome do header e mecanismo de expiração, e o rate limit com janela de tempo e escopo do contador
- [ ] Cada endpoint no catálogo tem propósito descrito em linguagem do consumidor (o que o consumidor consegue fazer), flag de autenticação com escopo quando aplicável, e indicação de idempotência
- [ ] Cada campo nos schemas de request tem tipo específico, constraint em linguagem de negócio e valor de exemplo realista — nenhum campo com apenas `string` ou `number` genérico
- [ ] A taxonomia de erros cobre todos os códigos HTTP usados pela API, todos os códigos de erro de domínio com resolução para o consumidor, e o formato JSON padrão de resposta de erro
- [ ] A política de breaking changes define explicitamente o que é uma breaking change (lista afirmativa), o que não é, o prazo mínimo de deprecação e o mecanismo de notificação aos consumidores
- [ ] O changelog tem pelo menos uma entrada para a versão atual com tipo de mudança (Breaking / Non-breaking / Deprecation)

## Boas práticas verificadas

- [ ] **Governança:** a política de breaking changes é o mecanismo de governança da API — sem ela, o produtor pode mudar o contrato sem aviso e causar falhas em cascata nos consumidores; o cronograma de deprecação de 90 dias é o gate de aprovação para cada breaking change
- [ ] **Observabilidade:** o formato padrão de erro inclui `request_id` (correlation ID) e `timestamp` — essas informações são obrigatórias para rastrear falhas em sistemas distribuídos; consumidores que não recebem correlation IDs não conseguem abrir chamados úteis
- [ ] **Clean Architecture:** o contrato documenta separadamente o que o endpoint faz do ponto de vista do consumidor e como os erros são estruturados — a API é uma fronteira de bounded context; seu contrato deve ser legível por qualquer consumidor independentemente da implementação interna
- [ ] **SOLID:** a taxonomia de erros usa Open/Closed: novos tipos de erro (novas condições de domínio) são adicionados ao catálogo sem renomear ou remover códigos existentes; consumidores que não conhecem o novo código de erro continuam funcionando com o tratamento genérico de 4xx
- [ ] **Event-Driven:** para APIs orientadas a eventos, o catálogo de endpoints documenta tópicos, exchanges e filas com semântica de producer/consumer — a nomenclatura dos eventos segue o padrão de past tense (SessionCancelled, ConsultantActivated) conforme a convenção do sistema
