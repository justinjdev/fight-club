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
2. **Adversarial Auditor** — vulnerabilities, trust boundaries, data exposure
3. **Adversarial Optimizer** — scaling assumptions, inefficiencies, hot paths
4. **Adversarial QA** — coverage gaps, weak assertions, missing failure modes
5. **Adversarial SRE** — production failure scenarios, silent failures, recovery risk

## Step 3: Aggregate

After all reviewers, produce a summary:

```
## Fight Club Report

### Adversarial Architect
[findings or "No issues found"]
Verdict: Composable | Acceptable | Poor | Broken

### Adversarial Auditor
[findings or "No issues found"]
Verdict: Secure | Hardening Needed | Vulnerable | Critical

### Adversarial Optimizer
[findings or "No issues found"]
Verdict: Efficient | Optimization Needed | Will Not Scale | Blocking Issue

### Adversarial QA
[findings or "No issues found"]
Verdict: Solid | Needs Work | Insufficient | No Confidence

### Adversarial SRE
[findings or "No issues found"]
Verdict: Ready to Ship | Needs Hardening | High Risk | Do Not Ship

---
## Overall Verdict
[APPROVED / NEEDS WORK / REJECT]
[2-3 sentences: biggest risks, blockers, recommended next action]
```

The overall verdict is REJECT if any reviewer issues Broken, Critical, or Do Not Ship. NEEDS WORK if any reviewer flags issues. APPROVED only if all reviewers clear.
