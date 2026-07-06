# AITOS Standard Error Codes

**Document ID:** AOS-SPEC-008

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the canonical error taxonomy, namespace, severity model, recovery strategy, retry behavior, audit requirements, and machine-readable error contract for the AITOS AI Operating System (AOS).

Every runtime component SHALL emit standardized errors defined by this specification.

---

# Objectives

The Error Model enables:

- Consistent error reporting
- Automated recovery
- Cross-agent interoperability
- Machine-readable diagnostics
- Governance compliance
- Security monitoring
- Auditability
- Observability

---

# Design Principles

Errors SHALL be:

- Deterministic
- Machine-readable
- Versioned
- Traceable
- Actionable
- Auditable

Errors describe failures.

They SHALL NOT contain implementation-specific debugging information.

---

# Error Namespace

```
AOS

├── AUTH
├── PERMISSION
├── CONTEXT
├── MEMORY
├── REGISTRY
├── AGENT
├── WORKFLOW
├── EVENT
├── API
├── VALIDATION
├── SECURITY
├── RUNTIME
├── STORAGE
├── NETWORK
├── GOVERNANCE
└── INTERNAL
```

Each namespace SHALL own its numeric range.

---

# Error Identifier Format

```
AOS-{NAMESPACE}-{CODE}

Examples

AOS-AUTH-1001

AOS-CONTEXT-2003

AOS-SECURITY-5002

AOS-RUNTIME-7001
```

Error identifiers SHALL be immutable.

---

# Standard Error Object

```json
{
  "schemaVersion":"1.0.0",

  "errorId":"AOS-CONTEXT-2001",

  "category":"Context",

  "severity":"ERROR",

  "message":"Required context not found.",

  "recoverable":true,

  "retryable":true,

  "timestamp":"2026-07-06T12:00:00Z",

  "correlationId":"TASK-0007",

  "details":{},

  "status":"ACTIVE"
}
```

---

# Required Fields

Every Error Object SHALL include:

- schemaVersion
- errorId
- category
- severity
- message
- recoverable
- retryable
- timestamp
- status

---

# Severity Levels

Supported values:

## INFO

Operational information.

No action required.

---

## WARNING

Execution continues.

Operator review recommended.

---

## ERROR

Operation failed.

Recovery required.

---

## CRITICAL

System integrity at risk.

Immediate intervention required.

---

## FATAL

Execution SHALL terminate.

Recovery requires manual action.

---

# Error Categories

Supported categories include:

- Authentication
- Authorization
- Context
- Memory
- Registry
- Workflow
- Agent
- Runtime
- Security
- Validation
- API
- Event
- Network
- Storage
- Governance
- Internal

---

# Standard Error Ranges

| Range | Namespace |
|--------|-----------|
|1000–1999|Authentication|
|2000–2999|Context|
|3000–3999|Memory|
|4000–4999|Registry|
|5000–5999|Security|
|6000–6999|Validation|
|7000–7999|Runtime|
|8000–8999|Workflow|
|9000–9999|Internal|

---

# Canonical Error Catalog

## Authentication

| Code | Description |
|------|-------------|
|1001|Authentication Failed|
|1002|Invalid Identity|
|1003|Expired Credential|

---

## Context

| Code | Description |
|------|-------------|
|2001|Context Not Found|
|2002|Context Validation Failed|
|2003|Context Version Mismatch|
|2004|Context Dependency Missing|

---

## Memory

| Code | Description |
|------|-------------|
|3001|Memory Not Found|
|3002|Memory Conflict|
|3003|Memory Validation Failed|
|3004|Memory Synchronization Failed|

---

## Registry

| Code | Description |
|------|-------------|
|4001|Agent Not Registered|
|4002|Registry Entry Missing|
|4003|Duplicate Agent Identifier|

---

## Security

| Code | Description |
|------|-------------|
|5001|Permission Denied|
|5002|Prompt Injection Detected|
|5003|Secret Access Blocked|
|5004|Trust Validation Failed|

---

## Validation

| Code | Description |
|------|-------------|
|6001|Schema Validation Failed|
|6002|Required Field Missing|
|6003|Unsupported Version|

---

## Runtime

| Code | Description |
|------|-------------|
|7001|Runtime Initialization Failed|
|7002|Execution Timeout|
|7003|Resource Exhausted|

---

## Workflow

| Code | Description |
|------|-------------|
|8001|Workflow Failed|
|8002|Task Dependency Failed|
|8003|Workflow Cancelled|

---

## Internal

| Code | Description |
|------|-------------|
|9001|Unexpected Internal Error|
|9002|Unknown Exception|

---

# Recovery Classification

Every error SHALL declare one of:

- Recover Automatically
- Retry
- Manual Intervention
- Permanent Failure

---

# Retry Policy

Retryable errors SHALL define:

- Maximum retries
- Backoff strategy
- Retry interval
- Retry timeout

Recommended strategies:

- Exponential Backoff
- Linear Backoff
- Immediate Retry
- No Retry

Retry behavior SHALL be deterministic.

---

# Machine-readable Recovery Contract

```json
{
  "recovery":{

    "strategy":"EXPONENTIAL_BACKOFF",

    "maxRetries":5,

    "initialDelayMs":1000,

    "maximumDelayMs":60000
  }
}
```

---

# Validation Rules

Every Error Object MUST satisfy:

✓ Valid schema

✓ Valid namespace

✓ Valid code

✓ Supported severity

✓ Timestamp present

✓ Recovery strategy declared

✓ Audit enabled

---

# Audit Requirements

Every error SHALL record:

- Error ID
- Timestamp
- Source Component
- Agent ID
- Workflow ID
- Task ID
- Correlation ID
- Recovery Action
- Final Resolution

Audit records SHALL be immutable.

---

# Security Requirements

Error messages SHALL NOT expose:

- Secrets
- Tokens
- Passwords
- Internal stack traces