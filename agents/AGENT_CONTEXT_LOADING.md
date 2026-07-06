# AI Agent Context Loading

**Document ID:** AOS-006

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This document defines the Context Management Standard for AI agents operating within the AITOS AI Operating System (AOS).

Its purpose is to ensure that every AI agent loads the right context, at the right time, in the right order, while maintaining consistency, traceability, and governance compliance.

Context loading should reduce ambiguity, improve decision quality, and eliminate unnecessary or conflicting information.

---

# Scope

This policy applies to:

- AI Agents
- Context Layer
- Memory Layer
- Prompt Packs
- ADRs
- RFCs
- Engineering Standards
- Governance Documents
- Architecture
- Requirements
- Repository Metadata

---

# Context Principles

Context is authoritative only when it originates from governed repository artifacts.

Agents shall:

- Prefer repository context over assumptions.
- Load only relevant context.
- Preserve context consistency.
- Respect context versioning.
- Synchronize context after approved changes.

Context should be minimal, relevant, and sufficient.

---

# Context Architecture

```
Repository
     │
     ▼
Repository Metadata
     │
     ▼
Governance
     │
     ▼
Engineering Standards
     │
     ▼
Architecture
     │
     ▼
Context Layer
     │
     ▼
Institutional Memory
     │
     ▼
Task Context
     │
     ▼
Execution
```

---

# Context Hierarchy

Priority (highest to lowest):

1. Active Task Instructions
2. Constitution
3. Governance Policies
4. Engineering Standards (AES)
5. Approved ADRs
6. Active RFCs
7. Architecture
8. Requirements
9. Context Layer
10. Institutional Memory
11. Research Documents
12. Historical Records

Higher-priority context overrides lower-priority context when conflicts exist.

---

# Context Lifecycle

```
Identify
   │
   ▼
Select
   │
   ▼
Validate
   │
   ▼
Load
   │
   ▼
Apply
   │
   ▼
Monitor
   │
   ▼
Refresh
   │
   ▼
Release
```

Each stage shall be traceable.

---

# Context Selection

Before execution an agent should identify:

- Task objective
- Repository area
- Required standards
- Related architecture
- Applicable governance
- Historical decisions
- Dependencies

Selection should prioritize relevance over volume.

---

# Context Validation

Validate that context is:

- Current
- Approved
- Version compatible
- Internally consistent
- Accessible
- Applicable

Invalid or deprecated context should not be treated as authoritative.

---

# Context Loading Strategy

Recommended loading order:

1. Task Definition
2. Constitution
3. Governance
4. Engineering Standards
5. ADRs
6. RFCs
7. Architecture
8. Context Layer
9. Memory Layer
10. Supporting Research

Agents should avoid loading unrelated artifacts.

---

# Context Refresh

Refresh context when:

- A task changes.
- An ADR is approved.
- Governance is updated.
- Standards change.
- Repository version changes.
- Memory synchronization occurs.

Long-running workflows should periodically verify context freshness.

---

# Context Synchronization

Synchronization should occur after:

- Repository releases
- Governance updates
- Architecture revisions
- Standards revisions
- Context updates
- Memory updates

All participating agents should operate on compatible context versions.

---

# Multi-Agent Context Sharing

Shared context shall:

- Be version controlled.
- Preserve provenance.
- Maintain consistency.
- Prevent duplication.
- Record synchronization events.

Agents should exchange references rather than duplicate large context payloads whenever practical.

---

# Conflict Resolution

When conflicting context exists:

1. Identify conflicting sources.
2. Compare authority levels.
3. Prefer higher-priority governed artifacts.
4. Escalate unresolved conflicts.
5. Record the resolution.

Conflict resolution should never silently discard authoritative information.

---

# Context Security

Context access shall follow least-privilege principles.

Agents shall not:

- Access restricted context without authorization.
- Modify protected context.
- Expose sensitive information unnecessarily.

Protected context should remain auditable.

---

# Performance Guidelines

Context loading should optimize for:

- Relevance
- Accuracy
- Freshness
- Low latency
- Minimal duplication
- Predictable behavior

Loading excessive context should be avoided.

---

# Compliance Checklist

Before execution verify:

✓ Task identified

✓ Context selected

✓ Context validated

✓ Correct versions loaded

✓ ADRs checked

✓ RFCs checked

✓ Memory synchronized

✓ Security requirements satisfied

---

# Success Metrics

Evaluate context management using:

- Context retrieval accuracy
- Context freshness
- Synchronization success rate
- Duplicate context reduction
- Context conflict frequency
- Task success rate
- Human review acceptance

---

# Cross References

- AI Operating System (AOS)
- AI Agent Governance
- AI Agent Lifecycle
- AI Agent Memory Model
- Agent Communication Protocol
- Agent Security Policy
- Context Layer
- Memory Layer
- ADR Framework
- RFC Framework
- Engineering Governance Framework

---

# Change Log

| Version | Date | Description |
|----------|------------|-------------------------------------------|
| 1.0.0 | 2026-07-06 | Initial AI Agent Context Management Standard |