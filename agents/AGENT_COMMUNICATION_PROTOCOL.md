# Agent Communication Protocol (ACP)

**Document ID:** AOS-004

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

The Agent Communication Protocol (ACP) defines the official communication standard for all AI agents operating within the AITOS AI Operating System (AOS).

ACP standardizes how AI agents:

- communicate,
- exchange context,
- delegate work,
- coordinate execution,
- synchronize memory,
- resolve conflicts,
- report failures,
- maintain auditability.

The protocol enables reliable, secure, and governed multi-agent collaboration.

---

# Scope

ACP applies to:

- AI Agents
- Human-Agent collaboration
- Agent-to-Agent communication
- Agent orchestration
- Distributed AI workflows
- External AI integrations

---

# Communication Principles

All communication shall be:

- Structured
- Traceable
- Context-aware
- Secure
- Deterministic where possible
- Human-reviewable
- Version controlled

Messages should never bypass repository governance.

---

# Communication Architecture

```
Human
   │
   ▼
Coordinator Agent
   │
   ├─────────────┐
   ▼             ▼
Research      Architecture
   │             │
   ▼             ▼
Documentation  Coding
      │
      ▼
Testing
      │
      ▼
Security
      │
      ▼
Repository
```

Coordinator agents orchestrate.

Specialized agents execute.

---

# Message Lifecycle

```
Create
   ↓
Validate
   ↓
Route
   ↓
Receive
   ↓
Process
   ↓
Respond
   ↓
Log
   ↓
Archive
```

Every significant communication should be logged.

---

# Message Structure

Every ACP message should contain:

- Message ID
- Protocol Version
- Timestamp
- Sender
- Receiver
- Conversation ID
- Task ID
- Message Type
- Priority
- Context Reference
- Memory Reference
- Payload
- Status

---

# Message Types

## REQUEST

Requests work from another agent.

---

## RESPONSE

Returns requested information.

---

## PROPOSAL

Suggests a solution requiring review.

---

## APPROVAL_REQUEST

Requests human approval.

---

## REVIEW

Provides review findings.

---

## CONTEXT_UPDATE

Shares updated context.

---

## MEMORY_UPDATE

Publishes institutional memory changes.

---

## WARNING

Reports potential risks.

---

## ERROR

Reports execution failures.

---

## EVENT

Broadcasts lifecycle or operational events.

---

# Priority Levels

P0 — Critical

Immediate attention required.

---

P1 — High

Important engineering work.

---

P2 — Normal

Standard engineering communication.

---

P3 — Low

Informational messages.

---

# Task Delegation

Delegation is permitted only when:

- The receiving agent has the required capability.
- Required permissions exist.
- Responsibilities remain traceable.

Delegation transfers execution.

Delegation never transfers accountability.

---

# Context Exchange

Before processing work an agent should receive:

- Relevant repository context
- Active standards
- Applicable ADRs
- Active RFCs
- Project metadata

Context exchange should minimize unnecessary information while preserving correctness.

---

# Memory Synchronization

Agents should synchronize:

- Engineering decisions
- Accepted patterns
- Lessons learned
- New terminology
- Workflow improvements

Memory synchronization should occur only after validated changes.

---

# Conflict Resolution

When conflicting outputs occur:

1. Detect conflict.
2. Compare supporting evidence.
3. Escalate unresolved conflicts.
4. Request human review.
5. Record final resolution.

Conflict resolution should preserve engineering history.

---

# Failure Handling

Failures include:

- Context loading failure
- Memory inconsistency
- Permission denial
- Invalid task
- Dependency failure
- Timeout
- Unsupported capability

Recovery should prioritize consistency over speed.

---

# Retry Policy

Retry only when:

- Failure is transient.
- Context remains valid.
- Duplicate execution is safe.

Maximum retry limits should be configurable.

---

# Security Requirements

All communications should:

- Respect permission boundaries
- Avoid exposing sensitive information
- Validate sender identity
- Validate receiver capability
- Preserve message integrity

Security policies take precedence over communication convenience.

---

# Human Interaction

Human review is mandatory for:

- Governance decisions
- Architectural changes
- Security-sensitive operations
- Production releases
- Risk acceptance

ACP should support seamless human intervention.

---

# Audit Logging

Every significant message should be logged with:

- Message ID
- Timestamp
- Sender
- Receiver
- Task
- Outcome
- Review status
- Related ADR
- Related RFC

Audit logs should be immutable.

---

# Compliance

Before accepting communication verify:

✓ Protocol version supported

✓ Sender registered

✓ Receiver registered

✓ Capability verified

✓ Permissions verified

✓ Context available

✓ Memory synchronized

✓ Logging enabled

---

# Performance Objectives

ACP should optimize for:

- Reliability
- Low ambiguity
- High traceability
- Context accuracy
- Minimal duplication
- Scalable orchestration

---

# Cross References

- AI Operating System (AOS)
- AI Agent Governance
- AI Agent Lifecycle
- AI Agent Capability Model
- Agent Memory Model
- Agent Context Loading
- Agent Registry
- Engineering Governance Framework

---

# Change Log

| Version | Date | Description |
|----------|------------|----------------------------------------------|
| 1.0.0 | 2026-07-06 | Initial Agent Communication Protocol (ACP) |