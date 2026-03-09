---
name: pre-mortem
description: This skill should be used when the user asks to "run a pre-mortem", "think about failure modes", "what could go wrong", "imagine this failed", or when reviewing a feature, design, or PR before it ships to production.
---

# Pre-Mortem

It is 3am. This code shipped last week. PagerDuty just woke you up. The incident has been ongoing for 45 minutes and is affecting all users.

Your job: figure out what went wrong — before it happens.

## The Frame

Assume this code caused a production incident. Work backwards. What failure mode triggered it?

This is not about finding bugs in the code. It is about finding the conditions under which this code fails in ways that are hard to detect, hard to recover from, or catastrophic in impact.

## What You Look For

### Silent Failures
- Code that swallows errors and continues as if nothing happened
- Fallback behavior that masks the real problem (returning empty instead of erroring)
- Missing metrics/logging at failure points — you'd never know it was broken

### Cascading Failures
- What happens if a downstream dependency is slow? Times out? Returns corrupt data?
- Does failure in one component take down unrelated components?
- Missing circuit breakers, bulkheads, or rate limiting

### Data Corruption
- Operations that partially succeed — what state is the system in if we crash halfway?
- Missing transactions around multi-step operations
- Non-idempotent operations that get retried

### Load & Scale
- What breaks at 10x current load?
- Unbounded queues, caches, or in-memory state
- N+1 patterns that are fine now but will kill the database later

### Operational Blindness
- No way to tell this is failing without user complaints
- No way to roll back safely
- No way to debug — insufficient context in logs/traces when something goes wrong

### Human Error
- Config values that are easy to misconfigure in production
- Behavior that differs meaningfully between environments
- Irreversible operations without confirmation or dry-run mode

## Output Format

```
## Most Likely Incident Scenarios
[Ranked by probability × impact. Each scenario: trigger condition, failure mode, blast radius, detection lag]

## Silent Failure Risks
[Things that could be broken for hours before anyone notices]

## Recovery Concerns
[What makes this hard to roll back or recover from?]

## Verdict
READY TO SHIP | NEEDS HARDENING | DO NOT SHIP
[One sentence on the biggest risk]
```

Be specific. "Database could be slow" is not useful. "The synchronous DB call on the hot path has no timeout — a single slow query will exhaust the connection pool and take down all API endpoints within 30 seconds" is useful.
