---
id: "000"
title: ADR Template
created: 2025-11-28
status: accepted
scope: process
tags: [template, adr, meta]
related: []
supersedes: null
superseded-by: null
---

# ADR-000: ADR Template

## Context

We need a consistent format for Architecture Decision Records (ADRs) in this framework project. ADRs document significant architectural and design decisions, providing context for future developers and maintaining a history of why certain choices were made.

## Decision

We will use the following ADR format with YAML FrontMatter for metadata and a structured body template.

### FrontMatter Specification

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `id` | ✅ | string | ADR number (e.g., "002") |
| `title` | ✅ | string | Decision title |
| `created` | ✅ | date | Creation date (YYYY-MM-DD) |
| `status` | ✅ | enum | `draft`, `accepted`, `deprecated`, `superseded` |
| `scope` | ✅ | enum | `philosophy`, `design`, `implementation`, `process` |
| `tags` | ✅ | array | Search tags for metadata-based lookup |
| `related` | ❌ | array | Related ADR numbers |
| `supersedes` | ❌ | string | Previous ADR this one replaces |
| `superseded-by` | ❌ | string | New ADR that replaces this one |

### Status Values

| Status | Meaning | When to Use |
|--------|---------|-------------|
| `draft` | Under review, not yet finalized | Initial writing phase |
| `accepted` | Adopted, currently in effect | Decision confirmed |
| `deprecated` | No longer recommended (without replacement) | Situation changed |
| `superseded` | Replaced by a new ADR | Use with `superseded-by` field |

### Scope Values

| Scope | Description | Example |
|-------|-------------|---------|
| `philosophy` | Conceptual/philosophical decisions | Constitution vs Instruction separation |
| `design` | Structural/architectural decisions | Schema structure, file organization |
| `implementation` | Technical implementation decisions | Annotation system, template format |
| `process` | Workflow/procedure decisions | Development workflow, deployment |

### Body Template

```markdown
# ADR-{id}: {title}

## Context
[Why is this decision needed? Background and problem situation]

## Decision
[What was decided? The chosen direction]

## Rationale
[Why was this decision made? The reasoning and justification]

## Consequences
[What are the results of this decision? Pros, cons, trade-offs]

## Alternatives Considered
[What alternatives were considered and why were they not chosen?]
```

## Rationale

1. **Immutability Principle**: ADRs should not be modified after acceptance. Instead, new ADRs supersede old ones, preserving decision history.

2. **Metadata-Based Search**: FrontMatter enables quick filtering and search by status, scope, or tags.

3. **Clear Separation**: Rationale (why we chose) is distinct from Consequences (what happens as a result).

4. **Consistent Structure**: All ADRs follow the same format, making them easier to read and write.

## Consequences

### Positive

- Consistent documentation across all architectural decisions
- Easy to search and filter ADRs by metadata
- Clear history of decision evolution through supersedes/superseded-by
- Rationale preserved separately from consequences

### Negative

- More fields to fill compared to minimal ADR formats
- Requires discipline to maintain supersedes chain

## Alternatives Considered

### Minimal ADR Format

Only Context, Decision, Consequences without FrontMatter.

**Rejected because**: Lacks metadata for search and categorization, no clear status tracking.

### Wiki-based Documentation

Using a wiki instead of markdown files.

**Rejected because**: Not version-controlled with code, harder to review changes.
