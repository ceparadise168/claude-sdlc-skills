# Claude SDLC Skills

> English | **[正體中文](README.zh-TW.md)**

**Let AI guard every stage of your development process — from requirements to release, so nothing falls through the cracks.**

---

## What Is This?

A set of 5 skills for [Claude Code](https://claude.com/claude-code) that provide structured guidance at every stage of software development:

| Phase | What You Say | What AI Does |
|-------|-------------|--------------|
| Requirements | "Write user stories for checkout" | INVEST-compliant user stories, acceptance criteria, PRD |
| Architecture | "Design the architecture for this service" | ASCII diagram confirmation, API contracts, domain boundaries |
| Test Planning | "What should I test before launch?" | Edge cases, stress test plan, E2E scenarios, security tests |
| Release Readiness | "Are we ready to ship?" | Release checklist, rollback plan, go/no-go assessment |

Or run the entire lifecycle at once:

> "Guide this feature from idea to production using Agile methodology"

AI switches phases automatically, pausing at review gates for your confirmation — ensuring no step is skipped.

---

## Why Does This Matter?

### Problems It Solves

| Common Problem | How These Skills Help |
|---------------|----------------------|
| Coding starts before requirements are clear — rework when direction was wrong | Forces user stories and acceptance criteria first; confirm scope before writing code |
| Architecture decisions live only in conversations or whiteboards | ASCII diagram confirmation protocol produces a Design Doc as a lasting artifact |
| Edge cases discovered only after launch | Systematic check across 11 categories (concurrency, injection, auth errors...) |
| Stress testing skipped, system breaks under load | Mandatory Load / Spike / Soak / Capacity testing before every launch |
| No rollback plan when releases go wrong | Rollback plan with trigger thresholds and step-by-step procedures generated upfront |
| Different teams follow different processes, inconsistent quality | Three methodology templates (Agile / Waterfall / Iterative) provide a unified baseline |

### Value Delivered

- **Reduce rework costs** — catch ambiguity at the requirements stage, not after implementation
- **Raise delivery quality** — every feature goes through the same quality gates
- **Accelerate onboarding** — junior engineers produce senior-level technical documents
- **Create a shared language** — how to write requirements, draw architecture, plan tests — consistent across the team
- **Capture institutional knowledge** — processes become reusable templates, not tribal knowledge

---

## How It Works

```
You describe a feature
        │
        v
┌─────────────────────────────────┐
│  Select methodology              │
│  Agile / Waterfall / Iterative   │
└──────────────┬──────────────────┘
               │
        ┌──────v──────┐
        │ Requirements │  ← user stories, acceptance criteria, PRD
        └──────┬──────┘
          Gate 1 ✓ you confirm
        ┌──────v──────┐
        │ Architecture │  ← ASCII diagrams, API contracts, domain model
        └──────┬──────┘
          Gate 2 ✓ you confirm
        ┌──────v──────┐
        │ Test Planning│  ← edge cases, stress test plan, E2E scenarios
        └──────┬──────┘
          Gate 3 ✓ you confirm
        ┌──────v──────┐
        │   Release    │  ← release checklist, rollback plan
        │  Readiness   │
        └──────┬──────┘
          Gate 4 ✓ you confirm
               │
               v
           Deploy
```

Every gate pauses, lists open items and risks, and waits for your "OK" before proceeding.

---

## Installation

### Prerequisites

- [Claude Code](https://claude.com/claude-code) CLI installed

### One-Line Install (Recommended)

```bash
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-orchestrator.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-requirements-shaper.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-architecture-designer.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-test-planner.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-release-readiness.skill
```

### Local Install

```bash
git clone https://github.com/ceparadise168/claude-sdlc-skills.git
cd claude-sdlc-skills
claude install-skill sdlc-orchestrator.skill
claude install-skill sdlc-requirements-shaper.skill
claude install-skill sdlc-architecture-designer.skill
claude install-skill sdlc-test-planner.skill
claude install-skill sdlc-release-readiness.skill
```

---

## Usage Examples

### Full Lifecycle

```
You:  Guide "membership points redemption" from requirements to release using Agile

AI:   Starting with requirements definition.
      [produces user stories, acceptance criteria]
      Requirements confirmed?

You:  Confirmed

AI:   Moving to architecture design.
      [produces architecture diagram, API contracts, domain model]
      Architecture confirmed?

You:  Yes

AI:   Moving to test planning...
      (continues through release readiness)
```

### Using Individual Skills

| Scenario | What to Say |
|----------|------------|
| Requirements only | "Write a PRD for the billing feature" |
| Architecture only | "Design the notification service architecture with ASCII diagrams" |
| Test plan only | "Black Friday traffic will spike 10x — plan stress tests" |
| Release checklist only | "We're deploying the new payment feature Friday — create a release checklist" |

---

## Engineering Practices Covered

<details>
<summary><b>Requirements</b> — turn vague ideas into actionable specs</summary>

- **User Stories** — INVEST-compliant (Independent, Negotiable, Valuable, Estimable, Small, Testable)
- **Acceptance Criteria** — Given/When/Then format, directly translatable to test cases
- **PRD Template** — problem statement, business objective, scope, stakeholders, success metrics
- **Priority Levels** — Must / Should / Could
</details>

<details>
<summary><b>Architecture</b> — confirm with diagrams before building</summary>

- **ASCII Diagram Confirmation Protocol** — draw → mark uncertainties → you confirm → design locked
- **Domain-Driven Design (DDD)** — bounded contexts, aggregates, entities, value objects
- **Clean Architecture** — domain → application → interface adapters → infrastructure (dependencies point inward)
- **Design by Contract (DbC)** — preconditions, postconditions, invariants for every API
- **12-Factor App** — config separation, stateless processes, log streams, disposability
</details>

<details>
<summary><b>Test Planning</b> — far beyond happy paths</summary>

- **11 Edge Case Categories** — concurrency/race conditions, boundary values, SQL injection/XSS, 401/403 auth errors, DB deadlocks, idempotency, network failures...
- **4 Stress Test Types** — Load (baseline performance), Spike (burst traffic), Soak (memory leak detection), Capacity (find the breaking point)
- **Playwright E2E** — mandatory end-to-end testing when UI exists
- **Resilience Testing** — circuit breakers, retry behavior, cascading failures
- **Traceability Matrix** — every user story mapped to its test cases
</details>

<details>
<summary><b>Release Readiness</b> — deployment is not the finish line</summary>

- **Release Checklist** — code, tests, docs, security, infrastructure verified item by item
- **Rollback Plan** — trigger criteria (error rate > X%, latency > Y ms), rollback steps, verification
- **Rollout Strategy Guidance** — Feature Flag / Canary / Blue-Green / Phased / Pilot / Big Bang
- **Post-Release Monitoring Plan** — what to watch for the first 24-48 hours
- **Formal Sign-Off Table** — for Waterfall workflows requiring stakeholder approval
</details>

---

## Methodology Comparison

| | Agile | Waterfall | Iterative |
|--|-------|-----------|-----------|
| **Best For** | Evolving requirements, frequent releases | Regulatory compliance, formal approvals | Engineering-first, phased delivery |
| **Documentation** | Lightweight | Comprehensive | Moderate |
| **Review Gates** | Verbal confirmation | Formal sign-off | Written confirmation |
| **User Stories** | Current sprint focus | Fully defined upfront | Current iteration + overall vision |

---

## Skill Source Code

Want to customize? Source code is in these directories — fork and modify as needed:

```
sdlc-orchestrator/           ← Coordinator: methodology selection, gate management, phase sequencing
sdlc-requirements-shaper/    ← Requirements: user stories, acceptance criteria, PRD template
sdlc-architecture-designer/  ← Architecture: ASCII diagrams, DDD, DbC, design doc template
sdlc-test-planner/           ← Testing: edge case checklists, stress test patterns, E2E templates
sdlc-release-readiness/      ← Release: release checklist, rollback plan template
```

Each directory contains `SKILL.md` (skill definition), `references/` (templates and reference docs), and `evals/` (trigger evaluation test cases for description optimization).

---

## License

MIT
