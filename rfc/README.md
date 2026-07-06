# Request for Comments (RFC)

Version: 1.0

Status: Active

---

# Purpose

The Request for Comments (RFC) framework provides a structured process for proposing, reviewing, and approving significant changes before implementation.

RFCs encourage collaborative discussion, reduce architectural drift, and improve long-term engineering quality.

Unlike ADRs, RFCs document **proposals**, not final decisions.

---

# Objectives

- Encourage design discussion
- Improve proposal quality
- Capture stakeholder feedback
- Reduce implementation risk
- Preserve proposal history
- Support AI-assisted planning

---

# When an RFC is Required

An RFC should be created before changes involving:

- System architecture
- Repository structure
- New subsystems
- Public APIs
- Technology stack changes
- Security model
- Deployment strategy
- Major research methodology
- Governance changes
- Breaking changes

Minor bug fixes and documentation updates do not require an RFC.

---

# RFC Lifecycle

Idea

↓

Draft

↓

Discussion

↓

Under Review

↓

Approved

↓

Implemented

↓

Closed

or

Rejected

---

# Naming Convention

RFC-XXXX-SHORT_TITLE.md

Examples:

RFC-0001-CORE_PLATFORM.md

RFC-0002-KNOWLEDGE_GRAPH.md

RFC-0003-AI_ROUTER.md

---

# Relationship with ADR

Idea

↓

RFC

↓

Discussion

↓

Approval

↓

ADR

↓

Implementation

RFC explores possibilities.

ADR records the final decision.

---

# Guiding Principle

Discuss first.

Implement second.