# Gates

> Validation checkpoints between Phases. Each Gate contains Aspects with Criteria.

---

## Purpose

Gates are **quality checkpoints** that must be passed before proceeding to the next Phase:

1. **Specification Gate**: Validates specs before implementation
2. **Implementation Gate**: Validates code before completion

Gates ensure quality is maintained throughout the workflow.

---

## Background

### Why Gates?

From DevOps Quality Gate patterns:
> "A Quality Gate is a checkpoint where specific quality criteria must be met before progressing"

Gates prevent:
- Incomplete specifications reaching implementation
- Low-quality code reaching production
- Errors propagating through phases

### Gate-Aspect-Criteria Hierarchy

```
Gate (Phase boundary checkpoint)
├── Aspect (Area of expertise)
│   └── Criteria (Specific checks)
└── Aspect
    └── Criteria
```

Each **Aspect** is validated by a dedicated **Reviewer Worker**.

---

## Directory Structure

```
gates/
├── README.md                    # This file
│
├── specification/               # Specification Phase Gate
│   ├── _gate.md                 # Gate metadata
│   └── aspects/
│       ├── completeness.md      # Aspect: Requirements completeness
│       └── feasibility.md       # Aspect: Technical feasibility
│
└── implementation/              # Implementation Phase Gate
    ├── _gate.md                 # Gate metadata
    └── aspects/
        ├── code-style.md        # Aspect: Code style compliance
        ├── architecture.md      # Aspect: Architecture principles
        └── component.md         # Aspect: Component design
```

---

## Gate Definition (`_gate.md`)

### Front Matter

```yaml
---
type: gate
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [gate, {phase-name}]
related: [../../workflows/phases.md]

# Gate-specific
phase: specification | implementation
pass-condition: all-aspects | percentage
pass-threshold: 100  # If percentage
---
```

### Content Structure

```markdown
# Gate: {Phase} Gate

## Description
[Purpose of this gate]

## When Applied
[At what point in the workflow]

## Aspects
| Aspect | Reviewer | Description |
|--------|----------|-------------|
| completeness | Completeness Reviewer | ... |
| feasibility | Feasibility Reviewer | ... |

## Pass Condition
[all-aspects | X% of criteria]

## On Pass
[What happens when gate passes]

## On Failure
[What happens when gate fails]
```

---

## Aspect Definition

### Front Matter

```yaml
---
type: aspect
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [aspect, {gate-name}, {aspect-name}]
related: [../_gate.md]

# Aspect-specific
gate: specification | implementation
reviewer: {reviewer-type}
---
```

### Content Structure

```markdown
# Aspect: {Name}

## Description
[What this aspect validates]

## Criteria

### Required (Must Pass)
- [ ] Criterion 1
- [ ] Criterion 2

### Recommended (Should Pass)
- [ ] Criterion 3
- [ ] Criterion 4

## Validation Method
[How the Reviewer should check each criterion]

## Feedback Format
[How to report findings]
```

---

## Planned Gates

### Specification Gate

| Aspect | Focus | Criteria Examples |
|--------|-------|-------------------|
| **Completeness** | Are all requirements captured? | Functional reqs, Non-functional reqs, Edge cases |
| **Feasibility** | Is it technically achievable? | Tech stack compatibility, Resource estimation |

### Implementation Gate

| Aspect | Focus | Criteria Examples |
|--------|-------|-------------------|
| **Code-Style** | Does code follow standards? | Naming, Formatting, Comments |
| **Architecture** | Does structure follow principles? | Single responsibility, Dependency direction |
| **Component** | Is component design sound? | Props design, Reusability, Composition |

---

## Reviewer-Aspect Relationship

Each Aspect maps to a Reviewer instance:

```
Implementation Gate
├── Aspect: code-style ────► Reviewer Worker (Code-Style)
├── Aspect: architecture ──► Reviewer Worker (Architecture)
└── Aspect: component ─────► Reviewer Worker (Component)
```

Reviewers can run in **parallel** since Aspects are independent.

---

## Validation Flow

```
Orchestrator
    │
    ▼
Gate Triggered
    │
    ├──► Spawn Reviewer (Aspect 1) ──► Check Criteria ──► Handoff
    ├──► Spawn Reviewer (Aspect 2) ──► Check Criteria ──► Handoff
    └──► Spawn Reviewer (Aspect 3) ──► Check Criteria ──► Handoff
                                              │
                                              ▼
                                    Orchestrator Aggregates
                                              │
                              ┌───────────────┴───────────────┐
                              ▼                               ▼
                        All Pass                         Some Fail
                              │                               │
                              ▼                               ▼
                     Proceed to Next Phase           Request Fix or User Decision
```

---

## Status Values

| Status | Meaning |
|--------|---------|
| `draft` | Gate/Aspect being defined, not enforced |
| `active` | Currently in effect, must pass |
| `deprecated` | Being phased out |
| `archived` | Historical reference |

---

## Best Practices

### Keep Criteria Specific

Bad: "Code should be clean"
Good: "Functions must be under 50 lines"

### Make Criteria Verifiable

Bad: "Architecture should be good"
Good: "No circular dependencies between modules"

### Balance Strictness

- Too strict → Development slows
- Too loose → Quality suffers
- Start stricter, relax based on experience

---

## Related

- `../workflows/` for Phase definitions
- `../constitutions/workers/reviewer.md` for Reviewer principles
- `.claude/agents/reviewer.md` for Reviewer behavior
