# Mission

## What We Build

A **practical framework** for orchestrating LLM agents that:

1. Preserves main session context through intelligent delegation
2. Maintains code quality and architectural consistency across parallel workers
3. Provides clear patterns that can be adopted incrementally

## How We Achieve It

### 1. Orchestrator-Workers Pattern

```
Main Session (Orchestrator)
├── State Management     → Track phase, status, pending decisions
├── Task Distribution    → Delegate to specialized workers
├── Result Collection    → Receive structured summaries only
└── Decision Making      → Handle user confirmations

Workers (Subagents)
├── Specifier    → Requirements and specification
├── Implementer  → Code implementation
└── Reviewer     → Quality validation
```

### 2. Role-Specific Constitutions

Instead of one massive instruction set, each worker loads only what it needs:

| Worker | Loads |
|--------|-------|
| Orchestrator | `base.md` + `orchestrator.md` |
| Specifier | `base.md` + `specifier.md` |
| Implementer | `base.md` + `implementer.md` |
| Reviewer | `base.md` + `reviewer.md` |

This prevents context pollution and instruction bleed.

### 3. Quality Gates

Phase boundaries enforce quality through structured validation:

```
Specification Phase → [Specification Gate] → Implementation Phase
                      ├── Completeness Aspect (Reviewer)
                      └── Feasibility Aspect (Reviewer)
```

Each aspect has dedicated criteria and a specialized reviewer.

### 4. Structured Handoffs

Workers return condensed, structured results:

```yaml
files_changed: [...]
status: success | fail
decisions_made: [...]
needs_confirmation: [...]
```

The orchestrator never receives raw implementation details.

## What We Don't Do

| Avoid | Instead |
|-------|---------|
| Over-engineering simple tasks | Use main session directly for trivial work |
| Static task decomposition | Allow dynamic adjustment based on context |
| Framework over-abstraction | Keep patterns transparent and debuggable |
| All-or-nothing adoption | Enable incremental adoption of patterns |

## Guiding Principles

1. **Start simple** - Only add complexity when simpler solutions fall short
2. **Context is precious** - Treat the main session context window as a scarce resource
3. **Isolation by design** - Workers should never pollute each other's context
4. **Quality at boundaries** - Validate at phase transitions, not continuously
5. **Practical over theoretical** - Patterns must work in real Claude Code sessions
