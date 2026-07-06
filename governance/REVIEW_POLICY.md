# Review Policy

**Document ID:** GOV-003

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This document defines the mandatory review process for all engineering artifacts within AITOS.

The objective is to ensure that every significant change is:

- Correct
- Secure
- Maintainable
- Well documented
- Standards compliant
- Architecturally consistent

No significant change should be merged without the appropriate review.

---

# Scope

This policy applies to:

- Source Code
- Documentation
- Architecture
- RFCs
- ADRs
- Standards
- Context Layer
- Memory Layer
- AI-generated content
- Infrastructure configuration

---

# Review Principles

Engineering reviews should be:

- Objective
- Evidence-based
- Respectful
- Constructive
- Traceable
- Reproducible

Reviews improve the system—not criticize the contributor.

---

# Review Types

## 1. Architecture Review

Purpose

Ensure architectural consistency.

Verify:

- Layer boundaries
- Dependency direction
- Modularity
- Scalability
- Maintainability
- ADR compliance

Required For

- New subsystems
- Architectural changes
- Repository restructuring
- Infrastructure redesign

---

## 2. Code Review

Purpose

Ensure implementation quality.

Verify:

- Correctness
- Readability
- Simplicity
- Test coverage
- Error handling
- Naming consistency
- Coding standards compliance

Required For

All production code.

---

## 3. Documentation Review

Purpose

Ensure documentation accuracy.

Verify:

- Technical correctness
- Completeness
- Cross references
- Formatting consistency
- Version information
- Update history

Documentation should evolve together with implementation.

---

## 4. Security Review

Purpose

Identify security risks.

Verify:

- Authentication
- Authorization
- Secret management
- Dependency risks
- Input validation
- Sensitive data exposure
- Compliance

Required For

- Authentication changes
- Infrastructure
- Deployment
- External integrations

---

## 5. AI Review

Purpose

Validate AI-generated work.

Verify:

- Hallucinations
- Unsupported assumptions
- Standards compliance
- Context awareness
- ADR alignment
- Documentation updates

AI output is always subject to human review.

---

# Review Workflow

```
Submission

↓

Reviewer Assignment

↓

Review

↓

Feedback

↓

Revision

↓

Approval

↓

Merge
```

---

# Reviewer Roles

## Project Owner

Approves:

- Governance
- Constitution
- Strategic architecture

---

## System Architect

Reviews:

- Architecture
- RFCs
- ADRs
- System design

---

## Engineering Lead

Reviews:

- Code quality
- Engineering standards
- Repository consistency

---

## Security Reviewer

Reviews:

- Security controls
- Infrastructure
- Compliance

---

## Documentation Lead

Reviews:

- Documentation quality
- Cross references
- Context updates

---

## AI Reviewer

Reviews:

- AI-generated content
- Prompt quality
- Context usage
- AI workflow compliance

---

# Review Checklist

## Architecture

✓ Modular

✓ Low coupling

✓ High cohesion

✓ ADR compliant

✓ Future extensibility

---

## Code

✓ Correct

✓ Readable

✓ Tested

✓ Maintainable

✓ Standards compliant

---

## Documentation

✓ Updated

✓ Accurate

✓ Linked

✓ Versioned

✓ Complete

---

## Security

✓ No exposed secrets

✓ Dependencies reviewed

✓ Access control verified

✓ Validation present

✓ Compliance maintained

---

## AI

✓ Context loaded

✓ Memory considered

✓ Standards followed

✓ Documentation updated

✓ No unsupported assumptions

---

# Review Outcomes

## Approved

Ready for merge.

---

## Approved with Comments

Minor improvements may be completed after merge.

---

## Changes Requested

Merge blocked until issues are resolved.

---

## Rejected

Proposal is fundamentally unsuitable.

A new submission is required.

---

# Merge Criteria

A change may be merged only if:

✓ Required reviews completed

✓ Blocking comments resolved

✓ Tests passed

✓ Documentation updated

✓ ADR/RFC updated (if required)

✓ Standards satisfied

✓ No unresolved critical issues

---

# Review Metrics

The project should monitor:

- Review completion rate
- Average review time
- Defect escape rate
- Documentation compliance
- AI correction rate
- Security issue detection rate

Metrics should be reviewed periodically to improve the review process.

---

# Exceptions

Emergency changes may receive an expedited review.

Requirements:

- Review after deployment
- Root cause analysis
- Documentation update
- Lessons learned
- Governance record

Emergency review is temporary—not optional.

---

# Compliance

Failure to follow this policy may result in:

- Merge rejection
- Release delay
- Governance review
- Corrective actions

---

# Cross References

- Engineering Governance Framework
- Engineering Decision Workflow
- Change Management
- Release Governance
- ADR Framework
- RFC Framework
- Engineering Standards (AES)
- Context Layer
- Memory Layer

---

# Change Log

| Version | Date | Description |
|----------|------------|--------------------------------|
| 1.0.0 | 2026-07-06 | Initial Review Policy |