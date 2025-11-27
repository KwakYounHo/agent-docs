---
type: schema
status: active
version: 1.0.0
created: 2024-11-27
updated: 2024-11-27
tags: [schema, common, front-matter]
related: []
---

# Schema: Common FrontMatter

> Defines FrontMatter fields shared by all document types.

## Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `type` | enum | Document type identifier |
| `status` | enum | Current document status |
| `version` | semver | Document version |
| `created` | date | Creation date |
| `updated` | date | Last modification date |

## Optional Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `tags` | string[] | `[]` | Searchable keywords |
| `related` | string[] | `[]` | Related document paths (only files copied together) |

## Field Definitions

### type

- **Type**: enum
- **Required**: Yes
- **Values**: `schema` | `constitution` | `worker` | `gate` | `aspect` | `phase` | `stage` | `feature` | `artifact`
- **Description**: Identifies the document category for routing and validation.

### status

- **Type**: enum
- **Required**: Yes
- **Description**: Current state of the document. Valid values depend on document category.

**Definition Documents** (`schema`, `constitution`, `worker`, `gate`, `aspect`, `phase`, `stage`):
| Value | Description |
|-------|-------------|
| `draft` | Work in progress, not ready for use |
| `active` | Current and in use |
| `deprecated` | Superseded, avoid using |
| `archived` | No longer relevant, kept for history |

**Task Documents** (`feature`, `artifact`):
| Value | Description |
|-------|-------------|
| `pending` | Not yet started |
| `in-progress` | Currently being worked on |
| `completed` | Successfully finished |
| `failed` | Did not complete successfully |

### version

- **Type**: string (semver)
- **Required**: Yes
- **Format**: `MAJOR.MINOR.PATCH`
- **Example**: `1.0.0`, `2.1.3`
- **Description**: Semantic version for tracking document changes.

### created

- **Type**: date
- **Required**: Yes
- **Format**: `YYYY-MM-DD`
- **Example**: `2024-11-27`
- **Description**: Date when the document was first created.

### updated

- **Type**: date
- **Required**: Yes
- **Format**: `YYYY-MM-DD`
- **Example**: `2024-11-27`
- **Description**: Date when the document was last modified.

### tags

- **Type**: string[]
- **Required**: No
- **Default**: `[]`
- **Example**: `[schema, common, front-matter]`
- **Description**: Keywords for search and categorization.

### related

- **Type**: string[]
- **Required**: No
- **Default**: `[]`
- **Example**: `[front-matters/constitution.schema.md, gates/documentation/README.md]`
- **Description**: Paths to related documents. Only reference files that will be copied together to target projects.

## Type-Status Mapping

| Type | Category | Valid Status Values |
|------|----------|---------------------|
| `schema` | Definition | `draft`, `active`, `deprecated`, `archived` |
| `constitution` | Definition | `draft`, `active`, `deprecated`, `archived` |
| `worker` | Definition | `draft`, `active`, `deprecated`, `archived` |
| `gate` | Definition | `draft`, `active`, `deprecated`, `archived` |
| `aspect` | Definition | `draft`, `active`, `deprecated`, `archived` |
| `phase` | Definition | `draft`, `active`, `deprecated`, `archived` |
| `stage` | Definition | `draft`, `active`, `deprecated`, `archived` |
| `feature` | Task | `pending`, `in-progress`, `completed`, `failed` |
| `artifact` | Task | `pending`, `in-progress`, `completed`, `failed` |
