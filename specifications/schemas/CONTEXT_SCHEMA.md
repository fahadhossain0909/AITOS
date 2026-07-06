# Context Schema

**Document ID:** AOS-SPEC-003

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the canonical machine-readable structure for Context Objects within the AITOS AI Operating System (AOS).

A Context Object represents the authoritative information provided to an AI agent before task execution.

The schema standardizes:

- Context identity
- Classification
- Authority
- Versioning
- References
- Dependencies
- Lifecycle
- Validation

Every execution context SHALL conform to this specification.

---

# Objectives

The Context Schema enables:

- Deterministic context loading
- Multi-agent interoperability
- Repository-aware execution
- Governance compliance
- Version consistency
- Runtime validation
- Context synchronization

---

# Design Principles

Every Context Object SHALL be:

- Structured
- Minimal
- Relevant
- Versioned
- Traceable
- Immutable after publication
- Machine-readable

---

# Context Architecture

```
Context
│
├── Metadata
├── Identity
├── Classification
├── Authority
├── Scope
├── References
├── Dependencies
├── Content
├── Validation
├── Lifecycle
└── Audit
```

---

# Canonical JSON Example

```json
{
  "schemaVersion": "1.0.0",

  "contextId": "CTX-000123",

  "title": "Architecture Review Context",

  "classification": "Architecture",

  "authority": "Approved",

  "version": "1.3.0",

  "scope": "Repository",

  "references": [
    "ADR-0007",
    "AES-004",
    "RFC-0012"
  ],

  "dependencies": [],

  "content": {},

  "status": "ACTIVE"
}
```

---

# Required Fields

Every Context Object SHALL include:

- schemaVersion
- contextId
- title
- classification
- authority
- version
- scope
- status
- content

---

# Identity

Context identifiers SHALL follow:

```
CTX-000001
```

Identifiers are immutable.

---

# Classification

Supported classifications include:

- Architecture
- Governance
- Documentation
- Security
- Testing
- Research
- Deployment
- Runtime
- Memory
- Operations

Projects MAY extend classifications.

---

# Authority Levels

Supported values:

- Draft
- Proposed
- Reviewed
- Approved
- Deprecated
- Archived

Only Approved context is authoritative by default.

---

# Scope

Supported scopes:

- Task
- Session
- Project
- Repository
- Organization
- Global

Context SHALL NOT exceed its declared scope.

---

# References

Context MAY reference:

- Constitution
- AES Documents
- ADRs
- RFCs
- Governance Policies
- Registry Entries
- Memory Objects

References SHALL identify governed artifacts.

---

# Dependencies

Dependencies SHALL identify prerequisite context required before this context becomes valid.

Circular dependencies SHOULD be avoided.

---

# Content

The content section SHALL contain the actual contextual knowledge.

Rules:

- UTF-8
- Structured
- Implementation-neutral
- Free from executable code

---

# Validation

A valid Context Object MUST satisfy:

✓ Valid schema

✓ Required fields present

✓ Version defined

✓ Authority assigned

✓ Scope defined

✓ Status valid

✓ References valid

✓ Content present

---

# Lifecycle

Supported states:

- Draft
- Review
- Approved
- Active
- Deprecated
- Archived

Lifecycle transitions SHALL be governed.

---

# Versioning

Context SHALL follow Semantic Versioning.

Major changes may invalidate dependent contexts.

Minor revisions SHOULD remain compatible.

---

# Security

Context SHALL NOT contain:

- Secrets
- Credentials
- Authentication tokens
- Private keys

Restricted context SHALL be access-controlled.

---

# Audit Metadata

Recommended metadata:

- Created By
- Created At
- Updated By
- Updated At
- Review Reference
- Approval Reference

Audit metadata SHALL support traceability.

---

# Compatibility

Consumers SHOULD ignore unknown optional fields.

Breaking changes require a major version increment.

---

# Compliance Checklist

Before publication verify:

✓ Context ID assigned

✓ Classification valid

✓ Authority defined

✓ Version assigned

✓ References verified

✓ Validation passed

✓ Audit metadata recorded

---

# Cross References

- AOS-005 Agent Memory Model
- AOS-006 Agent Context Loading
- AOS-007 Agent Security Policy
- AOS-009 Agent Registry
- AOS-SPEC-002 Agent Manifest Schema

---

# Change Log

| Version | Date | Description |
|----------|------------|------------------------------|
| 1.0.0 | 2026-07-06 | Initial Context Schema |