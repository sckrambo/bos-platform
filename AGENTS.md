# AGENTS.md

## Purpose

This document governs all artificial intelligence agents and automated contributors working in the BOS repository.

The BOS standards are normative.

Agents must implement the standards rather than reinterpret, weaken, or silently replace them.

## Required Reading

Before proposing or making changes, an agent must review:

1. `VISION.md`
2. `README.md`
3. Relevant documents in `docs/standards`
4. Relevant architecture and decision records

When documents conflict, the following order of authority applies:

1. `docs/standards/BOS-000.md`
2. Other documents in `docs/standards`
3. Architecture decision records
4. Product specifications
5. Implementation details
6. Agent suggestions

## Core Rules

### 1. Judgment Is the Primary Domain

BOS is a structured judgment platform.

Implementation convenience must not distort the judgment model.

### 2. Use BOS Terminology

Use established BOS language consistently.

Preferred terms include:

- Judgment Object
- Thinking Block
- Participant
- Relationship
- Conversation
- Decision
- Outcome
- Reflection
- Lesson
- Judgment Canvas
- Judgment Workspace
- Judgment Graph
- Judgment Brief

Do not introduce substitute terminology without documenting and approving the change.

### 3. Preserve Provenance

Every meaningful contribution must retain its origin.

The system must preserve:

- Who created it
- When it was created
- What it relates to
- How it changed
- Whether it was produced by a human, AI, system, or organization

### 4. Preserve History

History is append-oriented.

Agents must not design systems that silently overwrite meaningful judgment history.

Corrections, revisions, and reflections should be represented as new events or versions.

Reflection must never rewrite the original decision.

### 5. Decisions Require Lineage

A Decision must be traceable to the reasoning that preceded it.

Decision lineage may include:

- Context
- Observations
- Evidence
- Assumptions
- Alternatives
- Risks
- Challenges
- Participants
- Confidence
- Dissent

### 6. Thinking Blocks Are Structured Objects

Observations, Evidence, Assumptions, Alternatives, Risks, Decisions, Outcomes, Reflections, and Lessons are structured domain objects.

They must not be reduced to untyped text when their meaning is known.

### 7. Conversations Belong to Something

There is no context-free global conversation in the core BOS model.

A Conversation must attach to a meaningful object, such as:

- A Thinking Block
- A Relationship
- A Decision
- An Outcome
- A Reflection

### 8. AI Participates but Does Not Silently Decide

AI may recommend, analyze, challenge, summarize, and assist.

AI-generated content must be distinguishable from human-generated content.

AI must distinguish among:

- Fact
- Inference
- Speculation
- Recommendation
- Confidence

Any delegated decision authority must be explicit, bounded, logged, and reviewable.

### 9. Domain Logic Must Remain Independent

Core judgment rules belong in domain packages rather than interface components.

User interface code must not become the sole location of domain validation.

### 10. Validate the Grammar

Important domain rules must have automated tests.

Examples include:

- Decisions require lineage
- Reflections cannot rewrite prior history
- Conversations require attachment
- Assumptions remain visible
- Provenance is preserved
- AI contributions are attributable

### 11. Prefer a Modular Monolith

Begin with a modular monolith unless a documented architecture decision justifies greater distribution.

Do not introduce unnecessary services, queues, databases, or infrastructure.

### 12. Make Small, Reviewable Changes

Agents should:

- Explain their plan
- Identify affected files
- Make focused changes
- Add or update tests
- Report assumptions
- Report unresolved questions
- Avoid unrelated refactoring

## Implementation Expectations

The preferred initial implementation stack is:

- TypeScript
- Next.js
- React
- PostgreSQL
- Drizzle or Prisma
- Vitest
- Playwright

This stack may change through a documented architecture decision.

## Prohibited Behavior

Agents must not:

- Invent BOS requirements without labeling them as proposals
- Remove provenance for convenience
- Collapse distinct lifecycle stages
- Treat reflection as retroactive correction
- Allow silent AI decisions
- Hide uncertainty
- Delete dissent from the historical record
- Implement major architecture changes without documenting them
- Generate large amounts of application code before understanding the relevant standards

## Working Method

Before implementation, provide:

1. A summary of the requested change
2. The standards that govern it
3. The files expected to change
4. Key assumptions
5. A validation plan

After implementation, provide:

1. A summary of changes
2. Tests performed
3. Standards satisfied
4. Known limitations
5. Recommended next step

## Foundational Principle

No meaningful act of reasoning shall be lost.