# Vision

## The Problem

Modern LLM agents suffer from **context window degradation**. As conversations grow longer:

- Computational costs increase quadratically
- "Lost in the Middle" effect reduces information recall
- Tool call efficiency drops by up to 70%
- Critical information accuracy falls from 92% to 63%

Even with context windows expanding from 4K to 1M+ tokens, the fundamental constraint remains: **longer context leads to worse performance**.

## The Vision

> **Enable deeper, more complex work with less main session context.**

We envision a future where:

1. **Main sessions remain lightweight** - The orchestrator holds only state and decisions, never implementation details
2. **Workers operate in isolation** - Each specialized agent works with a clean, focused context
3. **Quality is guaranteed** - Structured handoffs and validation gates ensure consistency
4. **Complexity scales efficiently** - Adding more features doesn't degrade the agent's reasoning ability

## Success Metrics

| Metric | Current State | Target State |
|--------|---------------|--------------|
| Main session context usage | Unbounded growth | Capped at lightweight state |
| Worker context pollution | Shared instructions | Role-specific only |
| Information handoff | Unstructured | Structured summaries (1-2K tokens) |
| Quality consistency | Variable | Gate-validated at phase boundaries |

## Core Belief

The solution is not larger context windows. The solution is **smarter context engineering**:

- **Write** - Persist state externally
- **Select** - Load only what's needed
- **Compress** - Summarize at boundaries
- **Isolate** - Separate worker contexts

This framework exists to make these principles actionable.
