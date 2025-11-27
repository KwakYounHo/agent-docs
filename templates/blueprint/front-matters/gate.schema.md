---
type: schema
status: active
version: 1.0.0
created: 2024-11-27
updated: 2024-11-27
tags: [schema, gate, front-matter]
related: [front-matters/base.schema.md, front-matters/aspect.schema.md]
---

# Schema: Gate FrontMatter

> Extends base schema with Gate-specific fields.

## Inherits

All fields from `base.schema.md`:

- `type`, `status`, `version`, `created`, `updated`, `tags`, `related`

## Additional Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Gate identifier (used in Handoff `required-gates`) |
| `validates` | enum | What this gate validates |
| `description` | string | Purpose of this gate |

## Field Definitions

### name

- **Type**: string
- **Required**: Yes
- **Format**: lowercase, hyphen-separated
- **Examples**: `specification`, `implementation`, `documentation`
- **Description**: Unique identifier for the gate. Referenced in Orchestrator → Reviewer Handoff.

### validates

- **Type**: enum
- **Required**: Yes
- **Values**: `code` | `document`
- **Description**: Classification of what this gate validates.

| Value | Description | Examples |
|-------|-------------|----------|
| `code` | Validates code/artifact quality | Specification Gate, Implementation Gate |
| `document` | Validates document format compliance | Documentation Gate |

### description

- **Type**: string
- **Required**: Yes
- **Description**: Human-readable explanation of the gate's purpose and focus.
- **Example**: `"Validates specification completeness before implementation"`

## Constraints

| Rule | Description |
|------|-------------|
| Type | `type` field must be `gate` |
| Status | Must be: `draft`, `active`, `deprecated`, `archived` |
| Name Uniqueness | `name` should be unique across all gates |

## Field Guidelines

### tags (recommended)

- Include: `[gate, {gate-name}]`
- Example: `[gate, specification]`, `[gate, documentation]`

### related (recommended)

- Should reference Aspect files that belong to this gate
- Example: `[./aspects/completeness.md, ./aspects/feasibility.md]`

## Usage Examples

### Code Gate (Specification)

```yaml
---
type: gate
status: active
version: 1.0.0
created: 2024-11-27
updated: 2024-11-27
tags: [gate, specification]
related: [./aspects/completeness.md, ./aspects/feasibility.md]

name: specification
validates: code
description: "Validates specification completeness and feasibility before implementation"
---
```

### Code Gate (Implementation)

```yaml
---
type: gate
status: active
version: 1.0.0
created: 2024-11-27
updated: 2024-11-27
tags: [gate, implementation]
related: [./aspects/code-style.md, ./aspects/architecture.md, ./aspects/component.md]

name: implementation
validates: code
description: "Validates code quality, architecture, and component design"
---
```

### Document Gate

```yaml
---
type: gate
status: active
version: 1.0.0
created: 2024-11-27
updated: 2024-11-27
tags: [gate, documentation]
related: [./aspects/schema-validation.md]

name: documentation
validates: document
description: "Validates document format compliance against schema definitions"
---
```

## Execution Context

Gates are **definition documents**, not executable code. Execution is controlled by Orchestrator:

```yaml
# Orchestrator → Reviewer Handoff
handoff:
  action: review
  document: path/to/document
  required-gates:
    - specification    # Gate name
    - documentation    # Gate name
```

The Orchestrator decides:
- **Which gates** to run (via `required-gates`)
- **Pass/fail criteria** (based on Reviewer response)
- **Next action** (proceed, fix, or escalate)
