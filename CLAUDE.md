## **CRITICAL RULES**
- When you're uncertain or unable to make an independent judgment, ask the user.

## **Project Identity (MUST READ)**

### What This Project Is
- This is a **framework template repository**, NOT a working framework instance.
- We are **creating** a framework, NOT **using** one.
- The goal: Provide minimal constraints and guidelines for target projects to customize.

### What Gets Copied vs What Stays
| Item | Copied to Target? | Purpose |
|------|-------------------|---------|
| `templates/blueprint/*` | ✅ Yes → `blueprint/` | Framework core (schemas, constitutions, gates) |
| `templates/claude-agents/*` | ✅ Yes → `.claude/agents/` | Worker definitions (Instructions) |
| `commands/*` | ✅ Yes → `.claude/commands/` | Slash commands |
| `README.md` files (all) | ❌ No | Developer documentation only |
| `docs/adr/*` | ❌ No | Framework design decisions |

### Template Rules
- Use **placeholders** (`{{project-name}}`, `{{date}}`) for values that vary per project.
- Provide **minimal required structure** - let project maintainers customize.
- **Token efficiency** is critical: base.md ~500 tokens, worker constitutions ~300-500 tokens each.

## Context Window Management Strategy
- Actively leverage Subagents when summarization or deep analysis is needed.
- Treat the Main Session's context window as a precious resource.
- Workers are defined in `.claude/agents/` - use them for delegated tasks.

## Conversation Rules
- **IMPORTANT** You must converse with the user in Korean, as it's their native language.
- Writing documentation and code in English, but keep user-facing messages in Korean.

## Development Status
- **Current Phase**: Phase 3 - Worker Instructions (ADR-001)
- **Completed**: Phase 1 (Schemas), Phase 2 (Constitutions)
- **Next**: `.claude/agents/*.md` templates, Gate/Aspect documents, Slash commands

### Phase 2 Completion Summary (This Session)
```
constitutions/workers/
├── orchestrator.md  ✅ Created
├── specifier.md     ✅ Created
├── implementer.md   ✅ Created
└── reviewer.md      ✅ Created
```

### Phase 3: Worker Instructions (Next)
```
templates/claude-agents/
├── orchestrator.md   # To create - coordinates workers, handles [DECIDE]
├── specifier.md      # To create - creates specifications
├── implementer.md    # To create - implements code
└── reviewer.md       # To create - validates against gates
```

See `templates/claude-agents/README.md` for file format and handoff structure.

## Key Concepts

### Constitution vs Instruction (ADR-002)

| | Constitution | Instruction |
|---|-------------|-------------|
| **Essence** | Law to obey | Responsibility to fulfill |
| **Location** | `blueprint/constitutions/` | `.claude/agents/` |
| **Content** | Principles, Boundaries | Role, Workflow, Handoff format |

### `[DECIDE]` Marker System

**Purpose**: Mark items requiring user judgment in Workflow documents.

| Worker | Usage |
|--------|-------|
| **Specifier** | Marks ambiguous requirements needing clarification |
| **Implementer** | Marks unclear specifications that cannot be implemented |
| **Orchestrator** | Detects markers and requests user confirmation |

**Handoff includes `decide-markers` field** when markers are added.
See `templates/claude-agents/README.md#decide-marker-handling`.

### Orchestrator Role Clarification

> **Important**: Orchestrator is a **Special Worker**, not a subagent.

```
User ↔ Main Session (can assume Orchestrator role)
           │
           ├──► Specifier (subagent)
           ├──► Implementer (subagent)
           └──► Reviewer (subagent)
```

- Main Session can be "delegated" Orchestrator role (not invoked as subagent)
- Implementation method is user's choice (CLAUDE.md, slash command, explicit subagent)
- Framework provides guidelines, not enforcement

### Annotation System (ADR-003)

| Annotation | Purpose | LLM Action |
|------------|---------|------------|
| `[FIXED]` | Framework core rules | Do NOT modify without user confirmation |
| `[INFER]` | Codebase-derivable content | Analyze and fill |
| `[DECIDE]` | User judgment needed | Mark and request confirmation |
| `[ADAPT]` | Conditional content | Evaluate and include/exclude |

## Research Insights (Applied to Constitutions)

### From Spec-kit
- What/Why focus, not How (Specifier)
- Constitution as immutable principles (all Workers)
- Quality gates: Pass/Fail only (Reviewer)

### From SDD Best Practices
- YAGNI principle (Implementer)
- Actionable feedback with location + reason + suggestion (Reviewer)
- ISO/IEC/IEEE 29148 quality criteria (Specifier)

Full research available via Subagent web search for "Spec-kit" and "SDD best practices".

## Project Structure
```
agent-docs/
├── docs/adr/                 # Architecture Decision Records
├── templates/
│   ├── claude-agents/        # Worker definitions (Instructions) 🔄 Next
│   └── blueprint/
│       ├── front-matters/    # FrontMatter Schema definitions ✅
│       ├── constitutions/    # Principles ✅ Complete
│       ├── gates/            # Validation checkpoints
│       └── workflows/        # Work containers
├── initializers/             # Setup scripts
└── commands/                 # Slash commands
```

## Key Terminology
| Term | Definition |
|------|------------|
| Constitution | Laws/Principles Workers must obey |
| Instruction | Responsibilities Workers must fulfill (`.claude/agents/`) |
| Gate | Validation checkpoint (Pass/Fail) |
| Aspect | Specific criteria within Gate |
| `[DECIDE]` | Marker for items needing user judgment |

## Commands
- `/specify` - Start specification for a new workflow
- `/implement` - Begin implementation phase
- `/review` - Run gate validation

## Key References
| Document | Purpose |
|----------|---------|
| `docs/adr/001-schema-first-development.md` | Implementation phases |
| `docs/adr/002-constitution-instruction-separation.md` | Constitution vs Instruction |
| `docs/adr/003-template-annotation-system.md` | Annotation markers |
| `templates/claude-agents/README.md` | Worker file format, Handoff structure |
| `templates/blueprint/gates/README.md` | Gate-Aspect-Criteria hierarchy |
