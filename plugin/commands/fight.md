---
description: Run all fight-club reviewers against a file, diff, or PR. Aggregates verdicts from every installed skill.
argument-hint: <file|diff|PR-number>
allowed-tools: [Read, Glob, Grep, Bash, Agent]
---

# /fight

Run every fight-club reviewer against the target and produce a combined report.

The user invoked this command with: $ARGUMENTS

## Step 1: Get the code

Parse $ARGUMENTS to determine the target:
- File path → read the file
- Number → `gh pr diff <number>` to get the PR diff
- "diff" or empty → `git diff HEAD`

## Step 2: Run all reviewers

Apply each of these lenses to the code. For each, produce findings using that skill's output format:

1. **Adversarial Architect** — design, factoring, coupling, abstraction
2. **Security Auditor** — vulnerabilities, trust boundaries, data exposure
3. **Performance Skeptic** — scaling assumptions, inefficiencies, hot paths
4. **Test Critic** — coverage gaps, weak assertions, missing failure modes
5. **Pre-Mortem** — production failure scenarios, silent failures, recovery risk

## Step 3: Aggregate

After all reviewers, produce a summary:

```
## Fight Club Report

### Adversarial Architect
[findings or "No issues found"]
Verdict: APPROVED | NEEDS WORK | REJECT

### Security Auditor
[findings or "No issues found"]
Verdict: APPROVED | NEEDS WORK | REJECT

### Performance Skeptic
[findings or "No issues found"]
Verdict: APPROVED | NEEDS WORK | REJECT

### Test Critic
[findings or "No issues found"]
Verdict: SOLID | NEEDS WORK | REWRITE

### Pre-Mortem
[findings or "No issues found"]
Verdict: READY TO SHIP | NEEDS HARDENING | DO NOT SHIP

---
## Overall Verdict
[APPROVED / NEEDS WORK / REJECT]
[2-3 sentences: biggest risks, blockers, recommended next action]
```

The overall verdict is REJECT if any reviewer issues a REJECT or DO NOT SHIP. NEEDS WORK if any reviewer flags issues. APPROVED only if all reviewers approve.
