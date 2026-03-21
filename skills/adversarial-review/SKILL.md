---
name: adversarial-review
description: >
  Use when rigorous adversarial critique is needed for any artifact — code, APIs, plans,
  architectures, UX flows, data schemas, infrastructure configs, or documents.
  Triggers on: "adversarial review", "stress test this", "validate this hard",
  "find weaknesses", "critique this plan/code/design/architecture".
---

## Overview

Adversarial Review is a 5-stage protocol that first understands what "good" looks like
in the domain being reviewed, then validates claims, tests executor capability, forces
discovery of non-obvious issues, and turns every finding into an actionable resolution.

**Core principle:** A good review isn't about throwing criticism, but transforming findings into decisions.

---

## Stage 0 — Domain Reconnaissance & Best Practices

**Objective:** Before critiquing, understand the standard. Know what excellent looks like in this domain.

### Step 1: Identify the Artifact Type

```
What TYPE is this artifact?
→ Code (language? framework?)
→ API design (REST, GraphQL, gRPC?)
→ Architecture (microservices, monolith, event-driven?)
→ Product plan / brainstorming document
→ Database schema / data model
→ Infrastructure / DevOps config
→ UX flow / wireframe spec
→ Documentation
→ Other: [describe]
```

### Step 2: Research Best Practices for That Domain

Use web_search **minimum 2x** targeting:
- "best practices [domain]"
- "[domain] common mistakes / anti-patterns"
- Production post-mortems or failure cases in this domain

### Output Format

```
**Domain:** [artifact type + specific tech/context]
**Excellence benchmark:** [3-5 bullet points: what does excellent look like here?]
**Known anti-patterns:** [2-3 common traps for this domain]
**Review lens:** [what specific lens will you apply throughout stages 1-4?]
```

---

## Stage 1 — Reality vs Claims

**Objective:** Validate claims and assumptions against real data, docs, and best practices benchmark from Stage 0.

### Execution Rules

1. Use web_search and web_fetch minimum **3 times** with different angles
2. Diversify: library docs, benchmarks, user pain points, known issues, comparisons
3. Cross-reference findings against the Stage 0 excellence benchmark
4. Mark each claim: **VALID** / **PARTIAL** / **INVALID** / **UNVERIFIED**

### Search Angle Guidelines

```
Angle 1: Core tech/library → known limitations, version issues, gotchas
Angle 2: User pain points → real UX research or forum complaints
Angle 3: Benchmark/performance → real data, not marketing claims
Angle 4: Competitor/alternative → how others solved the same problem
Angle 5: Production failure → post-mortems, known failure modes
```

### Output Format

```
**Claim:** "[claim from document]"
**Status:** VALID / PARTIAL / INVALID / UNVERIFIED
**Facts:** [data from search]
**Best practice alignment:** [does this align with Stage 0 benchmark?]
**Hidden caveat:** [not mentioned in original]
```

---

## Stage 2 — Acceptance Criteria

**Objective:** Not just whether the idea can be done, but whether this specific executor can do it.

### Questions to Answer

```
1. Does the chosen tech stack match the executor's skills?
2. Are there components requiring significant learning curve?
3. Is the time estimate realistic for this executor (not a team)?
4. Are there external dependencies outside the executor's control?
5. Is there a proof-of-concept needed before committing to production?
6. Does the approach match the executor's operational constraints (infra, team size, budget)?
```

### Output Format

```
**Component:** [component/feature/system name]
**Verdict:** Executable / Partial / Needs PoC first / Beyond capability
**Reason:** [specific, honest]
**Recommendation:** [concrete steps if there's a gap]
```

---

## Stage 3 — Mandatory Issues Quota

**Objective:** Force discovery of non-obvious problems. Prevent reviews that are too soft.

### Quota Rule (NON-NEGOTIABLE)

```
MINIMUM 3 specific issues must be found.
If < 3 found → SYSTEM MUST SEARCH AGAIN across:
  - Best practice violations: does this deviate from Stage 0 benchmark?
  - Edge cases: what happens during extreme conditions?
  - Performance issues: what happens at scale/high load?
  - Architecture violations: is this design internally consistent?
  - Dependency risk: what happens if a library or service changes?
  - Security gaps: are there attack surfaces or data leakage risks?
  - UX failure modes: what happens when users do unexpected things?
  - Operational concerns: observability, debuggability, rollback?
```

### Severity Categories

| Level | Definition | Example |
|-------|------------|---------|
| 🔴 Critical | Will kill the product/project if not fixed | Core promise cannot be delivered |
| 🟠 High | Will cause significant problems in production | Performance degradation, bad UX |
| 🟡 Medium | Real issue but has workaround | File clutter, minor inconsistency |
| 🟢 Low | Needs attention but not urgent | Naming convention, minor inefficiency |

### Issue Report Format

```
### [EMOJI] [LEVEL] #N — [Short Title]

**Problem:** [specific description, not generic]
**Trigger scenario:** [when this issue occurs]
**Impact:** [what happens to user/product]
**Best practice contrast:** [what the benchmark says should happen instead]
**Why it's dangerous:** [why this isn't an edge case that can be ignored]
```

---

## Stage 4 — Interactive Resolution

**Objective:** Every finding must have a resolution path, not just criticism.

### Three Resolution Options (Always Provide All Three)

```
A. Auto-fix
   → Directly fix the problematic idea/plan/code
   → Output: revised version of the affected section
   → Suitable for: problems with clear solutions

B. Action Items
   → Checklist to be executed by user
   → Output: [ ] specific item with done criteria
   → Suitable for: problems requiring user decision

C. Deep Dive
   → Detailed problem explanation with concrete examples
   → Output: in-depth analysis + trade-offs
   → Suitable for: problems requiring understanding before deciding
```

### Resolution Summary Table

```markdown
| # | Issue | Severity | Primary Recommendation |
|---|-------|----------|------------------------|
| 1 | [title] | 🔴 Critical | [one sentence fix] |
| 2 | [title] | 🟠 High | [one sentence fix] |
```

---

## Full Protocol Template

```markdown
# Adversarial Review — [Artifact Name]

## Stage 0 — Domain Reconnaissance
**Domain:** [artifact type + tech context]
**Excellence benchmark:** [3-5 bullets]
**Known anti-patterns:** [2-3 traps]
**Review lens:** [specific lens applied]

## Stage 1 — Reality vs Claims
[3+ web searches with diverse angles]
[Status per claim: VALID/PARTIAL/INVALID/UNVERIFIED + best practice alignment]

## Stage 2 — Acceptance Criteria
[Verdict per component/feature]

## Stage 3 — Mandatory Issues (minimum 3)
[Issue #1 — 🔴/🟠/🟡/🟢]
[Issue #2 — ...]
[Issue #3 — ...]

## Stage 4 — Interactive Resolution
[Per issue: option A / B / C]

### Summary Priority Matrix
| # | Issue | Severity | Primary Recommendation |
|---|-------|----------|------------------------|

## Meta-Conclusion
[One paragraph: what changed the most? what's the first priority action?]
```

---

## Quick Reference

**Applies to any artifact:**
- Code (any language/framework)
- API design (REST, GraphQL, gRPC)
- Architecture diagrams / ADRs
- Product plans, PRDs, brainstorming docs
- Database schemas / data models
- Infrastructure / DevOps configs
- UX flows, wireframe specs, user stories
- Documentation, README, specs

**Do's:**
- Run Stage 0 first — know the standard before judging
- Run stages 1-4 sequentially without skipping
- Diversify web search angles (don't search the same topic 3x)
- Be honest about executor capability in Stage 2
- Provide all three resolution options for each Critical and High finding

**Don'ts:**
- Don't skip Stage 0 — reviewing without knowing the benchmark is guesswork
- Don't stop at < 3 issues in Stage 3
- Don't create resolutions too generic ("just fix it")
- Don't combine multiple distinct problems in one issue report

**Trigger phrases:**
```
"run adversarial review for [artifact]"
"adversarial review this"
"validate this hard"
"stress test this plan/code/design"
"find weaknesses in [artifact]"
"critique this"
```
