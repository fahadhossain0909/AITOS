# Version Policy

**Document ID:** GOV-008

**Version:** 1.0.0

**Status:** Active

**Owner:** Engineering Lead

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This document defines the official version governance policy for AITOS.

Versioning provides:

- Traceability
- Compatibility
- Change visibility
- Controlled evolution
- AI context stability
- Long-term maintainability

Every governed engineering artifact shall follow a documented versioning strategy.

---

# Scope

This policy applies to:

- Repository
- Constitution
- Engineering Standards (AES)
- Governance
- Architecture
- Requirements
- Research
- AI Agents
- Prompt Library
- Context Layer
- Memory Layer
- Documentation
- Software Releases

---

# Guiding Principles

Version numbers communicate engineering meaning.

A version should indicate:

- Compatibility
- Stability
- Scope of change
- Migration expectations

Version history must never be rewritten.

---

# Version Hierarchy

AITOS maintains independent but coordinated version streams.

```
Repository
│
├── Constitution
├── Standards
├── Governance
├── Architecture
├── Requirements
├── Research
├── Context
├── Memory
├── AI Agents
├── Prompt Library
├── Documentation
└── Software
```

Each layer evolves independently.

---

# Semantic Versioning

Format

MAJOR.MINOR.PATCH

Example

1.0.0

---

## MAJOR

Increment when:

- Breaking changes
- Repository restructuring
- Governance redesign
- Constitution revision

---

## MINOR

Increment when:

- New capability
- New engineering standard
- New framework
- Additional documentation

---

## PATCH

Increment when:

- Corrections
- Clarifications
- Editorial improvements
- Non-breaking fixes

---

# Repository Version

Represents the complete engineering platform.

Example

AITOS Repository

v2.3.1

Repository version changes only after an approved release.

---

# Constitution Version

Represents governance principles.

MAJOR

Changes to immutable principles.

MINOR

New constitutional articles.

PATCH

Editorial improvements.

---

# Engineering Standards Version

AES documents evolve independently.

Example

AES-004

v1.2.0

New standards should not force repository version changes.

---

# Governance Version

Governance documents follow independent semantic versioning.

Examples

Review Policy

v1.3.0

Change Management

v2.0.0

Approval Matrix

v1.1.0

---

# ADR Versioning

Accepted ADRs are immutable.

Minor editorial corrections

↓

PATCH

Architectural decisions themselves

↓

Never modified

New decisions require new ADRs.

---

# RFC Versioning

RFCs evolve while under discussion.

Possible statuses:

Draft

Review

Approved

Implemented

Rejected

Version history should remain visible.

---

# AI Agent Versioning

Each AI Agent maintains:

Identity Version

Behavior Version

Prompt Version

Knowledge Version

Workflow Version

Major behavioral changes require a major version increment.

---

# Prompt Library Versioning

Prompt Packs evolve independently.

Examples

Trading Prompt Pack

v3.1.2

Research Prompt Pack

v2.0.0

Architecture Prompt Pack

v1.5.0

---

# Context Version

Context reflects current project state.

Major repository milestones should update Context versions.

Historical context remains archived.

---

# Memory Version

Institutional Memory is append-only.

Existing records should not be silently rewritten.

Corrections require explicit revision entries.

---

# Documentation Version

Documentation versions indicate documentation maturity.

Suggested stages:

0.x.x

Draft

1.x.x

Production Ready

2.x.x

Major Revision

---

# Release Version

Every release includes:

Repository Version

Release Date

Release Notes

Compatibility Statement

Migration Notes

Supported Versions

---

# Compatibility Policy

Every release should specify:

Backward Compatible

Partially Compatible

Breaking Change

Compatibility expectations must be documented.

---

# Deprecation Policy

Deprecated artifacts must include:

Deprecation Date

Replacement

Removal Timeline

Migration Guidance

Deprecated artifacts remain accessible until officially retired.

---

# Version Tags

Recommended Git tags:

v1.0.0

v1.1.0

v2.0.0

Documentation tags may include:

docs-v1.0.0

constitution-v2.0.0

aes-v1.4.0

---

# Release Documentation

Every release should publish:

Release Notes

Change Log

Migration Guide

Compatibility Matrix

Known Issues

Resolved Issues

Version Summary

---

# Compliance Checklist

Before releasing verify:

✓ Version updated

✓ Change log updated

✓ Release notes prepared

✓ Compatibility documented

✓ Migration documented

✓ Cross references verified

✓ Previous versions archived

---

# Cross References

Engineering Governance Framework

Release Governance

Document Lifecycle

Change Management

ADR Framework

RFC Framework

Engineering Standards

---

# Change Log

| Version | Date | Description |
|----------|------------|------------------------------|
| 1.0.0 | 2026-07-06 | Initial Version Governance Policy |