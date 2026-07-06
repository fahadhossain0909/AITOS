# API Specification

**Document ID:** AOS-SPEC-009

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the canonical REST API architecture for the AITOS AI Operating System (AOS).

It standardizes:

- Resource Model
- URI Design
- HTTP Methods
- Authentication
- Authorization
- Versioning
- Idempotency
- Pagination
- Filtering
- Error Handling
- Rate Limiting
- OpenAPI Compatibility

Every externally exposed AITOS service SHALL comply with this specification.

---

# Objectives

The API Specification enables:

- Consistent service interfaces
- Secure interoperability
- SDK generation
- OpenAPI compatibility
- Stable integrations
- Automated validation
- Governance compliance

---

# Architectural Principles

Every API SHALL be:

- RESTful
- Stateless
- Versioned
- Secure
- Observable
- Idempotent where applicable
- Backward compatible

---

# Base URL

```
https://api.aitos.io/v1/
```

Internal deployments MAY use private endpoints.

---

# Resource Model

Primary resources:

```
/agents
/registry
/context
/memory
/events
/workflows
/tasks
/evaluations
/governance
/rfcs
/adrs
/releases
/users
/health
```

Resources SHALL be nouns.

---

# Standard HTTP Methods

| Method | Purpose |
|---------|----------|
| GET | Read |
| POST | Create |
| PUT | Replace |
| PATCH | Partial Update |
| DELETE | Delete |
| HEAD | Metadata |
| OPTIONS | Discovery |

---

# Standard HTTP Status Codes

| Code | Meaning |
|------|----------|
|200|OK|
|201|Created|
|202|Accepted|
|204|No Content|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|409|Conflict|
|422|Validation Failed|
|429|Too Many Requests|
|500|Internal Error|
|503|Unavailable|

---

# Resource Naming

Good:

```
GET /agents

GET /agents/{id}

GET /registry

GET /memory/{id}
```

Avoid:

```
/getAgent

/createMemory

/deleteRegistry
```

Verbs SHALL NOT appear in resource names.

---

# API Versioning

URI versioning SHALL be used.

```
/v1/

/v2/
```

Major versions MAY introduce breaking changes.

Minor versions SHALL remain backward compatible.

---

# Authentication

Supported mechanisms:

- OAuth 2.1
- OpenID Connect
- JWT
- Mutual TLS (optional)
- API Keys (internal only)

Anonymous access SHOULD be disabled unless explicitly approved.

---

# Authorization

Authorization SHALL use Role-Based Access Control (RBAC).

Example roles:

- Administrator
- Maintainer
- Reviewer
- Agent
- ReadOnly

Fine-grained permissions SHOULD be supported.

---

# Request Headers

Required:

```
Authorization: Bearer <token>

Accept: application/json

Content-Type: application/json
```

Recommended:

```
X-Correlation-ID

Idempotency-Key

X-Request-ID
```

---

# Response Format

Success:

```json
{
  "success": true,

  "data": {}
}
```

Failure:

```json
{
  "success": false,

  "error": {}
}
```

Response envelopes SHALL remain consistent.

---

# Pagination

Supported parameters:

```
?page=1

&pageSize=50
```

Response:

```json
{
  "page":1,

  "pageSize":50,

  "totalItems":200,

  "totalPages":4,

  "items":[]
}
```

Cursor pagination MAY be supported for large datasets.

---

# Filtering

Examples:

```
GET /agents?status=ACTIVE

GET /memory?classification=Architecture

GET /registry?trust=T3
```

Filtering SHALL be deterministic.

---

# Sorting

Example:

```
?sort=name

?sort=-createdAt
```

Negative prefix indicates descending order.

---

# Field Selection

Example:

```
GET /agents?fields=id,name,status
```

Consumers MAY request partial resources.

---

# Search

Example:

```
GET /memory?search=context
```

Full-text search SHOULD support relevance ranking.

---

# Idempotency

POST requests creating resources SHOULD support:

```
Idempotency-Key
```

Duplicate requests SHALL NOT create duplicate resources.

---

# Rate Limiting

Recommended response headers:

```
X-RateLimit-Limit

X-RateLimit-Remaining

X-RateLimit-Reset
```

Suggested defaults:

- Anonymous: Disabled
- Authenticated: 1000 requests/hour
- Internal Services: Configurable

---

# Timeout

Recommended maximum request duration:

30 seconds

Long-running operations SHOULD return:

```
202 Accepted
```

and provide an operation status endpoint.

---

# Long-running Operations

Example:

```
POST /evaluations

↓

202 Accepted

↓

GET /operations/{id}
```

---

# Error Handling

Errors SHALL comply with:

- ERROR_CODES.md
- RFC 9457 (Problem Details)

Example:

```json
{
  "type":"validation",

  "title":"Schema Validation Failed",

  "status":422,

  "detail":"Required field missing."
}
```

---

# OpenAPI Compatibility

Every public API SHALL provide:

```
openapi.yaml
```

Minimum supported version:

```
OpenAPI 3.1
```

SDK generation SHOULD be automated.

---

# Security Requirements

Every API SHALL support:

- TLS 1.3
- Input validation
- Output encoding
- Secret isolation
- Audit logging
- Request tracing

Sensitive endpoints SHALL require elevated authorization.

---

# Observability

Every request SHOULD produce:

- Request ID
- Correlation ID
- Metrics
- Logs
- Distributed Trace

OpenTelemetry compatibility is RECOMMENDED.

---

# API Lifecycle

States:

```
Experimental

↓

Beta

↓

Stable

↓

Deprecated

↓

Retired
```

Deprecated endpoints SHOULD remain available for at least one major release.

---

# Compliance Checklist

✓ REST compliant

✓ Versioned

✓ Authenticated

✓ Authorized

✓ Paginated

✓ Filterable

✓ Rate limited

✓ Idempotent

✓ Observable

✓ OpenAPI compliant

---

# Cross References

- EVENT_MODEL.md
- ERROR_CODES.md
- VERSIONING.md
- ACP_JSON_SCHEMA.md
- REGISTRY_SCHEMA.md
- MEMORY_SCHEMA.md

---

# Change Log

| Version | Date | Description |
|----------|------------|--------------------------------|
|1.0.0|2026-07-06|Initial REST API Specification|