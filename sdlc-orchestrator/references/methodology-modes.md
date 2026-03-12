# Methodology-Specific Modes

This reference defines how the SDLC workflow adapts based on the user's chosen methodology. The core phases remain the same, but cadence, artifact depth, and gate formality change.

---

## Agile Mode

**Cadence:** Iterative sprints (1-2 weeks). Phases may overlap. Requirements evolve.

**Artifact depth:** Lightweight. Prefer working software over comprehensive documentation.
- PRD: brief, focused on user stories and acceptance criteria
- Design doc: architecture summary + key decisions, not exhaustive
- Test plan: test strategy summary + key test cases, not exhaustive matrix
- Release checklist: streamlined, focused on ship-readiness

**Review gates:**
- G1 (Requirements): Quick confirmation — "Does this scope look right?" User stories + ACs shared, verbal/written OK.
- G2 (Architecture): ASCII diagram + brief discussion. Confirm boundaries and contracts.
- G3 (Readiness): Sprint planning confirmation. Implementation plan fits in sprint.
- G4 (Release): Demo + ship decision. Quick checklist review.

**Iteration pattern:** Each sprint may touch multiple phases. Refinement is continuous.

**When to use:** Product teams with evolving requirements, frequent releases, collaborative environments.

---

## Waterfall Mode

**Cadence:** Sequential phases. Each phase completes before the next begins. Formal transitions.

**Artifact depth:** Comprehensive. Each phase produces formal, reviewable documents.
- PRD: full document with all sections populated
- Design doc: complete technical specification with all architecture views
- Test plan: comprehensive test matrix with full traceability
- Release checklist: formal with sign-off rows for each stakeholder

**Review gates:**
- G1 (Requirements): Formal requirements review meeting. Written sign-off required.
- G2 (Architecture): Formal design review. Architecture review board if applicable. Written approval.
- G3 (Readiness): Formal readiness review. Test plan approved. Implementation plan approved.
- G4 (Release): Formal release approval. All sign-offs collected. Go/no-go decision documented.

**Iteration pattern:** Minimal. Changes after gate approval require formal change requests.

**When to use:** Regulated environments, compliance-heavy projects, projects with fixed scope and timeline, formal approval processes.

---

## Iterative Mode

**Cadence:** Phased delivery cycles (2-4 weeks). Each cycle delivers a working increment. Engineering-first.

**Artifact depth:** Moderate. Practical documentation that serves engineering needs.
- PRD: focused on current iteration scope + overall vision
- Design doc: architecture + key contracts, updated each iteration
- Test plan: test strategy + key scenarios, grows with each iteration
- Release checklist: practical, focused on operational readiness

**Review gates:**
- G1 (Requirements): Written confirmation. Scope for current iteration locked.
- G2 (Architecture): ASCII diagram + written confirmation. Architecture evolves across iterations.
- G3 (Readiness): Milestone confirmation. Current iteration's implementation and test plan confirmed.
- G4 (Release): Readiness checklist reviewed and confirmed.

**Iteration pattern:** Each cycle refines and extends. Architecture may evolve. Earlier iterations may be MVPs.

**When to use:** Engineering teams building complex systems incrementally, platform work, infrastructure projects.

---

## Custom Mode

**Cadence:** User-defined. Ask the user to describe their process.

**Artifact depth:** User-defined. Ask what artifacts they need and at what depth.

**Review gates:** Map the user's existing checkpoints to G1-G4. If they have more or fewer gates, adapt.

**How to handle:**
1. Ask the user to describe their delivery process
2. Map their process to the 8 SDLC phases
3. Identify which phases they emphasize and which they skip
4. Adapt artifact depth and gate formality accordingly
5. Confirm the adapted workflow before proceeding

**When to use:** Teams with established internal processes that don't map cleanly to standard methodologies.

---

## Methodology Comparison at a Glance

| Aspect | Agile | Waterfall | Iterative | Custom |
|--------|-------|-----------|-----------|--------|
| Phase overlap | High | None | Moderate | Varies |
| Artifact depth | Light | Heavy | Moderate | Varies |
| Gate formality | Informal | Formal | Moderate | Varies |
| Change tolerance | High | Low | Moderate | Varies |
| Documentation | Minimal viable | Comprehensive | Practical | Varies |
| Best for | Product teams | Regulated/fixed | Platform/infra | Established process |
