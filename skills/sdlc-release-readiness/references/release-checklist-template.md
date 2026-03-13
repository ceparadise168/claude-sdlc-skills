# Release Checklist Template

Use this template when generating a release checklist. Adapt to the feature size and methodology — not every item applies to every release.

---

# Release Checklist: [Feature Name]

**Release Date:** [Date]
**Release Owner:** [Name]
**Methodology:** [Agile | Waterfall | Iterative | Custom]
**Rollout Strategy:** [Feature flag | Canary | Blue-green | Phased | Big bang]

---

## Pre-Release

### Code Readiness
- [ ] All code changes merged to release branch
- [ ] Code review completed and approved
- [ ] No outstanding merge conflicts
- [ ] Feature flags configured (if applicable)

### Testing Readiness
- [ ] All unit tests passing
- [ ] All integration tests passing
- [ ] All contract tests passing
- [ ] E2E tests passing (Playwright, if UI exists)
- [ ] Performance tests passing (if applicable)
- [ ] **Stress tests passing** (load, spike, soak — see stress test report)
- [ ] Capacity ceiling documented and above expected peak with safety margin
- [ ] No memory leaks detected in soak test
- [ ] Security tests passing
- [ ] No open P0/P1 defects
- [ ] Test coverage meets project standards

### Documentation
- [ ] API documentation updated
- [ ] User-facing documentation updated (if applicable)
- [ ] Internal runbook / playbook updated
- [ ] Change log updated

### Architecture and Design
- [ ] Implementation matches confirmed architecture
- [ ] Clean Architecture boundaries preserved
- [ ] Design by Contract specifications implemented
- [ ] Domain boundaries respected (DDD)

### Security and Compliance (ISO 27001 Awareness)
- [ ] Security review completed (if required)
- [ ] Access controls verified
- [ ] Data handling reviewed (classification, encryption)
- [ ] Audit logging in place
- [ ] Compliance requirements met
- [ ] Change traceability documented

### Infrastructure and Operations
- [ ] Infrastructure changes applied (if applicable)
- [ ] Configuration changes deployed (12-Factor: config separation)
- [ ] Database migrations tested and ready
- [ ] Monitoring and alerting configured
- [ ] Log aggregation confirmed
- [ ] Health checks in place

### Dependencies
- [ ] Upstream dependencies deployed and verified
- [ ] External service dependencies confirmed available
- [ ] Backward compatibility verified (if applicable)

---

## Release Execution

### Deployment Steps
1. [ ] [Step 1: e.g., Deploy database migration]
2. [ ] [Step 2: e.g., Deploy backend services]
3. [ ] [Step 3: e.g., Deploy frontend]
4. [ ] [Step 4: e.g., Enable feature flag for canary %]
5. [ ] [Step 5: e.g., Monitor metrics for N minutes]
6. [ ] [Step 6: e.g., Expand rollout]

### Smoke Test
- [ ] Critical user flow 1 verified in production
- [ ] Critical user flow 2 verified in production
- [ ] API health check passing
- [ ] No error rate increase observed

---

## Rollback Plan

### Rollback Trigger Criteria
- Error rate exceeds [threshold]
- Latency exceeds [threshold]
- Critical user flow broken
- Data integrity issue detected

### Rollback Steps
1. [ ] [Step 1: e.g., Disable feature flag]
2. [ ] [Step 2: e.g., Revert deployment]
3. [ ] [Step 3: e.g., Rollback database migration]
4. [ ] [Step 4: e.g., Verify system restored]
5. [ ] [Step 5: e.g., Notify stakeholders]

### Rollback Verification
- [ ] System health confirmed after rollback
- [ ] No data loss or corruption
- [ ] Monitoring back to baseline

---

## Post-Release

### Monitoring (first 24-48 hours)
- [ ] Error rates within normal range
- [ ] Latency within expected bounds
- [ ] No unexpected resource usage
- [ ] User-reported issues triaged

### Validation
- [ ] Success metrics baseline established
- [ ] Feature adoption tracking in place (if applicable)
- [ ] Stakeholders notified of release

### Follow-Up
- [ ] Post-release review scheduled
- [ ] Lessons learned captured
- [ ] Technical debt items logged (if any)
- [ ] Feature flag cleanup scheduled (if applicable)

---

## Sign-Off

| Role | Name | Approved | Date |
|------|------|----------|------|
| Engineering Lead | | [ ] | |
| QA Lead | | [ ] | |
| Product Owner | | [ ] | |
| Security (if required) | | [ ] | |
| Operations (if required) | | [ ] | |
