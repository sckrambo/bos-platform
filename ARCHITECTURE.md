# BOS Platform Architecture

**Status:** Foundation Draft  
**Version:** 0.1.0  
**Last Updated:** 2026-07-21

## 1. Purpose

This document defines the high-level software architecture of the BOS Platform.

It explains:

- The major parts of the system
- The responsibility of each package
- Permitted dependency directions
- Where domain rules belong
- How applications interact with the domain
- How persistence, artificial intelligence, and external systems may integrate
- Which architectural boundaries must remain stable

This document governs implementation structure.

The BOS standards govern judgment behavior.

When architecture and implementation convenience conflict with the BOS standards, the standards take precedence.

## 2. Architectural Goals

The BOS Platform should be:

- Faithful to the BOS standards
- Explainable
- Testable
- Modular
- Extensible
- Technology-conscious but technology-independent at the domain level
- Safe for human and AI collaboration
- Capable of preserving provenance and historical integrity
- Suitable for incremental development
- Simple enough to operate as a modular monolith during the initial product stages

## 3. Architectural Principles

### 3.1 Domain Before Interface

The judgment domain is the center of the platform.

User interfaces, databases, APIs, and AI systems exist to interact with the domain.

They must not define the domain.

### 3.2 Standards Before Implementation

Normative behavior originates in:

- `docs/standards/BOS-000.md`
- `docs/standards/BOS-100.md`
- Future documents in `docs/standards`

Application code must implement these standards rather than silently reinterpret them.

### 3.3 Dependency Direction Matters

Dependencies must point inward toward the domain.

Preferred direction:

```text
Applications
    ↓
Application Services
    ↓
Domain
```
Infrastructure may implement interfaces defined by the domain or application layer.

The domain must not depend on:

-React
-Next.js
-Database clients
-HTTP frameworks
-AI providers
-Cloud vendors
-User-interface components
3.4 Preserve Provenance and History

Provenance and historical integrity are domain concerns, not optional audit features.

They must be represented in the core model and validated independently from the user interface.

3.5 AI Is an Adapter and Participant

Artificial intelligence may participate in BOS, but no model provider belongs inside the core domain.

The domain recognizes an AI Participant.

Provider-specific integration belongs in an adapter or infrastructure module.

3.6 Prefer a Modular Monolith

The initial BOS implementation should be a modular monolith.

Modules may have strong boundaries without requiring separate services.

Distributed systems should be introduced only after a documented need exists.

3.7 Explicit Over Implicit

Important judgment relationships must be represented explicitly.

The system should not depend on hidden inference for:

Decision lineage
Provenance
Participant attribution
Historical revision
AI involvement
Conversation attachment
Dissent
Confidence
Uncertainty
4. Repository Structure

The intended top-level repository structure is:

bos-platform/
├── apps/
│   └── web/
├── docs/
│   ├── architecture/
│   ├── decisions/
│   ├── design/
│   ├── product/
│   └── standards/
├── packages/
│   ├── application/
│   ├── conformance/
│   ├── domain/
│   ├── infrastructure/
│   └── ui/
├── tests/
├── AGENTS.md
├── ARCHITECTURE.md
├── README.md
└── VISION.md

Some directories may not exist until their first implementation is created.

5. Architectural Layers

The BOS Platform is organized into five primary layers.

Presentation
    ↓
Application
    ↓
Domain
    ↑
Infrastructure

Conformance validates behavior across the architecture.

6. Domain Layer

Location:

packages/domain

The domain layer is the most important implementation package in BOS.

It contains the software representation of the judgment architecture.

6.1 Responsibilities

The domain layer owns:

Judgment Objects
Thinking Blocks
Participants
Relationships
Conversations
Decisions
Outcomes
Reflections
Lessons
Provenance
Historical events
Decision lineage
Lifecycle rules
Domain validation
Domain errors
Domain events
Value objects
Core state transitions
6.2 Domain Rules

Domain rules must be enforceable without:

A web browser
A database
An API
An AI model
A graphical interface

Examples include:

A Decision must have lineage
A Conversation must have an attachment
A Reflection cannot alter an earlier Decision
A Thinking Block must retain its type
Every meaningful contribution must identify its Participant
AI-generated contributions must remain distinguishable
Historical changes must remain replayable
6.3 Domain Package Constraints

packages/domain must not import from:

apps/web
packages/ui
packages/infrastructure
Next.js
React
Database libraries
HTTP libraries
AI SDKs

The domain may depend only on:

The TypeScript standard library
Small, implementation-neutral utility libraries when justified
Other internal domain modules
6.4 Initial Domain Modules

The initial domain package may include:

packages/domain/
├── src/
│   ├── judgment/
│   ├── thinking-block/
│   ├── participant/
│   ├── relationship/
│   ├── conversation/
│   ├── decision/
│   ├── outcome/
│   ├── reflection/
│   ├── provenance/
│   ├── history/
│   ├── validation/
│   └── index.ts
└── tests/

The final organization may evolve through documented architecture decisions.

7. Application Layer

Location:

packages/application

The application layer coordinates use cases.

It tells domain objects what operation is being attempted but does not replace their rules.

7.1 Responsibilities

The application layer may own:

Commands
Queries
Use cases
Application services
Transaction boundaries
Authorization coordination
Repository interfaces
Workflow orchestration
Domain event coordination
DTO mapping
Application-level validation
AI orchestration interfaces

Examples:

Create a Judgment Object
Add an Observation
Link Evidence to an Assumption
Record a Decision
Add an Outcome
Record a Reflection
Replay a Judgment Object
Generate a Judgment Brief
7.2 Application Constraints

The application layer may depend on the domain.

It must not contain core domain rules that belong in packages/domain.

It should depend on interfaces for infrastructure behavior, including:

Persistence
Time
Identity generation
AI services
External systems
Notifications
8. Infrastructure Layer

Location:

packages/infrastructure

The infrastructure layer connects BOS to implementation technologies.

8.1 Responsibilities

Infrastructure may include:

PostgreSQL persistence
ORM configuration
Repository implementations
Authentication providers
File storage
AI provider adapters
Email or notification adapters
Logging
Telemetry
External-system integrations
Search infrastructure
Background jobs
8.2 Infrastructure Constraints

Infrastructure may depend on application and domain interfaces.

The domain must not depend on infrastructure implementations.

Provider-specific details must remain outside the domain.

Examples:

OpenAIJudgmentAssistant
PostgresJudgmentRepository
ClerkIdentityProvider
S3EvidenceStorage

These are adapters, not domain concepts.

9. Presentation Layer

Locations:

apps/web
packages/ui

The presentation layer provides the user experience.

9.1 apps/web

apps/web is the primary BOS reference application.

It may contain:

Next.js routes
Server actions
API endpoints
Page composition
Authentication integration
Application-service composition
Web-specific configuration
9.2 packages/ui

packages/ui contains reusable interface components.

Examples:

Judgment Card
Observation Card
Evidence Card
Assumption Badge
Alternative Card
Risk Card
Lesson Card
Challenge Card
Confidence Meter
Lifecycle Ribbon
Reflection Panel
Relationship Thread
Judgment Canvas components
9.3 Presentation Constraints

The presentation layer may display and initiate domain operations.

It must not become the sole enforcer of domain rules.

A user-interface validation may improve usability, but the domain must still reject invalid operations independently.

10. Conformance Layer

Location:

packages/conformance

The conformance package validates implementation behavior against BOS standards.

10.1 Responsibilities

It may include:

Requirement identifiers
Conformance test suites
Shared behavioral fixtures
Replay validation
Decision-lineage validation
Provenance validation
AI-attribution validation
Historical-integrity validation
Certification reporting
10.2 Requirement Traceability

Tests should reference stable requirement identifiers when those identifiers exist.

Example:

Validates: REQ-B100-DECISION-LINEAGE-001

The intended traceability model is:

Standard
    ↓
Requirement
    ↓
Implementation
    ↓
Automated Test
    ↓
Conformance Result
11. Data Architecture

The initial production data store is expected to be PostgreSQL.

PostgreSQL is an infrastructure choice, not a domain dependency.

11.1 Persistence Principles

Persistence must support:

Stable object identity
Participant attribution
Explicit relationships
Append-oriented history
Version or event tracking
Decision lineage
Multiple Outcomes
Multiple Reflections
Replay
AI contribution attribution
11.2 Persistence Model

The first implementation may use a relational model with explicit tables for primary objects and relationships.

The architecture should not begin with a graph database unless implementation evidence demonstrates that one is necessary.

Graph behavior may be produced through relational relationships and query projections.

11.3 Deletion

Meaningful judgment records should not be physically deleted through ordinary user operations.

The platform may support:

Archival
Redaction
Supersession
Correction
Access restriction
Retention-policy enforcement

Any destructive operation must preserve required historical and legal controls.

12. Event and History Architecture

BOS requires replayable historical state.

The initial implementation does not require full event sourcing.

A practical starting model may combine:

Current-state records
Append-only domain events
Version history
Provenance records

This approach must still support understanding what existed at a specific point in time.

A future move to fuller event sourcing requires an Architecture Decision Record.

13. Artificial Intelligence Architecture

AI integrations belong outside the domain core.

The domain represents:

AI as a Participant type
AI attribution
AI-generated contributions
Confidence
Contribution classification
Delegated authority when explicitly configured

The infrastructure layer implements specific AI providers.

The application layer coordinates AI use cases.

13.1 AI Contribution Types

AI output should distinguish among:

Fact
Inference
Speculation
Recommendation
Confidence
13.2 AI Constraints

AI must not:

Silently author a human contribution
Silently alter a Decision
Remove provenance
Hide uncertainty
Rewrite history
Exercise authority beyond an explicit delegation
13.3 Multi-Agent Design

Multiple AI capabilities may participate through defined roles, such as:

Researcher
Challenger
Validator
Summarizer
Reflection Coach
Judgment Director

These roles are application capabilities, not separate authorities over the domain.

14. Security and Authorization

Authorization must be enforced at the application boundary and supported by infrastructure.

Authorization must not erase provenance.

The platform should eventually support permissions at multiple levels:

Organization
Workspace
Judgment Object
Thinking Block
Conversation
Participant role
Operation type

Sensitive content may require:

Field-level access control
Redaction
Restricted evidence
Private dissent
Audit access
Retention controls

Detailed authorization rules require a separate standard and architecture decision.

15. API Architecture

Initial APIs should expose application use cases rather than raw database operations.

Preferred examples:

createJudgmentObject
addThinkingBlock
linkObjects
recordDecision
recordOutcome
recordReflection
replayJudgment
generateJudgmentBrief

Avoid generic operations that bypass domain meaning, such as unrestricted record mutation.

The API must not permit clients to silently overwrite historical judgment state.

16. Testing Strategy

BOS uses multiple levels of testing.

16.1 Domain Tests

Validate core rules without infrastructure.

Examples:

Decision lineage requirements
Conversation attachment
Participant attribution
Reflection integrity
Provenance preservation
Historical revision behavior
16.2 Application Tests

Validate use-case coordination.

Examples:

Create a valid Judgment Object
Reject unauthorized Decision recording
Persist a Reflection without altering the Decision
Replay a Judgment history
16.3 Infrastructure Tests

Validate adapters.

Examples:

PostgreSQL repositories preserve history
AI adapters label contributions correctly
Authentication maps users to Participants
16.4 End-to-End Tests

Validate complete user workflows.

Playwright is the preferred initial browser-testing tool.

16.5 Conformance Tests

Validate BOS implementation behavior against normative standards.

17. Technology Baseline

The initial preferred technology baseline is:

TypeScript
Node.js
Next.js
React
PostgreSQL
Drizzle or Prisma
Vitest
Playwright

This baseline is a starting point rather than a constitutional requirement.

Changes must be documented when they materially affect architecture.

18. Dependency Rules

Permitted dependencies:

apps/web
  → packages/application
  → packages/domain

apps/web
  → packages/ui

packages/infrastructure
  → packages/application
  → packages/domain

packages/conformance
  → packages/domain
  → packages/application

Prohibited dependencies:

packages/domain
  ✗→ apps/web

packages/domain
  ✗→ packages/ui

packages/domain
  ✗→ packages/infrastructure

packages/domain
  ✗→ Next.js

packages/domain
  ✗→ React

packages/domain
  ✗→ database clients

packages/domain
  ✗→ AI provider SDKs

Circular package dependencies are prohibited.

19. Architecture Decision Records

Material architectural changes must be documented in:

docs/decisions

Each Architecture Decision Record should include:

Title
Status
Context
Decision
Alternatives considered
Consequences
Related BOS standards
Date

Examples of decisions requiring an ADR:

Choosing Drizzle or Prisma
Introducing event sourcing
Adding a graph database
Creating a separate service
Selecting an authentication provider
Introducing a message queue
Delegating autonomous AI authority
Changing the package dependency model
20. Initial Implementation Sequence

The recommended implementation order is:

Phase 1 — Domain Foundation
Shared identifiers
Timestamps
Participant
Provenance
Thinking Block
Relationship
Judgment Object
Decision lineage
Historical events
Validation
Phase 2 — Domain Demonstration
Create a Judgment Object
Add typed Thinking Blocks
Link reasoning objects
Record a Decision
Record an Outcome
Record a Reflection
Replay the history
Generate a console demonstration
Phase 3 — Application Layer
Commands
Queries
Use cases
Repository interfaces
Judgment Brief generation
Phase 4 — Persistence
PostgreSQL
Schema
Migrations
Repository implementations
Persistence tests
Phase 5 — Web Reference Application
Judgment list
Judgment Workspace
Judgment Canvas
Thinking Block creation
Decision flow
Outcome and Reflection flow
Phase 6 — AI Participation
AI Participant model
Provider adapter
Research
Challenge
Validation
Reflection support
Phase 7 — Collaboration and Graph
Multiplayer participation
Contextual conversations
Expertise discovery
Judgment Graph
Story Graph
Replay visualization
21. Architectural Invariants

Every implementation must preserve these architectural invariants:

The domain is independent of the user interface.
The domain is independent of persistence technology.
The domain is independent of AI providers.
Core rules are testable without infrastructure.
Provenance is represented in the domain.
History is append-oriented.
Decisions remain distinct from Outcomes.
Reflection cannot rewrite prior judgment history.
AI contributions remain attributable.
Conversations retain meaningful attachment.
Dependency direction points toward the domain.
Material architectural changes are documented.
22. Open Decisions

The following choices remain unresolved:

Drizzle versus Prisma
Exact event-history implementation
Authentication provider
Identifier format
Date and time abstraction
Validation library
Monorepo package manager
Build orchestration tooling
Initial AI provider
Hosting platform

These choices should not be made implicitly.

Each should be resolved through implementation evidence or an Architecture Decision Record.

23. References
VISION.md
AGENTS.md
docs/standards/BOS-000.md
docs/standards/BOS-100.md
24. Revision History
Version 0.1.0

Initial foundation draft defining the BOS Platform software architecture.