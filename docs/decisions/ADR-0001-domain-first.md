# ADR-0001: Adopt a Domain-First Architecture

**Status:** Accepted  
**Date:** 2026-07-21

## Context

BOS is intended to be a long-lived platform for structured judgment. Over time it will include web applications, AI participants, persistence, APIs, collaboration features, analytics, and replay.

Without a clear architectural boundary, business rules become scattered across user interfaces, databases, and infrastructure. As the platform evolves, this increases complexity and makes it more difficult to preserve the meaning of BOS standards.

The concepts that define BOS—Judgment Objects, Thinking Blocks, Participants, Relationships, Decisions, Outcomes, Reflections, Provenance, and History—should exist independently of any specific technology.

## Decision

The BOS Platform will adopt a **domain-first architecture**.

The domain layer is the authoritative implementation of BOS concepts and behavior.

The domain must not depend on:

- User interface frameworks
- Web frameworks
- Database libraries
- AI provider SDKs
- Cloud providers
- Authentication providers

Dependencies must always point toward the domain.

```text
Presentation
      │
      ▼
Application
      │
      ▼
Domain
```

Infrastructure components may depend on the domain.

The domain must never depend on infrastructure.

Core judgment behavior—including lineage, provenance, attribution, replay, validation, and historical integrity—belongs within the domain layer.

## Consequences

### Benefits

- Business rules have a single authoritative implementation.
- Core behavior can be tested without a browser or database.
- User interfaces can evolve independently of judgment logic.
- Infrastructure can be replaced without changing BOS semantics.
- AI remains a participant in judgment rather than the source of truth.

### Trade-offs

- Additional design effort is required before implementing user interfaces.
- Mapping between domain objects and persistence models may require additional code.
- Architectural boundaries must be actively maintained as the project grows.

## Alternatives Considered

### Database-first

Rejected because storage should not define business meaning.

### UI-first

Rejected because presentation is only one representation of the judgment model.

### Framework-centric

Rejected because BOS should remain independent of any single framework or technology stack.

## Review Criteria

This decision remains valid if:

- The domain can be tested independently.
- Core business rules reside in `packages/domain`.
- Dependency direction always points toward the domain.
- Significant architectural changes are recorded in future ADRs rather than modifying this decision.

## References

- `ARCHITECTURE.md`
- `docs/standards/BOS-000.md`
- `docs/standards/BOS-100.md`