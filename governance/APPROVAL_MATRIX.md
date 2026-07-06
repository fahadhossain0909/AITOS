# Approval Matrix

**Document ID:** GOV-006

**Version:** 1.0.0

**Status:** Active

**Owner:** Project Owner

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This document defines the approval authority for all significant engineering changes within the AITOS project.

Its objectives are to:

- Ensure proper governance
- Prevent unauthorized changes
- Clarify approval responsibilities
- Reduce ambiguity
- Maintain engineering accountability
- Support auditability

Approval authority applies to both engineering and governance artifacts.

---

# Scope

This policy applies to:

- Documentation
- Source Code
- Architecture
- Standards
- Governance
- Security
- Infrastructure
- AI Agents
- Research
- Releases
- Repository Structure

---

# Approval Principles

## Principle 1 — Authority Follows Responsibility

Approval authority belongs to the accountable owner.

---

## Principle 2 — Separation of Duties

The contributor should not be the sole approver of their own significant change.

Independent review is required.

---

## Principle 3 — Risk-Based Approval

Higher-risk changes require higher approval authority.

---

## Principle 4 — Traceability

Every approval must be documented.

Approvals should reference:

- RFC (if applicable)
- ADR (if applicable)
- Pull Request
- Release Version

---

# Approval Levels

## Level A — Maintainer

May approve:

- Editorial changes
- Formatting
- Documentation fixes
- Internal links

---

## Level B — Engineering Lead

May approve:

- Minor engineering changes
- Refactoring
- Test improvements
- Non-breaking code updates

---

## Level C — System Architect

May approve:

- Architecture changes
- New modules
- Repository restructuring
- Design decisions
- ADR acceptance

---

## Level D — Project Owner

Required for:

- Governance updates
- Constitution changes
- Strategic architecture
- Major releases
- Repository policy changes

---

## Level E — Emergency Authority

Activated only during:

- Critical production incidents
- Severe security vulnerabilities
- Regulatory emergencies

Emergency approvals are temporary and require retrospective review.

---

# Approval Matrix

| Change Type | Reviewer | Approver | RFC | ADR |
|--------------|----------|----------|-----|-----|
| Editorial Documentation | Documentation Lead | Maintainer | No | No |
| Documentation Structure | Documentation Lead | Engineering Lead | Optional | No |
| Source Code (Minor) | Senior Developer | Engineering Lead | No | No |
| Source Code (Major) | Engineering Lead | Architect | Yes | Optional |
| Architecture | Architect | Project Owner | Yes | Yes |
| Repository Structure | Architect | Project Owner | Yes | Yes |
| Engineering Standards | Engineering Lead | Project Owner | Yes | Yes |
| Governance | Architect | Project Owner | Yes | Yes |
| Security | Security Reviewer | Project Owner | Yes | Yes |
| Infrastructure | Architect | Project Owner | Yes | Optional |
| AI Agent Definitions | AI Reviewer | Architect | Yes | Optional |
| Release Process | Release Manager | Project Owner | Optional | No |

---

# Multi-Level Approval

Certain changes require multiple approvals.

Examples:

Architecture

↓

Architect

↓

Project Owner

Security

↓

Security Reviewer

↓

Project Owner

Major Release

↓

Engineering Lead

↓

Release Manager

↓

Project Owner

---

# Approval Requirements

Before approval verify:

✓ Reviews completed

✓ Standards followed

✓ Documentation updated

✓ Tests passed

✓ Risks assessed

✓ Rollback plan prepared

✓ Context updated

✓ Memory updated

---

# Emergency Approval

Emergency approval may be granted only when:

- Production outage exists.
- Critical security exposure exists.
- Regulatory compliance requires immediate action.
- Data integrity is at risk.

Emergency changes require:

- Immediate documentation
- Post-implementation review
- Root Cause Analysis
- ADR/RFC updates (if applicable)

---

# Approval Records

Every approved change should record:

- Change ID
- Title
- Approval Date
- Reviewer(s)
- Approver(s)
- Decision
- Related RFC
- Related ADR
- Release Version

Approval records should be retained for audit purposes.

---

# Approval Revocation

An approval may be revoked if:

- New critical risks emerge.
- Significant defects are identified.
- Governance violations are discovered.
- Approval was granted using incomplete information.

Revocation must be documented.

---

# Exceptions

No exception may bypass:

- Project Constitution
- Mandatory Security Review
- Legal or Regulatory Requirements

Emergency exceptions remain subject to retrospective governance.

---

# Compliance

A change must not proceed when:

✗ Required approvals are missing.

✗ Review requirements are incomplete.

✗ Mandatory documentation is absent.

✗ Security approval is outstanding.

✗ Governance requirements are violated.

---

# Cross References

- Engineering Governance Framework
- Ownership Model
- Review Policy
- Change Management
- Release Governance
- ADR Framework
- RFC Framework

---

# Change Log

| Version | Date | Description |
|----------|------------|--------------------------------|
| 1.0.0 | 2026-07-06 | Initial Approval Matrix |