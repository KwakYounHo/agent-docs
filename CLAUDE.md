## **CRITICAL RULES**
- When you're uncertain or unable to make an independent judgment, ask the user.

## Conversation Rules
- **IMPORTANT** You must converse with the user in Korean, as it's their native language.
- Writing documentation and code in English, but keep user-facing messages in Korean.

## **Project Identity (MUST READ)**

## Context Window Management Strategy
- Actively leverage Subagents when summarization or deep analysis is needed.
- Treat the Main Session's context window as a precious resource.
- Workers are defined in `.claude/agents/` - use them for delegated tasks.

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

## Key Concepts

### Constitution vs Instruction (ADR-002)

| | Constitution | Instruction |
|---|-------------|-------------|
| **Essence** | Law to obey | Responsibility to fulfill |
| **Location** | `blueprint/constitutions/` | `.claude/agents/` |
| **Content** | Principles, Boundaries | Role, Workflow, Handoff format |

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
