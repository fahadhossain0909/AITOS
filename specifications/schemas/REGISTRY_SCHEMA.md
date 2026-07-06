# Registry Schema

**Document ID:** AOS-SPEC-005

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the canonical machine-readable schema for Agent Registry entries within the AITOS AI Operating System (AOS).

Every registered AI agent SHALL have exactly one Registry Entry conforming to this specification.

The Registry serves as the authoritative inventory and governance record for all AI agents operating within AITOS.

---

# Objectives

The Registry Schema enables:

- Agent discovery
- Identity management
- Capability verification
- Trust management
- Certification tracking
- Dependency resolution
- Lifecycle governance
- Runtime initialization
- Auditability

---

# Design Principles

Every Registry Entry SHALL be:

- Globally identifiable
- Version controlled
- Machine-readable
- Searchable
- Auditable
- Immutable in identity
- Governed

---

# Registry Architecture

```
Registry Entry
│
├── Metadata
├── Identity
├── Ownership
├── Version
├── Lifecycle
├── Capability Profile
├── Permission Profile
├── Trust Model
├── Certification
├── Dependencies
├── Runtime Metadata
├── Audit
└── Governance
```

---

# Canonical JSON Example

```json
{
  "schemaVersion": "1.0.0",

  "registryId": "REG-000001",

  "agent": {
    "agentId": "AOS-AGENT-0007",
    "name": "Architecture Agent",
    "category": "Architecture"
  },

  "version": {
    "current": "2.1.0"
  },

  "lifecycle": {
    "status": "ACTIVE"
  },

  "trust": {
    "level": "T3"
  },

  "certification": {
    "status": "CERTIFIED"
  },

  "dependencies": [],

  "status": "ACTIVE"
}
```

---

# Required Fields

Every Registry Entry SHALL include:

- schemaVersion
- registryId
- agent
- version
- lifecycle
- trust
- certification
- status

---

# Registry Identifier

Registry identifiers SHALL follow:

```
REG-000001
```

Registry identifiers are immutable.

---

# Agent Identity

Required:

- Agent ID
- Name
- Category

Agent ID SHALL match the Agent Manifest.

---

# Ownership

Required:

- Owner
- Maintainer

Optional:

- Reviewer
- Approver

Ownership SHALL reference organizational roles.

---

# Version Information

Required:

- Current Version

Optional:

- Supported Versions
- Runtime Compatibility

Semantic Versioning SHALL be used.

---

# Lifecycle

Supported values:

- Proposed
- Registered
- Validated
- Approved
- Active
- Suspended
- Deprecated
- Retired
- Archived

Lifecycle transitions SHALL comply with the Agent Lifecycle Standard.

---

# Capability Profile

The Registry SHALL reference:

- Capability Tier
- Capability Domains
- Responsibilities

Capability definitions SHALL remain synchronized with the Agent Manifest.

---

# Permission Profile

Permissions SHALL be explicitly declared.

Examples:

- READ_CONTEXT
- READ_MEMORY
- UPDATE_MEMORY
- CREATE_ADR
- REVIEW_CODE
- REGISTER_AGENT

Implicit permissions are prohibited.

---

# Trust Model

Supported trust levels:

- T0 — Untrusted
- T1 — Limited
- T2 — Managed
- T3 — Trusted
- T4 — Enterprise

Trust levels SHALL be assigned through formal evaluation.

---

# Certification Metadata

Required:

- Certification Status

Optional:

- Certification Level
- Evaluation Reference
- Evaluation Date
- Expiration Date

Supported status values:

- Pending
- In Evaluation
- Certified
- Conditionally Certified
- Suspended
- Revoked

---

# Dependency Graph

Dependencies MAY reference:

- Other Agents
- Context Objects
- Memory Objects
- ADRs
- RFCs
- External Services
- Runtime Components

Circular dependencies SHOULD be detected during validation.

---

# Runtime Metadata

Optional runtime information:

```json
{
  "runtime": {
    "executionMode": "Coordinator",
    "supportsStreaming": true,
    "minimumRuntimeVersion": "1.5.0"
  }
}
```

Runtime metadata SHALL remain implementation-neutral.

---

# Validation Rules

A valid Registry Entry MUST satisfy:

✓ Valid schema

✓ Registry ID assigned

✓ Agent exists

✓ Manifest exists

✓ Lifecycle defined

✓ Trust level assigned

✓ Certification status defined

✓ Dependencies validated

✓ Status valid

---

# Security

Registry entries SHALL NOT contain:

- Secrets
- Passwords
- API keys
- Authentication tokens
- Private keys

Sensitive operational data SHALL be stored separately.

---

# Audit Metadata

Recommended fields:

- Created By
- Created At
- Updated By
- Updated At
- Change Summary
- Review Reference
- Approval Reference

Audit records SHALL be immutable.

---

# Compatibility

Consumers SHOULD ignore unknown optional fields.

Breaking structural changes require a major schema version increment.

---

# Compliance Checklist

Before publication verify:

✓ Registry ID assigned

✓ Agent registered

✓ Manifest verified

✓ Lifecycle valid

✓ Trust assigned

✓ Certification recorded

✓ Dependencies validated

✓ Audit metadata recorded

---

# Cross References

- AOS-009 Agent Registry
- AOS-003 Agent Capability Model
- AOS-007 Agent Security Policy
- AOS-008 Agent Evaluation
- AOS-SPEC-002 Agent Manifest Schema
- AOS-SPEC-003 Context Schema
- AOS-SPEC-004 Memory Schema

---

# Change Log

| Version | Date | Description |
|----------|------------|----------------------------------|
| 1.0.0 | 2026-07-06 | Initial Registry Schema |