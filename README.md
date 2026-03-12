# Claude SDLC Skills

A set of 5 Claude Code skills that guide software features through the full Software Development Life Cycle — from requirements to release.

## Architecture

```
sdlc-orchestrator          ← Thin coordinator: methodology selection, review gates, phase sequencing
    ├── sdlc-requirements-shaper   ← User stories, acceptance criteria, PRDs
    ├── sdlc-architecture-designer ← ASCII diagrams, DDD, Clean Architecture, Design by Contract
    ├── sdlc-test-planner          ← Edge cases, stress tests, Playwright E2E, resilience testing
    └── sdlc-release-readiness     ← Release checklists, rollback plans, go/no-go decisions
```

Each skill works **standalone** (e.g. "write user stories for checkout") or **coordinated** through the orchestrator (e.g. "guide this feature from idea to production").

## Quick Install

Install the `.skill` files directly:

```bash
claude install-skill sdlc-orchestrator.skill
claude install-skill sdlc-requirements-shaper.skill
claude install-skill sdlc-architecture-designer.skill
claude install-skill sdlc-test-planner.skill
claude install-skill sdlc-release-readiness.skill
```

Or install from GitHub URL:

```bash
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-orchestrator.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-requirements-shaper.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-architecture-designer.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-test-planner.skill
claude install-skill https://raw.githubusercontent.com/ceparadise168/claude-sdlc-skills/main/sdlc-release-readiness.skill
```

## Usage

### Full SDLC (orchestrator)

> "Run this feature through full SDLC"
> "Guide this feature from idea to production using Agile methodology"

The orchestrator will:
1. Ask you to select a methodology (Agile / Waterfall / Iterative / Custom)
2. Delegate to sub-skills phase by phase
3. Pause at review gates (G1-G4) for your confirmation

### Standalone skills

Each skill can be used independently:

| Skill | Example Prompts |
|-------|----------------|
| **Requirements Shaper** | "Write user stories for checkout", "Create a PRD for billing" |
| **Architecture Designer** | "Design the architecture for this service", "Help me model this domain" |
| **Test Planner** | "What should I test?", "Plan stress tests for Black Friday traffic" |
| **Release Readiness** | "Are we ready to ship?", "Create a release checklist" |

## What's Included

### Methodologies
- **Agile** — lightweight artifacts, informal gates, sprint-sized stories
- **Waterfall** — full documentation, formal sign-offs, comprehensive checklists
- **Iterative** — phased delivery, moderate formality, evolving artifacts
- **Custom** — adapts to your existing process

### Key Principles
- INVEST-compliant user stories with Given/When/Then acceptance criteria
- ASCII diagram confirmation protocol for architecture validation
- Domain-Driven Design (bounded contexts, aggregates, entities)
- Clean Architecture layer separation
- Design by Contract (preconditions, postconditions, invariants)
- 12-Factor App considerations
- ISO 27001 security awareness

### Test Coverage
- Edge cases: concurrency, race conditions, injection attacks, auth errors, DB locks, boundary values
- Stress tests: load, spike, soak, capacity (mandatory before launch)
- Playwright E2E (required when UI exists)
- Resilience testing: circuit breakers, retry behavior, network failures

## Skill Source Code

The source directories contain the full SKILL.md and reference files if you want to customize:

```
sdlc-orchestrator/
├── SKILL.md
└── references/methodology-modes.md

sdlc-requirements-shaper/
├── SKILL.md
└── references/prd-template.md

sdlc-architecture-designer/
├── SKILL.md
└── references/
│   ├── ascii-diagram-conventions.md
│   ├── design-by-contract.md
│   └── design-doc-template.md

sdlc-test-planner/
├── SKILL.md
└── references/
    ├── edge-case-test-patterns.md
    ├── stress-test-patterns.md
    └── test-plan-template.md

sdlc-release-readiness/
├── SKILL.md
└── references/release-checklist-template.md
```

## License

MIT
