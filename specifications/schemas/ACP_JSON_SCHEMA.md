# Agent Communication Protocol (ACP) JSON Schema

**Document ID:** AOS-SPEC-001

**Version:** 1.0.0

**Status:** Active

**Owner:** AITOS Engineering

**Classification:** Internal

**Last Updated:** 2026-07-06

---

# Purpose

This specification defines the canonical JSON message contract for the Agent Communication Protocol (ACP).

Every AI agent operating within AITOS shall exchange structured messages using this schema to ensure interoperability, validation, traceability, and governance compliance.

This specification complements the Agent Communication Protocol (AOS-004).

---

# Scope

This schema applies to:

- Agent-to-Agent communication
- Human-Agent interaction
- Workflow orchestration
- Event notifications
- Context synchronization
- Memory synchronization
- Task delegation
- Review workflows

---

# Design Principles

ACP messages shall be:

- Structured
- Deterministic
- Self-describing
- Versioned
- Immutable after transmission
- Auditable

---

# Canonical Message Structure

```json
{
  "schemaVersion": "1.0.0",
  "messageId": "MSG-20260706-000001",
  "conversationId": "CONV-001",
  "taskId": "TASK-001",

  "timestamp": "2026-07-06T10:30:00Z",

  "sender": {
    "agentId": "AOS-AGENT-0001",
    "name": "Architecture Agent",
    "version": "2.0.0"
  },

  "receiver": {
    "agentId": "AOS-AGENT-0002",
    "name": "Documentation Agent"
  },

  "type": "REQUEST",

  "priority": "P1",

  "context": {
    "contextVersion": "1.4.0",
    "references": [
      "ADR-0007",
      "AES-004"
    ]
  },

  "memory": {
    "memoryVersion": "2.3",
    "references": [
      "MEM-00012"
    ]
  },

  "payload": {

  },

  "status": "NEW",

  "metadata": {

  }
}
```

---

# Top-Level Fields

| Field | Required | Description |
|--------|----------|-------------|
| schemaVersion | Yes | ACP schema version |
| messageId | Yes | Globally unique message identifier |
| conversationId | Yes | Conversation correlation identifier |
| taskId | Yes | Task correlation identifier |
| timestamp | Yes | ISO-8601 UTC timestamp |
| sender | Yes | Message origin |
| receiver | Yes | Intended recipient |
| type | Yes | Message type |
| priority | Yes | Processing priority |
| context | Optional | Context references |
| memory | Optional | Memory references |
| payload | Yes | Message content |
| status | Yes | Processing state |
| metadata | Optional | Additional metadata |

---

# Sender Object

Required fields:

```json
{
  "agentId": "AOS-AGENT-0001",
  "name": "Architecture Agent",
  "version": "2.0.0"
}
```

Rules:

- agentId MUST exist in Agent Registry.
- version MUST identify executing version.
- sender identity MUST be authenticated.

---

# Receiver Object

```json
{
  "agentId": "AOS-AGENT-0005",
  "name": "Testing Agent"
}
```

Rules:

- Receiver MUST be registered.
- Receiver capability SHALL match requested task.

---

# Message Types

Supported values:

- REQUEST
- RESPONSE
- PROPOSAL
- REVIEW
- APPROVAL_REQUEST
- APPROVAL_RESPONSE
- CONTEXT_UPDATE
- MEMORY_UPDATE
- EVENT
- WARNING
- ERROR
- HEARTBEAT

Unknown message types MUST be rejected.

---

# Priority Levels

| Value | Meaning |
|---------|----------|
| P0 | Critical |
| P1 | High |
| P2 | Normal |
| P3 | Low |

Priority affects scheduling only.

It shall not override governance.

---

# Payload

Payload SHALL contain task-specific data.

Rules:

- Valid JSON
- UTF-8
- No executable code
- Schema compliant
- Self-contained where practical

---

# Context References

Example

```json
{
  "references": [
    "ADR-0002",
    "RFC-0011",
    "AES-003"
  ]
}
```

Rules:

References SHOULD identify governed repository artifacts.

---

# Memory References

```json
{
  "references": [
    "MEM-0042",
    "MEM-0150"
  ]
}
```

Rules:

Referenced memory MUST exist.

---

# Metadata

Optional metadata may include:

```json
{
  "correlationId":"...",
  "workflowId":"...",
  "traceId":"...",
  "retryCount":1
}
```

Metadata SHALL NOT alter protocol semantics.

---

# Processing Status

Supported values:

- NEW
- RECEIVED
- VALIDATED
- PROCESSING
- COMPLETED
- FAILED
- REJECTED
- CANCELLED
- ARCHIVED

State transitions shall be auditable.

---

# Validation Rules

Every ACP message MUST satisfy:

✓ Valid JSON

✓ Required fields present

✓ Valid schema version

✓ Registered sender

✓ Registered receiver

✓ Valid timestamp

✓ Supported message type

✓ Valid processing state

✓ Payload integrity

Invalid messages SHALL be rejected.

---

# Version Compatibility

Rules:

Minor versions:

Backward compatible.

Major versions:

Compatibility not guaranteed.

Agents SHOULD negotiate supported versions before communication.

---

# Error Handling

Validation failures should produce standardized errors.

Examples:

ACP-1001

Missing required field

ACP-1002

Unknown message type

ACP-1003

Unsupported version

ACP-1004

Invalid sender

ACP-1005

Invalid receiver

ACP-1006

Payload validation failed

---

# Security Requirements

Messages SHALL:

- preserve integrity
- verify sender identity
- respect permission boundaries
- avoid secret disclosure
- support audit logging

---

# Audit Requirements

Every processed message should record:

- Message ID
- Timestamp
- Sender
- Receiver
- Status
- Processing outcome
- Related Task ID
- Related Workflow ID

Audit records shall be immutable.

---

# Example Valid Message

```json
{
  "schemaVersion":"1.0.0",
  "messageId":"MSG-001",
  "conversationId":"CONV-15",
  "taskId":"TASK-88",
  "timestamp":"2026-07-06T12:00:00Z",
  "sender":{
      "agentId":"AOS-AGENT-0001",
      "name":"Coordinator Agent",
      "version":"1.2.0"
  },
  "receiver":{
      "agentId":"AOS-AGENT-0007",
      "name":"Security Agent"
  },
  "type":"REQUEST",
  "priority":"P1",
  "payload":{
      "action":"Security Review"
  },
  "status":"NEW"
}
```

---

# Compliance Checklist

Before transmission verify:

✓ JSON valid

✓ Schema valid

✓ Sender authenticated

✓ Receiver registered

✓ Required fields complete

✓ Payload validated

✓ Context references verified

✓ Security requirements satisfied

---

# Cross References

- AOS-004 Agent Communication Protocol
- AOS-003 Agent Capability Model
- AOS-005 Agent Memory Model
- AOS-006 Agent Context Loading
- AOS-007 Agent Security Policy
- AOS-009 Agent Registry

---

# Change Log

| Version | Date | Description |
|----------|------------|--------------------------------------------|
| 1.0.0 | 2026-07-06 | Initial ACP JSON Schema |