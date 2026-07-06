# Dispatcher Algorithm Specification

Document ID: RT-PSEUDO-DISPATCHER-001

Version: 1.0.0

Status: Production Draft

Owner: Runtime Architecture Working Group

Component: EVENT_BUS

Related Documents

- EVENT_BUS.md
- event_lifecycle.md
- event_dispatch.md
- event_contract.json
- event_bus.py

---

# 1. Purpose

This document defines the canonical algorithms governing the Runtime Dispatcher.

The Dispatcher is responsible for transforming a routed event into one or more successful deliveries while enforcing reliability, security, ordering, and observability requirements.

This document is language-independent and SHALL serve as the reference implementation model.

---

# 2. Responsibilities

The Dispatcher SHALL:

- acquire queued events
- verify dispatch readiness
- resolve subscribers
- execute delivery
- collect acknowledgements
- invoke retry policy
- update lifecycle state
- emit telemetry
- generate audit records

The Dispatcher SHALL NOT:

- modify event payload
- execute business logic
- bypass authorization
- bypass lifecycle rules

---

# 3. Core Dispatch Pipeline

```
Queue
    ↓
Acquire Event
    ↓
Verify Dispatch Conditions
    ↓
Resolve Subscribers
    ↓
Create Dispatch Plan
    ↓
Execute Delivery
    ↓
Collect ACK/NACK
    ↓
Evaluate Retry Policy
    ↓
Finalize Lifecycle
```

---

# 4. Main Dispatch Algorithm

```text
function dispatch(event):

    validateLifecycle(event)

    verifyDispatchEligibility(event)

    subscribers = resolveSubscribers(event)

    if subscribers.isEmpty():

        return routingFailure(event)

    plan = createDispatchPlan(event, subscribers)

    result = executeDispatchPlan(plan)

    finalizeDispatch(event, result)

    return result
```

---

# 5. Dispatch Eligibility

```text
function verifyDispatchEligibility(event):

    assert event.state == QUEUED

    assert event.schemaValid

    assert event.authenticationPassed

    assert event.authorizationPassed

    assert event.persisted

    assert event.notExpired

    return true
```

Events failing eligibility SHALL transition to the appropriate lifecycle failure state.

---

# 6. Subscriber Resolution

```text
function resolveSubscribers(event):

    subscribers = subscriptionManager.lookup(
        topic = event.topic,
        labels = event.labels,
        trust = event.trustLevel
    )

    return subscribers
```

Resolution SHALL be deterministic.

---

# 7. Dispatch Plan Generation

The Dispatcher SHALL construct an immutable Dispatch Plan.

Dispatch Plan fields include:

- Event ID
- Subscriber List
- Delivery Policy
- Ordering Policy
- Retry Policy
- Timeout Policy
- Trace Context

The Dispatch Plan SHALL remain immutable after creation.

---

# 8. Delivery Execution

```text
function executeDispatchPlan(plan):

    for subscriber in plan.subscribers:

        deliver(plan.event, subscriber)

    return collectResults()
```

When parallel delivery is permitted, subscribers MAY be processed concurrently while preserving ordering guarantees.

---

# 9. Acknowledgement Collection

```text
function collectResults():

    results = []

    waitForAcknowledgements()

    aggregate(results)

    return results
```

Supported acknowledgement outcomes:

- ACK
- NACK
- TIMEOUT

---

# 10. Dispatch Finalization

```text
function finalizeDispatch(event, result):

    updateLifecycle(event)

    emitMetrics(event)

    writeAudit(event)

    closeTrace(event)
```

Dispatch finalization SHALL occur regardless of success or failure.

---

# 11. Retry Orchestration Algorithm

## 11.1 Purpose

The Dispatcher SHALL automatically recover from transient delivery failures according to the configured Retry Policy.

Retry execution SHALL be deterministic, auditable, and bounded.

---

## 11.2 Retry Decision Algorithm

```text
function evaluateRetry(event, failure):

    policy = event.retryPolicy

    if failure.isPermanent():
        return DEAD_LETTER

    if event.retryCount >= policy.maxRetries:
        return RETRY_EXHAUSTED

    if event.age() >= policy.maxRetryDuration:
        return RETRY_EXHAUSTED

    return RETRY
```

---

## 11.3 Retry Scheduling

```text
function scheduleRetry(event):

    delay = calculateBackoff(
        strategy = event.retryPolicy.strategy,
        retryCount = event.retryCount
    )

    enqueueRetry(event, delay)
```

Supported strategies:

- Fixed
- Linear
- Exponential
- Exponential + Jitter

---

# 12. Replay Algorithm

## 12.1 Purpose

Replay SHALL create a new execution while preserving the original immutable event.

Replay SHALL NEVER modify historical records.

---

## 12.2 Replay Workflow

```text
function replay(event):

    authorizeReplay(event)

    validateReplayPolicy(event)

    replayContext = createReplayContext(event)

    enqueueReplay(replayContext)
```

---

## 12.3 Replay Context

Replay Context SHALL include:

- Original Event ID
- Replay ID
- Replay Timestamp
- Replay Requestor
- Replay Reason
- Trace Context
- Replay Policy Version

---

## 12.4 Replay Completion

```text
function completeReplay(context):

    updateMetrics()

    writeReplayAudit()

    closeReplayTrace()
```

---

# 13. Dead Letter Queue Algorithm

## 13.1 Purpose

The Dead Letter Queue (DLQ) SHALL isolate permanently failed events for inspection and recovery.

---

## 13.2 DLQ Decision

```text
function evaluateDeadLetter(event):

    if retryExhausted(event):
        moveToDLQ(event)

    if permanentFailure(event):
        moveToDLQ(event)
```

---

## 13.3 Move To DLQ

```text
function moveToDLQ(event):

    preserveEvent()

    preserveMetadata()

    preserveFailureHistory()

    preserveRetryHistory()

    storeDLQRecord()
```

---

## 13.4 Operator Actions

Supported operations:

- Inspect
- Replay
- Archive
- Export
- Delete (policy controlled)

Every operation SHALL generate an audit record.

---

# 14. Queue Scheduling Algorithm

## 14.1 Purpose

Queue scheduling determines which event is dispatched next.

Scheduling SHALL balance fairness, priority, and throughput.

---

## 14.2 Scheduling Algorithm

```text
function selectNextEvent(queue):

    candidates = queue.readyEvents()

    ordered = applySchedulingPolicy(candidates)

    return ordered.first()
```

---

## 14.3 Scheduling Policies

Supported policies:

- FIFO
- Priority
- Weighted Priority
- Fair Scheduling
- Deadline First
- Partition Ordered

Policy selection SHALL be configurable.

---

# 15. Priority Handling

Priority levels MAY include:

- Critical
- High
- Normal
- Low
- Background

Priority SHALL influence scheduling only.

Priority SHALL NOT bypass:

- Authentication
- Authorization
- Validation

---

## Priority Comparison

```text
Critical
↓

High
↓

Normal
↓

Low
↓

Background
```

---

# 16. Timeout Handling

## 16.1 Timeout Detection

```text
function monitorTimeout(dispatch):

    if currentTime() > dispatch.deadline:

        return TIMEOUT
```

---

## 16.2 Timeout Processing

```text
function handleTimeout(event):

    recordTimeout()

    emitMetric()

    evaluateRetry()

    updateLifecycle()
```

---

## 16.3 Timeout Escalation

Repeated timeout failures MAY result in:

- Retry
- Replay Recommendation
- Dead Letter Queue
- Operator Alert
- Incident Record

Escalation SHALL follow Runtime Policy.

---

# 17. Failure Recovery Algorithm

## 17.1 Purpose

Failure recovery restores interrupted dispatch operations without violating lifecycle guarantees.

---

## 17.2 Recovery Workflow

```text
function recoverDispatch():

    restoreQueues()

    restoreSubscriptions()

    restorePendingEvents()

    restoreRetryQueue()

    resumeDispatch()
```

---

## 17.3 Recovery Guarantees

Recovery SHALL preserve:

- Event ID
- Correlation ID
- Trace ID
- Retry Count
- Delivery State
- Audit History

Recovery SHALL resume from the last durable state.

---

# 18. Parallel Dispatch Algorithm

## 18.1 Purpose

The Dispatcher SHALL support parallel event delivery when permitted by the configured Delivery Policy.

Parallel execution SHALL improve throughput without violating ordering guarantees, lifecycle integrity, or audit requirements.

---

## 18.2 Eligibility

Parallel dispatch MAY be enabled when:

- Subscriber execution is independent.
- Ordering constraints are satisfied.
- Delivery policy permits concurrent execution.
- Required runtime resources are available.

Parallel dispatch SHALL NOT be used for strictly ordered delivery groups.

---

## 18.3 Parallel Dispatch Algorithm

```text
function dispatchParallel(event, subscribers):

    tasks = []

    for subscriber in subscribers:

        task = spawn(
            deliver(event, subscriber)
        )

        tasks.add(task)

    results = waitForCompletion(tasks)

    return aggregate(results)
```

---

## 18.4 Result Aggregation

```text
function aggregate(results):

    summary = new DispatchSummary()

    for result in results:

        summary.record(result)

    return summary
```

The Dispatcher SHALL preserve individual subscriber outcomes while generating an overall dispatch result.

---

# 19. Broadcast Dispatch Algorithm

## 19.1 Purpose

Broadcast delivery distributes one immutable event to all matching subscribers.

Each subscriber SHALL receive an independent execution context.

---

## 19.2 Broadcast Workflow

```text
function broadcast(event):

    subscribers = resolveSubscribers(event)

    for subscriber in subscribers:

        dispatch(event, subscriber)
```

---

## 19.3 Broadcast Guarantees

The Dispatcher SHALL preserve:

- immutable payload
- Event ID
- Correlation ID
- Trace context
- delivery independence

Failure of one subscriber SHALL NOT cancel successful deliveries to others unless explicitly required by policy.

---

# 20. Context Synchronization Algorithm

## 20.1 Purpose

Context synchronization propagates updates from the Context Engine to subscribed runtime components.

---

## 20.2 Synchronization Workflow

```text
function synchronizeContext(update):

    validate(update)

    subscribers = resolveContextSubscribers(update)

    dispatchParallel(update, subscribers)
```

---

## 20.3 Synchronization Guarantees

The Dispatcher SHALL preserve:

- Context ID
- Context Version
- Timestamp
- Correlation ID
- Source Agent

Context synchronization SHALL be deterministic.

---

# 21. Memory Synchronization Algorithm

## 21.1 Purpose

Memory synchronization propagates changes from the Memory Engine across the Runtime.

---

## 21.2 Memory Synchronization Workflow

```text
function synchronizeMemory(memoryEvent):

    validate(memoryEvent)

    subscribers = resolveMemorySubscribers(memoryEvent)

    dispatchParallel(memoryEvent, subscribers)
```

---

## 21.3 Synchronization Requirements

The Dispatcher SHALL preserve:

- Memory ID
- Version
- Trust Level
- Integrity Metadata

Memory synchronization SHALL generate audit records.

---

# 22. Metrics Collection Algorithm

## 22.1 Purpose

Every dispatch operation SHALL generate runtime metrics.

---

## 22.2 Metrics Workflow

```text
function collectMetrics(dispatch):

    increment(dispatchCount)

    observe(dispatchLatency)

    observe(queueDepth)

    record(result)
```

---

## 22.3 Mandatory Metrics

The Dispatcher SHALL expose:

- Dispatch Count
- Delivery Success
- Delivery Failure
- Retry Count
- Replay Count
- Queue Depth
- Processing Time
- ACK Latency
- DLQ Count

Metrics SHALL be suitable for Prometheus/OpenTelemetry export.

---

# 23. Audit Generation Algorithm

## 23.1 Purpose

Every significant dispatcher action SHALL generate an immutable audit record.

---

## 23.2 Audit Workflow

```text
function writeAudit(event, action):

    record = buildAuditRecord(
        event,
        action
    )

    persist(record)
```

---

## 23.3 Required Fields

Each audit record SHALL include:

- Event ID
- Action
- Previous State
- Current State
- Timestamp
- Publisher
- Subscriber
- Trace ID
- Correlation ID
- Runtime Instance

Audit records SHALL be immutable.

---

# 24. Distributed Tracing Algorithm

## 24.1 Purpose

Distributed tracing enables end-to-end visibility across the dispatch pipeline.

---

## 24.2 Trace Workflow

```text
function beginDispatchTrace(event):

    span = tracer.startSpan(
        event.traceId
    )

    return span
```

---

```text
function endDispatchTrace(span):

    span.finish()
```

---

## 24.3 Trace Propagation

The Dispatcher SHALL propagate:

- Trace ID
- Span ID
- Parent Span ID
- Correlation ID

Trace continuity SHALL survive:

- Retry
- Replay
- Broadcast
- Parallel Dispatch

---

# 25. Concurrency Control

## 25.1 Purpose

Concurrency control prevents race conditions while maximizing throughput.

---

## 25.2 Concurrency Model

The Dispatcher MAY use:

- Thread Pool
- Async Runtime
- Actor Model
- Event Loop
- Worker Pool

Implementation details are language-specific.

---

## 25.3 Concurrency Guarantees

The Dispatcher SHALL guarantee:

- event immutability
- lifecycle consistency
- deterministic routing
- isolated subscriber execution
- thread-safe metrics
- thread-safe audit generation

Shared mutable state SHOULD be avoided.

---

# 26. Error Handling Algorithm

## 26.1 Purpose

The Dispatcher SHALL process every runtime error through a deterministic error handling pipeline.

Errors SHALL never bypass:

- Audit
- Metrics
- Lifecycle updates
- Trace propagation

---

## 26.2 Canonical Error Pipeline

```text
Error Detected
        ↓
Classify Error
        ↓
Assign Error Code
        ↓
Update Lifecycle
        ↓
Generate Audit Record
        ↓
Emit Metrics
        ↓
Evaluate Retry Policy
        ↓
Return Result
```

---

## 26.3 Error Handling Algorithm

```text
function handleError(event, error):

    classification = classify(error)

    code = assignErrorCode(classification)

    updateLifecycle(event, classification)

    writeAudit(event, code)

    emitMetrics(code)

    if retryAllowed(event, classification):

        scheduleRetry(event)

    else:

        moveToDLQ(event)

    return buildErrorResult(code)
```

---

# 27. Health Check Algorithm

## 27.1 Purpose

The Dispatcher SHALL continuously expose operational health.

---

## 27.2 Health Indicators

Mandatory indicators:

- Queue availability
- Dispatcher status
- Worker utilization
- Retry queue health
- Dead Letter Queue health
- Average dispatch latency
- Error rate
- Success rate

---

## 27.3 Health Evaluation

```text
function evaluateHealth():

    score = calculateHealthScore()

    status = classify(score)

    publishHealth(status)
```

---

## 27.4 Health States

Supported health states:

- Healthy
- Degraded
- Warning
- Critical
- Unavailable

Health transitions SHALL generate telemetry events.

---

# 28. Extension Points

The Dispatcher SHALL support controlled extensibility.

Supported extension points include:

- Routing Strategy
- Queue Scheduler
- Retry Policy
- Replay Policy
- Audit Sink
- Metrics Exporter
- Trace Provider
- Authorization Provider
- Delivery Adapter

Extensions SHALL NOT violate lifecycle invariants.

---

## Plugin Contract

Every extension SHALL implement:

```text
initialize()

validate()

execute()

shutdown()
```

Extensions SHALL be versioned and auditable.

---

# 29. Performance Optimization

## 29.1 Objectives

The Dispatcher SHOULD optimize for:

- Low latency
- High throughput
- Predictable execution
- Resource efficiency

---

## 29.2 Recommended Optimizations

Implementations MAY use:

- Worker pools
- Async execution
- Batch acknowledgements
- Adaptive queue scheduling
- Connection pooling
- Zero-copy payload handling
- Efficient serialization
- Lazy metadata evaluation

All optimizations SHALL preserve observable behavior.

---

## 29.3 Performance Targets

Example production objectives:

- Dispatch latency < 10 ms (internal)
- ACK processing < 50 ms
- Queue scheduling O(log n) or better
- Retry scheduling O(1)

Actual targets SHALL be deployment-specific.

---

# 30. Configuration Model

## 30.1 Runtime Configuration

The Dispatcher SHALL support configurable parameters.

Examples:

```yaml
dispatcher:
  workers: 16
  queue_strategy: priority
  retry_strategy: exponential
  max_retries: 5
  dispatch_timeout_ms: 5000
  ack_timeout_ms: 3000
  tracing: enabled
  metrics: enabled
  audit: enabled
```

---

## 30.2 Configuration Validation

Configuration SHALL be validated during startup.

Invalid configuration SHALL prevent Dispatcher initialization.

---

# 31. Conformance Requirements

An implementation SHALL be considered conformant only if it satisfies all mandatory algorithms defined in this specification.

Mandatory capabilities include:

- Dispatch execution
- Subscriber resolution
- Queue scheduling
- Retry orchestration
- Replay support
- Dead Letter Queue support
- Timeout handling
- Audit generation
- Metrics generation
- Distributed tracing
- Lifecycle synchronization

Optional capabilities SHALL be explicitly documented.

---

# 32. References

## Normative

- runtime/architecture/EVENT_BUS.md
- runtime/state-machines/event_lifecycle.md
- runtime/sequences/event_dispatch.md
- runtime/contracts/event_contract.json
- specifications/ERROR_CODES.md
- specifications/VERSIONING.md

## Informative

- CloudEvents 1.0
- OpenTelemetry Specification
- AsyncAPI Specification
- Enterprise Integration Patterns
- ISO 31000
- ISO/IEC 27001

---

# 33. Revision History

| Version | Status | Description |
|---------|--------|-------------|
| 0.1.0 | Draft | Initial dispatcher algorithms |
| 0.7.0 | Review | Retry, replay and concurrency added |
| 1.0.0 | Production Draft | Canonical dispatcher specification |

---

# Appendix A — Complete Dispatcher Flow

```text
Acquire Event
      ↓
Validate Lifecycle
      ↓
Verify Eligibility
      ↓
Resolve Subscribers
      ↓
Generate Dispatch Plan
      ↓
Select Queue
      ↓
Dispatch
      ↓
Collect ACK/NACK
      ↓
Retry Evaluation
      ↓
Replay (if requested)
      ↓
Dead Letter Queue (if required)
      ↓
Audit
      ↓
Metrics
      ↓
Trace Completion
      ↓
Lifecycle Complete
```

---

# Appendix B — Dispatcher Guarantees

The Dispatcher SHALL guarantee:

- Deterministic routing
- Immutable events
- Ordered lifecycle transitions
- Thread-safe execution
- Auditable decisions
- Observable operations
- Configurable retry behavior
- Replay capability
- Dead Letter Queue integration
- End-to-end trace propagation

---

# Document Status

Status: **Production Draft**

Maturity Level: **L2 (Algorithm Complete)**

Implementation Status

- Core Dispatch Algorithm: Complete
- Queue Scheduling: Complete
- Retry Orchestration: Complete
- Replay Processing: Complete
- Dead Letter Queue: Complete
- Timeout Handling: Complete
- Parallel Dispatch: Complete
- Broadcast Dispatch: Complete
- Context Synchronization: Complete
- Memory Synchronization: Complete
- Metrics & Audit: Complete
- Distributed Tracing: Complete
- Health Monitoring: Complete
- Extension Model: Complete
- Conformance Rules: Complete

This document is the canonical algorithmic specification for all Dispatcher implementations within the AITOS Runtime.