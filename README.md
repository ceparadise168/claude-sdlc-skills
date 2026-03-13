# Claude SDLC Skills

> English | **[正體中文](README.zh-TW.md)**

**Let AI guard every stage of your development process — from requirements to release, so nothing falls through the cracks.**

---

## Design Philosophy

Most SDLC tools treat best practices as rules. These skills treat them as **tools you choose from** — consciously, based on the situation at hand.

The same design that saves one project can strangle another. What matters is not whether you applied DDD or Clean Architecture, but whether the choice was *conscious* — made with awareness of the team, the timeline, the risk, and the reality on the ground.

**Structure without dogma.** Every phase provides a framework, but the depth scales to context. An internal tool for 5 users and a payment system for millions both go through the same gates — with radically different rigor.

**Conscious trade-offs, not unconscious shortcuts.** When work is deferred — a simpler architecture, less test coverage — the skill names it: what you gain, what you give up, what would change the call. Intentional technical debt is wisdom. Unexamined shortcuts are just debt.

**Think about abstraction, then wait.** Not abstracting now doesn't mean not thinking about it. It means you've considered where things might go, and decided it's too early. The skills record what was considered and why — so the decision can be revisited later.

**Code is fluid.** It serves a solution that will evolve. These skills favor structures that can change shape without expensive rewrites — but won't over-engineer for futures that may never arrive.

**Know what's principled and what's adjustable.** Some things don't bend (testable acceptance criteria, rollback plans). Others flex with context (DDD or not, how many layers, how formal the sign-off). The skills name which is which.

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

### Option A: Install as Plugin (all 5 skills)

```bash
git clone https://github.com/ceparadise168/claude-sdlc-skills.git
claude --plugin-dir ./claude-sdlc-skills
```

Or add it permanently — inside a Claude Code session, run:

```
/plugin marketplace add ceparadise168/claude-sdlc-skills
```

### Option B: Install Individual Skills

Pick only what you need:

```bash
# Orchestrator — coordinates all phases with review gates
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-orchestrator.skill

# Requirements — user stories, acceptance criteria, PRD
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-requirements-shaper.skill

# Architecture — ASCII diagrams, DDD, API contracts
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-architecture-designer.skill

# Test Planning — edge cases, stress tests, E2E scenarios
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-test-planner.skill

# Release Readiness — release checklist, rollback plan
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-release-readiness.skill
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
skills/
├── sdlc-orchestrator/           ← Coordinator: methodology selection, gate management, phase sequencing
├── sdlc-requirements-shaper/    ← Requirements: user stories, acceptance criteria, PRD template
├── sdlc-architecture-designer/  ← Architecture: ASCII diagrams, DDD, DbC, design doc template
├── sdlc-test-planner/           ← Testing: edge case checklists, stress test patterns, E2E templates
└── sdlc-release-readiness/      ← Release: release checklist, rollback plan template
```

Each directory contains `SKILL.md` (skill definition), `references/` (templates and reference docs), and `evals/` (trigger evaluation test cases for description optimization).

---

## License

MIT
