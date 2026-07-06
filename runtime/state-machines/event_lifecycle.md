# Event Lifecycle Specification

Document ID: RT-SM-EVENT-LIFECYCLE-001
Version: 1.0.0
Status: Draft (Implementation Ready)
Owner: Runtime Architecture Working Group
Component: EVENT_BUS

---

# 1. Purpose

This document defines the canonical lifecycle of every Event within the AITOS Runtime.

It specifies:

- lifecycle states
- state transitions
- guard conditions
- failure handling
- replay semantics
- audit requirements
- observability requirements

All Event Bus implementations SHALL conform to this specification.

---

# 2. Scope

This specification applies to every event exchanged through the Runtime Event Bus, including:

- Agent Events
- Workflow Events
- Context Events
- Memory Events
- Registry Events
- Governance Events
- Security Events
- System Events

Transport-specific implementations SHALL preserve this lifecycle.

---

# 3. Lifecycle Design Principles

The lifecycle SHALL satisfy the following principles:

1. Every event has a finite lifecycle.
2. Events are immutable.
3. State transitions are deterministic.
4. Invalid transitions are prohibited.
5. Every transition is auditable.
6. Failures are explicit states.
7. Replay creates a new execution, not a new event.
8. Historical state SHALL remain immutable.

---

# 4. Canonical Lifecycle States

The Event Lifecycle consists of the following primary states:

1. Created
2. Validated
3. Authenticated
4. Authorized
5. Enriched
6. Persisted
7. Routed
8. Queued
9. Dispatched
10. Delivered
11. Acknowledged
12. Completed
13. Archived

These states represent the nominal execution path.

---

# 5. Failure States

The following failure states are canonical:

- ValidationFailed
- AuthenticationFailed
- AuthorizationFailed
- RoutingFailed
- DispatchFailed
- DeliveryTimeout
- RetryPending
- RetryExhausted
- DeadLettered
- Cancelled

Failure states SHALL generate Audit Events.

---

# 6. Initial State

Every Event SHALL begin in the **Created** state.

Entry Criteria:

- Event object instantiated
- Required metadata assigned
- Event ID generated
- Timestamp assigned

Exit Condition:

- Validation begins

---

# 7. Created State

Purpose:

Represents a newly constructed event that has not yet entered the runtime pipeline.

Entry Actions:

- Allocate Event ID
- Assign Timestamp
- Initialize Correlation ID
- Attach Trace Context

Allowed Transitions:

Created → Validated

Forbidden Transitions:

Created → Routed
Created → Delivered
Created → Completed

---

# 8. Validated State

Purpose:

Confirms structural correctness.

Validation SHALL include:

- JSON Schema validation
- Required fields
- Metadata verification
- Payload integrity
- Version compatibility

Success Transition:

Validated → Authenticated

Failure Transition:

Validated → ValidationFailed

---

# 9. Authenticated State

Purpose:

Verify publisher identity.

Checks MAY include:

- JWT
- mTLS
- Service Identity
- Agent Identity
- API Key (development only)

Success:

Authenticated → Authorized

Failure:

Authenticated → AuthenticationFailed

---

# 10. Authorized State

Purpose:

Determine whether the authenticated publisher is permitted to publish this event.

Policy MAY evaluate:

- Trust Level
- Topic
- Capability
- Role
- Runtime State

Success:

Authorized → Enriched

Failure:

Authorized → AuthorizationFailed

---

# 11. Enriched State

Purpose:

Attach runtime metadata.

Metadata MAY include:

- Runtime Version
- Schema Version
- Retry Policy
- Delivery Policy
- Labels
- Priority

Success:

Enriched → Persisted

---

# 12. Persisted State

Purpose:

Store the immutable event.

Persistence MAY be:

- None
- Memory
- Durable

Once persisted, the event SHALL NOT be modified.

Success:

Persisted → Routed

---

# 13. Routed State

Purpose:

Determine event destinations.

Routing SHALL consider:

- Topic
- Subscription
- Delivery Policy
- Priority
- Trust Constraints

Success:

Routed → Queued

Failure:

Routed → RoutingFailed

---

# 14. Queued State

Purpose:

Await dispatch.

The Event Bus MAY maintain multiple queues based on:

- Priority
- Topic
- Partition
- Subscriber

Success:

Queued → Dispatched

---

# 15. Dispatched State

## 15.1 Purpose

The **Dispatched** state indicates that the Event Bus has assigned the event to one or more delivery channels and has initiated transmission to the intended subscriber(s).

The event remains under runtime supervision until delivery succeeds, fails, or times out.

---

## 15.2 Entry Conditions

An event MAY enter the Dispatched state only if:

- Routing has completed successfully.
- At least one eligible subscriber exists.
- Delivery authorization has been verified.
- Required runtime resources are available.

---

## 15.3 Entry Actions

Upon entering this state, the Dispatcher SHALL:

- Select the appropriate delivery channel.
- Allocate delivery resources.
- Record dispatch timestamp.
- Create delivery span for distributed tracing.
- Emit `runtime.event.dispatched`.

---

## 15.4 Exit Conditions

Possible transitions:

Dispatched → Delivered

Dispatched → DeliveryTimeout

Dispatched → DispatchFailed

---

## 15.5 Invariants

While in the Dispatched state:

- Event payload SHALL remain immutable.
- Correlation metadata SHALL NOT change.
- Trace context SHALL propagate unchanged.
- Delivery attempts SHALL be counted.

---

# 16. Delivered State

## 16.1 Purpose

The Delivered state confirms that the event has successfully reached the intended subscriber.

Delivery does not necessarily imply successful processing.

---

## 16.2 Entry Criteria

Delivery succeeds when:

- Transport confirms successful transmission.
- Subscriber accepts the message.
- No transport error occurs.

---

## 16.3 Entry Actions

The runtime SHALL:

- Record delivery timestamp.
- Update delivery metrics.
- Emit delivery telemetry.
- Generate audit entry.

---

## 16.4 Exit Conditions

Delivered → Acknowledged

Delivered → DeliveryTimeout (if acknowledgement required)

---

# 17. Acknowledged State

## 17.1 Purpose

Acknowledgement confirms that the subscriber has accepted responsibility for processing the event.

---

## 17.2 Types of Acknowledgement

Supported acknowledgement models:

### Automatic ACK

Generated immediately after delivery.

### Explicit ACK

Returned by subscriber.

### Deferred ACK

Generated after processing completes.

---

## 17.3 Entry Actions

The runtime SHALL:

- Record acknowledgement timestamp.
- Update latency metrics.
- Complete delivery trace.

---

## 17.4 Exit Conditions

Acknowledged → Completed

---

# 18. Completed State

## 18.1 Purpose

Represents successful completion of the event lifecycle.

No further processing SHALL occur.

---

## 18.2 Completion Criteria

Completion requires:

- Delivery completed
- Required acknowledgements received
- Retry policy satisfied
- Audit record finalized

---

## 18.3 Entry Actions

The runtime SHALL:

- Finalize telemetry.
- Close trace span.
- Persist execution statistics.
- Mark lifecycle complete.

---

## 18.4 Terminal State

Completed is a terminal state.

No transition SHALL originate from Completed except archival operations.

---

# 19. Archived State

## 19.1 Purpose

Archived represents long-term retention of historical events.

Archived events SHALL remain immutable.

---

## 19.2 Archival Triggers

Examples:

- Retention policy reached.
- Workflow completed.
- Compliance archive.
- Manual archival.

---

## 19.3 Allowed Operations

Archived events MAY be:

- inspected
- exported
- replayed (subject to policy)

Archived events SHALL NOT be modified.

---

# 20. State Transition Rules

The following transitions are valid.

Created
↓

Validated
↓

Authenticated
↓

Authorized
↓

Enriched
↓

Persisted
↓

Routed
↓

Queued
↓

Dispatched
↓

Delivered
↓

Acknowledged
↓

Completed
↓

Archived

Any transition not explicitly defined by this specification SHALL be considered invalid.

---

# 21. Guard Conditions

Every state transition SHALL satisfy its associated guard conditions.

Examples:

Created → Validated

Guard:

- Event exists.
- Required metadata present.

Validated → Authenticated

Guard:

- Schema validation passed.

Authenticated → Authorized

Guard:

- Publisher identity verified.

Authorized → Enriched

Guard:

- Authorization granted.

Persisted → Routed

Guard:

- Persistence successful.

Queued → Dispatched

Guard:

- Dispatcher available.

Delivered → Acknowledged

Guard:

- ACK required.

Acknowledged → Completed

Guard:

- Processing finalized.

---

# 22. Invalid Transitions

The following transitions SHALL NEVER occur:

Created → Delivered

Validated → Completed

Persisted → Created

Completed → Routed

Archived → Created

Acknowledged → Authenticated

Such attempts SHALL generate:

- Runtime Error
- Audit Event
- Security Alert (optional)

---

# 23. Failure Handling Model

## 23.1 Purpose

The Event Bus SHALL treat failures as explicit lifecycle states rather than exceptional conditions.

Every failure SHALL:

- be classified
- generate telemetry
- generate an audit record
- preserve trace context
- support deterministic recovery

Failures SHALL NEVER silently discard events.

---

## 23.2 Failure Classification

Failures SHALL be classified into one of the following categories:

### Validation Failure

Examples:

- Invalid schema
- Missing required fields
- Invalid payload

Transition:

Validated → ValidationFailed

Recovery:

Publisher correction required.

---

### Authentication Failure

Examples:

- Invalid JWT
- Expired certificate
- Unknown Agent Identity

Transition:

Authenticated → AuthenticationFailed

Recovery:

Re-authentication required.

---

### Authorization Failure

Examples:

- Insufficient permission
- Topic restriction
- Trust level violation

Transition:

Authorized → AuthorizationFailed

Recovery:

Policy update or permission escalation.

---

### Routing Failure

Examples:

- Unknown topic
- Missing routing rule
- Invalid destination

Transition:

Routed → RoutingFailed

Recovery:

Routing recalculation or configuration correction.

---

### Dispatch Failure

Examples:

- Dispatcher unavailable
- Queue unavailable
- Internal runtime error

Transition:

Dispatched → DispatchFailed

Recovery:

Retry according to policy.

---

### Delivery Timeout

Examples:

- Subscriber unavailable
- Network delay
- ACK timeout

Transition:

Delivered → DeliveryTimeout

Recovery:

Retry or Dead Letter Queue.

---

# 24. Retry Lifecycle

## 24.1 Purpose

Retry provides controlled recovery from transient failures.

Retries SHALL be deterministic and policy-driven.

---

## 24.2 Retry State

RetryPending indicates that the event is awaiting another delivery attempt.

Entry Conditions:

- Retry policy permits additional attempts.
- Retry limit not exceeded.

Entry Actions:

- Increment retry counter.
- Calculate next retry time.
- Record retry metadata.
- Emit RetryScheduled event.

---

## 24.3 Retry Strategies

Supported strategies:

### Fixed Interval

Retry after constant delay.

---

### Linear Backoff

Delay increases linearly.

---

### Exponential Backoff

Delay doubles after each attempt.

Recommended default.

---

### Exponential Backoff with Jitter

Preferred for distributed deployments.

Prevents synchronized retry storms.

---

## 24.4 Retry Limits

Each topic SHALL define:

- Maximum retries
- Maximum retry duration
- Retry interval
- Escalation policy

Retry exhaustion SHALL transition to:

RetryExhausted

---

# 25. Retry Exhaustion

## Purpose

Indicates that all retry attempts have failed.

Transition:

RetryPending → RetryExhausted

Actions:

- Generate audit event
- Emit runtime alert
- Evaluate DLQ policy

Exit:

RetryExhausted → DeadLettered

---

# 26. Dead Letter Queue Lifecycle

## 26.1 Purpose

The Dead Letter Queue (DLQ) stores events that cannot be processed successfully.

---

## 26.2 Entry Conditions

Events SHALL enter the DLQ when:

- Retry limit exceeded
- Delivery permanently impossible
- Policy explicitly requires quarantine
- Subscriber repeatedly rejects delivery

---

## 26.3 Entry Actions

The runtime SHALL:

- Preserve original event
- Preserve metadata
- Preserve correlation information
- Preserve trace information
- Record failure reason

---

## 26.4 Allowed Operations

Operators MAY:

- Inspect
- Export
- Replay
- Delete
- Archive
- Quarantine

All operations SHALL be audited.

---

## 26.5 Exit Conditions

DeadLettered → ReplayPending

DeadLettered → Archived

DeadLettered → Deleted (policy permitting)

---

# 27. Replay Lifecycle

## 27.1 Purpose

Replay enables historical events to be processed again without altering historical records.

Replay SHALL create a new execution context while preserving the original event.

---

## 27.2 Replay States

ReplayPending

↓

ReplayValidated

↓

ReplayAuthorized

↓

ReplayQueued

↓

ReplayDispatched

↓

ReplayCompleted

---

## 27.3 Replay Restrictions

Replay SHALL require:

- Appropriate authorization
- Replay policy approval
- Audit recording

Replay SHALL NOT duplicate irreversible side effects unless explicitly authorized.

---

# 28. Cancellation Model

## Purpose

An event MAY be cancelled before successful completion.

Cancellation reasons MAY include:

- Workflow terminated
- Subscriber removed
- Runtime shutdown
- Administrative cancellation

---

## Transition

Any active processing state

↓

Cancelled

Cancelled is a terminal state.

---

# 29. Recovery State Machine

The Runtime SHALL support recovery from:

- Process restart
- Runtime crash
- Network interruption
- Dispatcher failure
- Subscriber restart

Recovery SHALL preserve:

- Event ID
- Correlation ID
- Trace context
- Retry count
- Audit history

Recovery SHALL resume from the last durable lifecycle state.

---

# 30. Failure Escalation

Escalation SHALL follow configurable policies.

Example:

Delivery Failure

↓

Retry

↓

Retry Exhausted

↓

Dead Letter Queue

↓

Operator Notification

↓

Replay or Archive

Critical failures MAY additionally generate:

- Security Event
- Governance Event
- Incident Record

---

# 31. Concurrency Model

## 31.1 Purpose

The Event Bus SHALL support concurrent event processing while preserving deterministic behavior within defined execution boundaries.

Concurrency SHALL improve throughput without compromising correctness, ordering guarantees, or auditability.

---

## 31.2 Concurrency Domains

The Runtime MAY process events concurrently across:

- Different Topics
- Different Workflows
- Different Agents
- Different Partitions

Concurrency SHALL NOT violate ordering guarantees within a single execution domain.

---

## 31.3 Ordering Constraints

Ordering SHALL be preserved for:

- Correlation ID
- Workflow ID
- Aggregate ID (when applicable)
- Partition

Global ordering SHALL NOT be guaranteed.

---

## 31.4 Parallel Dispatch

The Dispatcher MAY deliver events in parallel when:

- Subscribers are independent
- Ordering constraints are satisfied
- Delivery policy permits concurrent execution

Parallel execution SHALL remain transparent to publishers.

---

# 32. Idempotency Requirements

## 32.1 Purpose

Consumers SHALL assume that duplicate delivery is possible.

All critical subscribers SHOULD implement idempotent processing.

---

## 32.2 Idempotency Key

The canonical deduplication key SHALL consist of:

- Event ID
- Correlation ID
- Event Type
- Source Component

Alternative keys MAY be configured for domain-specific processing.

---

## 32.3 Duplicate Detection

The Runtime MAY expose duplicate detection services.

Duplicate detection SHALL NOT mutate the original event.

Duplicate processing decisions SHALL be auditable.

---

# 33. Timeout Management

## 33.1 Timeout Categories

The Runtime SHALL support configurable timeout policies for:

- Validation
- Authentication
- Authorization
- Routing
- Dispatch
- Delivery
- Acknowledgement
- Replay

---

## 33.2 Timeout Behavior

When a timeout occurs:

1. Record timeout reason.
2. Generate audit record.
3. Emit telemetry.
4. Evaluate retry policy.
5. Transition to the appropriate failure state.

Timeout handling SHALL be deterministic.

---

## 33.3 Timeout Escalation

Repeated timeout failures MAY trigger:

- Retry
- Dead Letter Queue
- Operator Notification
- Governance Alert
- Incident Record

Escalation thresholds SHALL be configurable.

---

# 34. Audit Requirements

## 34.1 Mandatory Auditing

Every lifecycle transition SHALL generate an immutable audit record.

The audit record SHALL include:

- Event ID
- Previous State
- Current State
- Timestamp
- Publisher
- Subscriber
- Correlation ID
- Trace ID
- Runtime Instance
- Result

---

## 34.2 Audit Integrity

Audit records SHALL be:

- Immutable
- Chronologically ordered
- Tamper-evident
- Exportable
- Searchable

---

## 34.3 Audit Retention

Retention SHALL follow Governance Policy.

Security-related lifecycle records MAY require extended retention periods.

---

# 35. Observability Requirements

## 35.1 Metrics

The Runtime SHALL expose metrics for:

- Events Published
- Events Routed
- Events Delivered
- Delivery Latency
- Retry Count
- Replay Count
- DLQ Count
- Validation Failures
- Authentication Failures
- Authorization Failures
- Timeout Count

---

## 35.2 Structured Logging

Each lifecycle transition SHALL produce structured log entries.

Required fields:

- Event ID
- Lifecycle State
- Timestamp
- Correlation ID
- Trace ID
- Component
- Result

---

## 35.3 Distributed Tracing

Every lifecycle transition SHALL extend the distributed trace.

The Runtime SHALL preserve:

- Trace ID
- Span ID
- Parent Span ID

Trace continuity SHALL survive retries and replay operations.

---

# 36. Canonical State Transition Matrix

| Current State | Allowed Next States |
|---------------|---------------------|
| Created | Validated |
| Validated | Authenticated, ValidationFailed |
| Authenticated | Authorized, AuthenticationFailed |
| Authorized | Enriched, AuthorizationFailed |
| Enriched | Persisted |
| Persisted | Routed |
| Routed | Queued, RoutingFailed |
| Queued | Dispatched |
| Dispatched | Delivered, DispatchFailed |
| Delivered | Acknowledged, DeliveryTimeout |
| Acknowledged | Completed |
| Completed | Archived |
| RetryPending | Dispatched, RetryExhausted |
| RetryExhausted | DeadLettered |
| DeadLettered | ReplayPending, Archived |
| ReplayPending | ReplayValidated |
| ReplayValidated | ReplayAuthorized |
| ReplayAuthorized | ReplayQueued |
| ReplayQueued | ReplayDispatched |
| ReplayDispatched | ReplayCompleted |
| Cancelled | Terminal |
| Archived | Terminal |

Any transition not listed above SHALL be considered invalid.

---

# 37. Conformance Requirements

An implementation SHALL be considered compliant only if it satisfies all mandatory lifecycle requirements.

Mandatory requirements include:

- Deterministic state transitions
- Immutable events
- Immutable audit records
- Lifecycle validation
- Failure state support
- Retry support
- Replay support
- Dead Letter Queue support
- Distributed tracing
- Lifecycle metrics

Non-conforming implementations SHALL explicitly declare unsupported capabilities.

---

# 38. References

Normative References

- runtime/architecture/EVENT_BUS.md
- runtime/contracts/event_contract.json
- runtime/pseudocode/dispatcher.md
- specifications/ERROR_CODES.md
- specifications/VERSIONING.md
- specifications/ACP_JSON_SCHEMA.md

Informative References

- CloudEvents 1.0
- OpenTelemetry Specification
- Enterprise Integration Patterns
- ISO/IEC 27001
- ISO 31000

---

# 39. Revision History

| Version | Status | Description |
|---------|--------|-------------|
| 0.1.0 | Draft | Initial lifecycle model |
| 0.7.0 | Review | Failure & replay model added |
| 1.0.0 | Production Draft | Canonical Event Lifecycle Specification |

---

# Appendix A — Lifecycle Summary

Canonical Success Path

Created

↓

Validated

↓

Authenticated

↓

Authorized

↓

Enriched

↓

Persisted

↓

Routed

↓

Queued

↓

Dispatched

↓

Delivered

↓

Acknowledged

↓

Completed

↓

Archived

Canonical Failure Path

ValidationFailed

AuthenticationFailed

AuthorizationFailed

RoutingFailed

DispatchFailed

DeliveryTimeout

↓

RetryPending

↓

RetryExhausted

↓

DeadLettered

↓

ReplayPending (optional)

OR

Archived

The Event Lifecycle defined in this specification is normative and SHALL govern every event processed by the AITOS Runtime Event Bus.