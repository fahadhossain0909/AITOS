# Memory Schema

**Document ID:** AOS-SPEC-004

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the canonical machine-readable schema for Memory Objects within the AITOS AI Operating System (AOS).

Memory Objects are the authoritative representation of persistent engineering knowledge shared between AI agents, humans, and automation systems.

This schema establishes a consistent structure for creating, storing, retrieving, validating, synchronizing, and governing engineering memory.

---

# Objectives

The Memory Schema enables:

- Persistent institutional knowledge
- Cross-agent memory sharing
- Version-controlled engineering memory
- Repository-aware retrieval
- Governance compliance
- Auditability
- Runtime interoperability

---

# Design Principles

Every Memory Object SHALL be:

- Persistent
- Traceable
- Versioned
- Immutable after publication
- Governed
- Machine-readable
- Searchable

Memory SHALL represent validated knowledge rather than temporary conversation history.

---

# Memory Architecture

```
Memory Object
│
├── Metadata
├── Identity
├── Classification
├── Source
├── Authority
├── Scope
├── Relationships
├── Content
├── Lifecycle
├── Validation
└── Audit
```

---

# Canonical JSON Example

```json
{
  "schemaVersion": "1.0.0",

  "memoryId": "MEM-000123",

  "title": "Architecture Decision Knowledge",

  "classification": "Architecture",

  "authority": "Approved",

  "version": "2.1.0",

  "scope": "Repository",

  "source": {
    "type": "ADR",
    "reference": "ADR-0007"
  },

  "relationships": [
    "CTX-000041",
    "RFC-0012"
  ],

  "content": {},

  "status": "ACTIVE"
}
```

---

# Required Fields

Every Memory Object SHALL include:

- schemaVersion
- memoryId
- title
- classification
- authority
- version
- source
- scope
- content
- status

---

# Identity

Memory identifiers SHALL follow:

```
MEM-000001
```

Identifiers are globally unique and immutable.

---

# Memory Classification

Supported classifications:

- Architecture
- Governance
- Engineering Standard
- Coding Pattern
- Security Practice
- Testing Pattern
- Deployment Practice
- Research
- Lesson Learned
- Anti-pattern
- Operational Procedure

Organizations MAY extend this list.

---

# Authority

Authority values:

- Draft
- Proposed
- Reviewed
- Approved
- Deprecated
- Archived

Only Approved memory SHALL be treated as institutional knowledge.

---

# Scope

Supported scopes:

- Session
- Project
- Repository
- Organization
- Global

Memory SHALL NOT be reused outside its declared scope without review.

---

# Source

Every Memory Object SHALL declare its origin.

Supported source types:

- ADR
- RFC
- AES
- Governance
- Research
- Human Review
- Incident Report
- Release

Source references SHALL be traceable.

---

# Relationships

Memory MAY reference:

- Context Objects
- ADRs
- RFCs
- Other Memory Objects
- Registry Entries
- Governance Policies

Relationships SHALL preserve knowledge traceability.

---

# Content

The content section SHALL contain validated engineering knowledge.

Content SHOULD be:

- Structured
- Technology-neutral
- Searchable
- Human-readable
- Machine-readable

Executable code SHALL NOT be embedded directly within memory records.

---

# Lifecycle

Supported states:

- Draft
- Review
- Approved
- Active
- Deprecated
- Archived

Lifecycle transitions SHALL be governed and auditable.

---

# Validation Rules

A valid Memory Object MUST satisfy:

✓ Schema valid

✓ Memory ID assigned

✓ Classification defined

✓ Authority assigned

✓ Source declared

✓ Scope defined

✓ Content present

✓ Status valid

---

# Versioning

Memory SHALL use Semantic Versioning.

Major revisions MAY invalidate dependent references.

Minor revisions SHOULD preserve compatibility.

---

# Synchronization

Memory synchronization SHALL preserve:

- Identity
- Version
- Provenance
- Relationships
- Audit history

Conflicting updates SHALL require review before publication.

---

# Security

Memory SHALL NOT contain:

- Secrets
- Credentials
- Private keys
- Authentication tokens

Sensitive knowledge SHALL be protected using repository access controls.

---

# Audit Metadata

Recommended metadata:

- Created By
- Created At
- Updated By
- Updated At
- Approved By
- Approval Reference
- Change Summary

Audit information SHALL remain immutable.

---

# Compatibility

Consumers SHOULD ignore unknown optional fields.

Breaking structural changes require a major schema version increment.

---

# Compliance Checklist

Before publication verify:

✓ Memory ID assigned

✓ Source declared

✓ Classification valid

✓ Version assigned

✓ Validation completed

✓ Audit metadata recorded

✓ Governance approval completed

---

# Cross References

- AOS-005 Agent Memory Model
- AOS-006 Agent Context Loading
- AOS-007 Agent Security Policy
- AOS-009 Agent Registry
- AOS-SPEC-003 Context Schema

---

# Change Log

| Version | Date | Description |
|----------|------------|--------------------------------|
| 1.0.0 | 2026-07-06 | Initial Memory Schema |