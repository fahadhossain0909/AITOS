# Evaluation Schema

**Document ID:** AOS-SPEC-006

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the canonical machine-readable schema for AI Agent Evaluation Reports within the AITOS AI Operating System (AOS).

Every formal evaluation SHALL produce an Evaluation Report conforming to this schema.

The Evaluation Schema standardizes quality measurement, benchmarking, governance compliance, certification, and continuous improvement.

---

# Objectives

The Evaluation Schema enables:

- Repeatable evaluation
- Objective scoring
- Certification automation
- Governance verification
- Security assessment
- Reliability measurement
- Auditability
- Historical comparison

---

# Design Principles

Every Evaluation Report SHALL be:

- Objective
- Evidence-based
- Version controlled
- Machine-readable
- Auditable
- Reproducible
- Traceable

---

# Evaluation Architecture

```
Evaluation Report
│
├── Metadata
├── Subject
├── Evaluation Scope
├── Benchmark Results
├── Quality Metrics
├── Security Results
├── Governance Results
├── Reliability Results
├── Hallucination Analysis
├── Human Review
├── Certification
├── Recommendations
└── Audit
```

---

# Canonical JSON Example

```json
{
  "schemaVersion": "1.0.0",

  "evaluationId": "EVAL-000001",

  "agentId": "AOS-AGENT-0007",

  "version": "2.1.0",

  "scope": "Production",

  "scores": {
    "accuracy": 97,
    "reliability": 95,
    "security": 100,
    "governance": 99
  },

  "hallucinationRate": 0.3,

  "certification": {
    "status": "CERTIFIED",
    "level": "D"
  },

  "status": "COMPLETED"
}
```

---

# Required Fields

Every Evaluation Report SHALL include:

- schemaVersion
- evaluationId
- agentId
- version
- scope
- scores
- certification
- status

---

# Evaluation Identifier

Format:

```
EVAL-000001
```

Identifiers are globally unique and immutable.

---

# Evaluation Scope

Supported values:

- Experimental
- Development
- Internal
- Production
- Recertification
- Incident Review

---

# Benchmark Results

Benchmark categories MAY include:

- Documentation
- Architecture
- Coding
- Testing
- Security
- Governance
- Research
- Multi-Agent Collaboration

Each benchmark SHOULD include:

- Test Name
- Result
- Score
- Pass/Fail
- Evidence Reference

---

# Quality Metrics

Recommended metrics:

- Accuracy
- Completeness
- Consistency
- Maintainability
- Explainability
- Performance

Scores SHALL use a normalized 0–100 scale unless otherwise specified.

---

# Security Results

Evaluation SHALL assess:

- Authentication
- Authorization
- Prompt Injection Resistance
- Secret Handling
- Context Isolation
- Audit Logging

Security findings SHALL be classified by severity.

---

# Governance Results

Governance compliance SHALL verify adherence to:

- Constitution
- AES Standards
- ADR Decisions
- RFC Requirements
- Governance Policies

Compliance SHALL be reported as both score and findings.

---

# Reliability Results

Recommended metrics:

- Success Rate
- Failure Rate
- Recovery Rate
- Determinism
- Repeatability
- Stability

Reliability SHALL be measured using representative workloads.

---

# Hallucination Analysis

The report SHALL include:

- Hallucination Rate
- Verified Findings
- Unsupported Claims
- Severity Classification
- Detection Method

Hallucination metrics SHALL be evidence-based.

---

# Human Review

Required fields:

- Reviewer
- Review Date
- Review Score
- Decision

Supported decisions:

- Approved
- Approved with Conditions
- Rejected
- Re-evaluation Required

Human review remains authoritative for certification.

---

# Certification

Supported levels:

- Level A — Experimental
- Level B — Development
- Level C — Operational
- Level D — Production
- Level E — Reference

Supported status:

- Pending
- Certified
- Suspended
- Revoked

---

# Recommendations

Recommendations SHALL be categorized:

- Critical
- High
- Medium
- Low
- Informational

Each recommendation SHOULD include an owner and target completion date.

---

# Validation Rules

A valid Evaluation Report MUST satisfy:

✓ Evaluation ID assigned

✓ Agent exists

✓ Version defined

✓ Scope declared

✓ Scores present

✓ Certification recorded

✓ Human review completed

✓ Status valid

---

# Security

Evaluation Reports SHALL NOT include:

- Secrets
- Credentials
- Private keys
- Authentication tokens

Evidence SHALL be sanitized before publication.

---

# Audit Metadata

Recommended fields:

- Evaluated By
- Evaluation Date
- Reviewed By
- Certification Date
- Change History

Audit records SHALL be retained according to governance policy.

---

# Versioning

Evaluation Reports SHALL use Semantic Versioning.

Schema evolution SHALL preserve backward compatibility where practical.

---

# Compliance Checklist

Before publication verify:

✓ Evaluation completed

✓ Benchmarks executed

✓ Security reviewed

✓ Governance reviewed

✓ Reliability measured

✓ Human review completed

✓ Certification assigned

✓ Audit metadata recorded

---

# Cross References

- AOS-008 Agent Evaluation
- AOS-009 Agent Registry
- AOS-007 Agent Security Policy
- AOS-SPEC-002 Agent Manifest Schema
- AOS-SPEC-005 Registry Schema

---

# Change Log

| Version | Date | Description |
|----------|------------|-----------------------------------|
| 1.0.0 | 2026-07-06 | Initial Evaluation Schema |