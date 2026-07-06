# Versioning Standard

**Document ID:** AOS-SPEC-010

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the official versioning model for the AITOS AI Operating System (AOS).

It standardizes version management across:

- Runtime
- APIs
- Protocols
- Schemas
- Agents
- Registry
- Memory
- Context
- Governance Documents
- SDKs
- CLI
- Plugins
- Extensions

Every versioned artifact SHALL comply with this specification.

---

# Objectives

The Versioning Standard enables:

- Predictable releases
- Backward compatibility
- Controlled evolution
- Safe upgrades
- Automated dependency management
- Long-term maintenance
- Enterprise governance

---

# Scope

This specification applies to:

- APIs
- JSON Schemas
- YAML Specifications
- Protocols
- Runtime Components
- AI Agents
- Registry Entries
- Context Objects
- Memory Objects
- SDK Packages
- CLI Releases
- Documentation

---

# Design Principles

Versioning SHALL be:

- Predictable
- Transparent
- Machine-readable
- Backward-compatible whenever practical
- Auditable
- Governed

Breaking changes SHALL NEVER occur silently.

---

# Canonical Version Format

AITOS adopts Semantic Versioning 2.0.0.

```
MAJOR.MINOR.PATCH
```

Example:

```
1.0.0
1.3.5
2.0.0
```

---

# Semantic Versioning Rules

## MAJOR

Increment when:

- Breaking API changes
- Breaking schema changes
- Protocol incompatibility
- Runtime behavior changes
- Removed features

Example

```
1.8.4

↓

2.0.0
```

---

## MINOR

Increment when:

- New features
- New endpoints
- New capabilities
- Backward-compatible enhancements

Example

```
1.2.0

↓

1.3.0
```

---

## PATCH

Increment when:

- Bug fixes
- Documentation corrections
- Security fixes
- Performance improvements

Example

```
1.3.2

↓

1.3.3
```

---

# Pre-release Versions

Supported identifiers:

```
-alpha

-beta

-rc
```

Examples

```
2.0.0-alpha.1

2.0.0-beta.2

2.0.0-rc.1
```

Production releases SHALL NOT contain pre-release identifiers.

---

# Build Metadata

Optional build metadata:

```
+build.125

+20260706

+sha.a4d91f2
```

Example

```
2.1.0+build.74
```

Build metadata SHALL NOT affect compatibility.

---

# Schema Evolution

Every schema SHALL declare:

- Schema Version
- Compatibility Level
- Previous Version
- Successor Version

Example

```yaml
schemaVersion: 2.1.0

compatibleWith:

- 2.0.x
- 2.1.x
```

---

# Compatibility Levels

Supported levels:

## Full Compatibility

Consumers require no changes.

---

## Backward Compatible

Older clients continue functioning.

---

## Forward Compatible

Newer clients degrade gracefully.

---

## Breaking

Migration required.

---

# API Compatibility

Rules:

PATCH releases:

✓ Fully compatible

MINOR releases:

✓ Backward compatible

MAJOR releases:

✗ Breaking changes permitted

Deprecated endpoints SHALL remain available during the transition period.

---

# Protocol Negotiation

Protocols SHALL expose supported versions.

Example

```
Accept-Version: 2.1
```

or

```
X-AITOS-Version: 2.1
```

When multiple versions are supported:

1. Highest mutually supported version
2. Otherwise fail with compatibility error

---

# Migration Strategy

Every breaking release SHALL provide:

- Migration Guide
- Upgrade Checklist
- Compatibility Matrix
- Deprecated Features
- Rollback Procedure

Migration documentation SHALL be published before release.

---

# Compatibility Matrix

| Consumer | Provider | Supported |
|----------|----------|-----------|
| v1 | v1 | ✓ |
| v1 | v2 Minor | ✓ |
| v1 | v2 Major | ✗ |
| v2 | v2 | ✓ |

Compatibility SHALL be explicitly documented.

---

# Deprecation Policy

Every deprecated artifact SHALL include:

- Deprecation Date
- Sunset Date
- Replacement
- Migration Guide
- Compatibility Notes

Example

```
Deprecated:
2027-01-01

Removal:
2028-01-01
```

Deprecation SHALL be announced before removal.

---

# Long-term Support (LTS)

AITOS defines two release channels:

## Standard Release

Support Duration

12 months

Features

- New capabilities
- Frequent updates

---

## Long-term Support (LTS)

Support Duration

36 months

Characteristics

- Stability prioritized
- Critical fixes only
- Security updates
- Backward compatibility

LTS releases SHOULD be designated every second major release unless governance approves otherwise.

---

# Runtime Version Policy

Runtime SHALL expose:

```
Runtime Version

Supported API Versions

Supported Schema Versions

Supported Protocol Versions
```

Version discovery SHALL be automated.

---

# Agent Versioning

Every Agent SHALL declare:

- Agent Version
- Manifest Version
- Capability Version
- Registry Version

Agent upgrades SHALL preserve registry identity.

---

# Context Versioning

Context Objects SHALL use Semantic Versioning.

Minor revisions SHOULD preserve semantic meaning.

Major revisions MAY invalidate dependent contexts.

---

# Memory Versioning

Memory SHALL maintain:

- Current Version
- Previous Version
- Change Summary

Historical revisions SHALL remain auditable.

---

# Registry Versioning

Registry Entries SHALL record:

- Registration Version
- Current Version
- Certification Version

Identity SHALL remain immutable across versions.

---

# Audit Requirements

Every version change SHALL record:

- Previous Version
- New Version
- Author
- Timestamp
- Change Reason
- Approval Reference

Audit history SHALL be immutable.

---

# Validation Rules

Every versioned artifact MUST satisfy:

✓ Valid Semantic Version

✓ Compatibility declared

✓ Lifecycle defined

✓ Migration documented (if breaking)

✓ Audit recorded

---

# Compliance Checklist

Before release verify:

✓ Semantic Version assigned

✓ Compatibility reviewed

✓ Migration guide completed

✓ Deprecation policy followed

✓ LTS impact assessed

✓ Audit metadata recorded

---

# Cross References

- API_SPECIFICATION.md
- ERROR_CODES.md
- EVENT_MODEL.md
- AGENT_REGISTRY.md
- AGENT_LIFECYCLE.md
- RELEASE_GOVERNANCE.md
- CHANGE_MANAGEMENT.md

---

# Appendix A — Release Lifecycle

```
Planning
    │
    ▼
Development