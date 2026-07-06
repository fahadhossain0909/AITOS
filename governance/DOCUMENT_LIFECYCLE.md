# Document Lifecycle

**Document ID:** GOV-009

**Version:** 1.0.0

**Status:** Active

**Owner:** Documentation Lead

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This document defines the official lifecycle governing every engineering artifact in the AITOS repository.

AITOS treats documentation as a first-class engineering artifact.

Every document must have:

- a defined purpose,
- a known owner,
- a controlled lifecycle,
- version history,
- review requirements,
- retirement rules.

---

# Scope

This policy applies to:

- Constitution
- Engineering Standards (AES)
- Governance
- ADR
- RFC
- Architecture
- Requirements
- Research
- Context
- Memory
- AI Prompt Packs
- AI Agent Specifications
- Source Documentation
- Release Documentation

---

# Core Principles

Documentation is engineering.

Documentation is version controlled.

Documentation evolves continuously.

Documentation preserves institutional knowledge.

No production artifact should exist without corresponding documentation.

---

# Knowledge Lifecycle

```
Idea
   │
   ▼
Draft
   │
   ▼
Technical Review
   │
   ▼
Approved
   │
   ▼
Published
   │
   ▼
Implemented
   │
   ▼
Referenced
   │
   ▼
Maintained
   │
   ▼
Deprecated
   │
   ▼
Archived
   │
   ▼
Historical Record
```

No stage should be skipped except under documented emergency procedures.

---

# Lifecycle Stages

## Idea

A concept has been identified.

Outputs:

- Initial notes
- Problem statement
- Proposed scope

---

## Draft

Initial engineering document.

May contain incomplete sections.

Not authoritative.

---

## Technical Review

Subject matter experts review:

- correctness,
- consistency,
- feasibility,
- completeness.

Changes may be requested.

---

## Approved

The document has received all required approvals.

Approved documents become official engineering references.

---

## Published

Document is released into the active repository.

Cross-references should now be updated.

---

## Implemented

The documented process, architecture or feature has been implemented.

Implementation should remain aligned with the approved document.

---

## Referenced

The document is actively used by:

- ADRs
- RFCs
- Standards
- Architecture
- AI Context
- Engineering workflows

Referenced documents become part of institutional knowledge.

---

## Maintained

Active documents require periodic maintenance.

Maintenance includes:

- accuracy verification,
- link validation,
- terminology consistency,
- version updates.

---

## Deprecated

The document is no longer recommended.

Requirements:

- Deprecation notice
- Replacement reference (if applicable)
- Effective date
- Migration guidance

Deprecated documents remain accessible.

---

## Archived

No longer active.

Retained for historical and audit purposes.

Archived documents are read-only.

---

## Historical Record

Final preservation stage.

Documents remain permanently available for:

- audits,
- research,
- architectural history,
- engineering traceability.

Historical records must never be silently modified.

---

# Document Maturity Model

| Level | Description |
|--------|-------------|
| L0 | Idea |
| L1 | Draft |
| L2 | Reviewed |
| L3 | Approved |
| L4 | Production Ready |
| L5 | Institutional Standard |

Higher maturity levels require stronger governance.

---

# Review Frequency

| Artifact Type | Recommended Review |
|---------------|-------------------|
| Constitution | Annual |
| Governance | Every 6 months |
| AES Standards | Every 6 months |
| ADR | When superseded |
| RFC | During active discussion |
| Architecture | Every major release |
| Requirements | Each release cycle |
| Context | Continuous |
| Memory | Continuous |
| AI Prompts | Quarterly |

Emergency reviews may occur at any time.

---

# Retention Policy

Retention periods:

- Constitution → Permanent
- ADR → Permanent
- Governance → Permanent
- Standards → Permanent
- RFC → Permanent
- Architecture → Permanent
- Release Notes → Permanent
- Research → Permanent unless explicitly retired

Permanent means version history must be preserved.

---

# Archive Policy

Archived documents:

- remain searchable,
- retain version history,
- preserve references,
- are excluded from active workflows.

Archive status should be clearly indicated.

---

# Cross-reference Integrity

Every governed document should maintain valid references to related artifacts.

Cross-references should be checked during:

- review,
- release,
- repository audits.

Broken references should be treated as documentation defects.

---

# AI Context Synchronization

Whenever a production document changes, evaluate whether updates are required to:

- Context Layer
- Memory Layer
- Prompt Library
- AI Agent Specifications
- ADR references
- RFC references

AI-facing knowledge should remain synchronized with authoritative documentation.

---

# Ownership and Accountability

Each document shall define:

- Owner
- Maintainer
- Reviewer
- Approver
- Current Status
- Version

Documents without ownership are non-compliant.

---

# Lifecycle Transitions

Every transition should be recorded.

Examples:

Draft → Reviewed

Reviewed → Approved

Approved → Published

Published → Deprecated

Deprecated → Archived

Transition history forms part of the engineering audit trail.

---

# Compliance Checklist

Before publishing verify:

✓ Metadata complete

✓ Owner assigned

✓ Review completed

✓ Approval recorded

✓ Version updated

✓ Cross-references validated

✓ Related ADR/RFC updated

✓ Context synchronized

✓ Memory synchronized

---

# Governance Metrics

Measure:

- Documentation coverage
- Review completion rate
- Broken reference count
- Outdated document count
- Average document age since review
- AI synchronization status
- Documentation maturity distribution

Metrics should support continuous improvement.

---

# Exceptions

Emergency documentation may be published before the normal review process only when operational continuity requires it.

A retrospective review must be completed as soon as practical.

---

# Cross References

- Engineering Governance Framework
- Decision Workflow
- Change Management
- Review Policy
- Version Policy
- Ownership Model
- ADR Framework
- RFC Framework
- Context Layer
- Memory Layer

---

# Change Log

| Version | Date | Description |
|----------|------------|--------------------------------|
| 1.0.0 | 2026-07-06 | Initial Document Lifecycle Standard |