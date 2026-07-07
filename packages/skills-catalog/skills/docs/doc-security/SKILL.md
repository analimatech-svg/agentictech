# Skill: Segurança

## Metadados

| Campo | Valor |
|---|---|
| Fase SDLC | Architecture |
| Categoria | docs |
| Nível mínimo Maestro | Praticante |
| Provedor LLM | Qualquer (Claude, GPT-4o, Gemini) |
| Versão | 1.1.0 |

## Objetivo

Gerar o documento de segurança do sistema, cobrindo modelo de ameaças, superfície de ataque, controles implementados, classificação de dados sensíveis e política de acesso, com referência ao OWASP Top 10 e ao princípio do menor privilégio.

## Input

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| arquitetura_tecnica | string (Markdown) | Sim | Documento de arquitetura com componentes, fluxo de dados e dependências externas |
| dados_tratados | lista de strings | Sim | Lista dos tipos de dados que o sistema processa, armazena ou transmite |
| fluxo_autenticacao | string | Sim | Como usuários e serviços se autenticam no sistema (JWT, OAuth 2.0, API Key, etc.) |
| fluxo_autorizacao | string | Sim | Como permissões são concedidas e verificadas (RBAC, ABAC, ACL, etc.) |
| compliance | lista de strings | Não | Regulamentações aplicáveis (LGPD, GDPR, PCI-DSS, SOC 2, ISO 27001) |

## Output

- **Formato:** Markdown
- **Arquivo sugerido:** `docs/security.md`
- **Campos obrigatórios:** classificação de dados sensíveis, modelo de ameaças com avaliação OWASP, superfície de ataque, controles implementados por ameaça, fluxo de autenticação e autorização, política de acesso

## Prompt base

```
You are a specialized AI assistant for software documentation.
Generate a security document based on the system architecture and data context.

Structure:
1. DATA CLASSIFICATION: For each data type, assign classification level:
   - Public: no restrictions
   - Internal: for team use only
   - Confidential: restricted access, requires authorization
   - Secret: highest restriction, minimal access, encrypted at rest and in transit

2. THREAT MODEL (OWASP Top 10 evaluation):
   - Evaluate at least 3 relevant threats from OWASP Top 10 for this system context
   - For each: threat description, likelihood (Low/Medium/High), impact (Low/Medium/High), implemented control

3. ATTACK SURFACE: List all entry points (APIs, UIs, webhooks, file uploads, admin panels) with exposure level

4. AUTHENTICATION AND AUTHORIZATION FLOW: Step-by-step description of how a request is authenticated and authorized

5. ACCESS POLICY: Role matrix (who can do what) following least privilege principle

6. SECURITY CONTROLS: Implemented controls per layer (input validation, encryption, rate limiting, audit logging)

Reference: OWASP Top 10, LGPD/GDPR when applicable, principle of least privilege.

Language: Brazilian Portuguese
```

## Critério de sucesso

- [ ] Cada tipo de dado sensível tem classificação definida (público, interno, confidencial ou secreto)
- [ ] O fluxo de autenticação está documentado passo a passo, sem ambiguidade
- [ ] Pelo menos três ameaças relevantes do OWASP Top 10 foram avaliadas com controle implementado
- [ ] A política de acesso segue o princípio do menor privilégio: cada papel tem apenas as permissões necessárias
- [ ] A superfície de ataque está mapeada com todos os pontos de entrada do sistema identificados

## Boas práticas verificadas

- [ ] OWASP Top 10: ameaças avaliadas são as mais relevantes para o contexto (não genéricas)
- [ ] Princípio do menor privilégio: nenhum papel tem permissões além do necessário para sua função
- [ ] Dados sensíveis têm controles técnicos especificados: criptografia em repouso e em trânsito
- [ ] Auditoria e rastreabilidade: ações críticas geram logs com identidade, timestamp e resultado
- [ ] Compliance identificado: se LGPD ou GDPR se aplica, base legal de tratamento está documentada
