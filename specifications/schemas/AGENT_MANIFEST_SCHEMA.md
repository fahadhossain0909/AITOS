# Agent Manifest Schema

**Document ID:** AOS-SPEC-002

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the canonical manifest format for every AI agent registered within the AITOS AI Operating System (AOS).

An Agent Manifest is the machine-readable identity document of an AI agent. It describes what the agent is, what it is permitted to do, what it depends on, and how it participates in governed engineering workflows.

Every registered agent SHALL provide exactly one valid manifest.

---

# Objectives

The manifest enables:

- Standardized registration
- Capability discovery
- Permission enforcement
- Dependency management
- Runtime initialization
- Security validation
- Evaluation integration
- Registry synchronization

---

# Design Principles

Every manifest SHALL be:

- Self-describing
- Machine-readable
- Version controlled
- Deterministic
- Extensible
- Auditable

---

# Manifest Structure

```
Manifest
│
├── Metadata
├── Identity
├── Ownership
├── Lifecycle
├── Capability
├── Permissions
├── Dependencies
├── Context
├── Memory
├── Security
├── Evaluation
├── Runtime
└── Audit
```

---

# Canonical YAML Example

```yaml
manifestVersion: "1.0.0"

agent:
  id: "AOS-AGENT-0007"
  name: "Architecture Agent"
  category: "Architecture"

version:
  current: "2.1.0"

ownership:
  owner: "Architecture Lead"
  maintainer: "AITOS Engineering"

lifecycle:
  status: "ACTIVE"

capabilities:
  tier: 4
  domains:
    - Architecture
    - Documentation

permissions:
  - READ_CONTEXT
  - READ_MEMORY
  - CREATE_ADR
  - REVIEW_ARCHITECTURE

security:
  trustLevel: "T3"

evaluation:
  certification: "Certified"
```

---

# Required Sections

## Metadata

Required fields:

- manifestVersion
- schemaVersion
- generatedAt

---

## Identity

Required:

- Agent ID
- Name
- Category
- Description

Agent IDs MUST be globally unique.

---

## Ownership

Required:

- Owner
- Maintainer

Optional:

- Reviewer
- Approver

Ownership SHALL reference organizational roles.

---

## Version Information

Required:

- Current Version

Optional:

- Compatibility Version
- Minimum Runtime Version

Semantic Versioning SHALL be used.

---

## Lifecycle

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

---

## Capability Profile

Required:

- Tier
- Domains
- Responsibilities

Capabilities SHALL reference the Agent Capability Model.

---

## Permission Profile

Permissions SHALL be explicit.

Examples:

- READ_CONTEXT
- WRITE_DOCUMENTATION
- GENERATE_CODE
- UPDATE_MEMORY
- CREATE_ADR
- REGISTER_AGENT

Undefined permissions SHALL NOT be assumed.

---

## Dependencies

Manifest SHALL declare:

- Required Context
- Required Memory
- Supporting Agents
- External Services
- Runtime Components

Dependency cycles SHOULD be avoided.

---

## Context Requirements

Context declaration may include:

```yaml
context:
  required:
    - constitution
    - governance
    - architecture
```

---

## Memory Requirements

```yaml
memory:

  read:

    - institutional

    - project

  write:

    - session
```

Memory permissions SHALL align with governance.

---

## Security Profile

Required:

- Trust Level
- Authentication Mode
- Authorization Scope

Optional:

- Allowed Environments
- Restricted Operations

---

## Evaluation Profile

Required:

- Certification Status

Optional:

- Benchmark Score
- Evaluation Date
- Review Reference

---

## Runtime Configuration

Optional runtime metadata:

```yaml
runtime:

  executionMode: coordinator

  supportsStreaming: true

  timeout: 120
```

Runtime configuration SHALL remain implementation-neutral.

---

## Audit Metadata

Recommended fields:

- Created
- Updated
- Owner
- Change History

Audit metadata SHALL support traceability.

---

# Validation Rules

A valid manifest MUST satisfy:

✓ Valid YAML

✓ Valid schema version

✓ Registered Agent ID

✓ Version present

✓ Lifecycle defined

✓ Capability profile complete

✓ Permission profile complete

✓ Security profile complete

✓ Evaluation profile present

---

# Compatibility Rules

Minor Version

Backward compatible.

Major Version

Migration required.

Manifest readers SHOULD ignore unknown optional fields.

---

# Security Requirements

The manifest SHALL NOT contain:

- Secrets
- Passwords
- Tokens
- Private Keys

Sensitive operational data SHALL remain external.

---

# Example Minimal Manifest

```yaml
manifestVersion: "1.0.0"

agent:

  id: "AOS-AGENT-0001"

  name: "Documentation Agent"

version:

  current: "1.0.0"

lifecycle:

  status: "ACTIVE"
```

---

# Compliance Checklist

✓ Identity defined

✓ Ownership assigned

✓ Version valid

✓ Capabilities declared

✓ Permissions declared

✓ Lifecycle valid

✓ Security profile defined

✓ Evaluation profile present

---

# Cross References

- AOS-003 Agent Capability Model
- AOS-004 Agent Communication Protocol
- AOS-005 Agent Memory Model
- AOS-006 Agent Context Loading
- AOS-007 Agent Security Policy
- AOS-008 Agent Evaluation
- AOS-009 Agent Registry

---

# Change Log

| Version | Date | Description |
|----------|------------|-----------------------------|
| 1.0.0 | 2026-07-06 | Initial Agent Manifest Schema |