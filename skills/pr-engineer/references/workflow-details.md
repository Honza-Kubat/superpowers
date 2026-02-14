# PR Engineer Workflow Details

**Load this reference when:** You need detailed guidance on specific workflow stages, orchestration patterns, or comprehensive checklists beyond the main skill.

This reference provides expanded details for the pr-engineer workflow. The main SKILL.md contains all essential rules; this file provides additional context and detailed guidance.

## Workflow Orchestration Alignment

### Plan Mode Default

- Enter plan mode for any non-trivial task (3+ steps or architecture decisions)
- If execution deviates or assumptions break, stop and re-plan
- Use plan mode for verification as well as implementation
- Write specification-level clarity before coding

### Subagent Strategy

- Use subagents liberally to keep context focused
- Offload research, exploration, and parallel analysis to subagents
- For complex tasks, increase parallel subagent usage
- Keep one task per subagent where feasible

### Self-Improvement Loop

- After any user correction, update `lessons-learned.md` with:
  - The correction pattern
  - A prevention rule
- Review relevant lessons at session start for this project
- Iterate lessons until repeated mistakes are eliminated

### Verification Before Done

- Never mark complete without proof
- Compare behavior against base branch (`main`) when relevant
- Ask: "Would a staff engineer approve this?"
- Run tests and checks, and show correctness evidence

### Demand Elegance (Balanced)

- For non-trivial changes, evaluate whether a simpler/elegant solution exists
- If solution feels hacky, redesign before finalizing
- For simple obvious fixes, avoid over-engineering

### Autonomous Bug Fixing

- For bug reports, diagnose and fix end-to-end without hand-holding
- Start from evidence (logs/errors/failing tests), then implement fix
- Minimize user context-switching
- Treat failing CI tests as direct actionable work

## Task Management Alignment

1. **Plan first:** Write checkable plan items in `implementation-plan.md`
2. **Verify plan:** Check in before implementation
3. **Track progress:** Mark items complete as work advances
4. **Explain changes:** Record high-level step summaries
5. **Document results:** Add review section in `implementation-plan.md`
6. **Capture lessons:** Update `lessons-learned.md` after corrections

**Note:** `implementation-plan.md` and `lessons-learned.md` are local operational tracking artifacts in the workflow directory and must not be committed unless user explicitly requests otherwise.

## Core Principles Alignment

- **Simplicity first:** Smallest effective change
- **No laziness:** Solve root causes, not temporary symptoms
- **Minimal impact:** Touch only necessary code paths to reduce regression risk

## Detailed Stage Guidance

### Stage 1: Scope and Slice - Detailed

**Dormant-First Slicing Strategy:**

The key principle is that all functionality is built in a "dormant" state across multiple PRs, then "activated" in a single final PR. This allows for:

- Safe, incremental merges
- Thorough review of each component
- Easy rollback at any stage
- Final activation with minimal risk

**Slicing Pattern:**

```
S1: Database schema + model (Exposure: none)
S2: Business logic + unit tests (Exposure: none)
S3: API endpoints + integration tests (Exposure: none)
S4: Feature flag + route wiring (Exposure: activation)
```

**Backward-Compatibility Strategy for Additions:**

When adding new interfaces/schemas:
1. **Additive approach:** Add new fields/endpoints, don't remove old ones
2. **Deprecation path:** If removing old behavior, add deprecation notice first
3. **Compatibility tests:** Write tests that verify old consumers still work
4. **Migration plan:** Document migration steps for consumers

**Gate Checklist:**
- [ ] Every task belongs to exactly one slice
- [ ] Activation slice is identified and last
- [ ] All non-activation slices have Exposure Impact = `none`
- [ ] Slice sizes are <= 400 LOC and <= 12 files
- [ ] Backward-compatibility plan exists for all new additions
- [ ] User has approved the slice plan before implementation

### Stage 2: Build and Test - Detailed

**TDD Approach:**

For bug fixes:
1. Write failing regression test that reproduces the bug
2. Verify test fails for the right reason
3. Implement minimal fix
4. Verify test passes
5. Add edge case tests

For behavior changes:
1. Write/update unit tests for changed logic
2. Verify tests capture the new behavior
3. Implement changes
4. Verify tests pass

**Backward Compatibility Preservation:**

When adding new features:
- Add new fields to schemas (don't remove old ones)
- Add new endpoints (don't modify existing ones in breaking ways)
- Add new parameters with defaults (don't make existing parameters required)
- Keep existing consumers working

**Non-Exposing Slices:**

To ensure non-activation slices remain unreachable:
- Don't wire routes to handlers
- Don't enable schedulers/cron jobs
- Set feature flags to default `OFF`
- Don't add UI elements that trigger the feature
- Don't update configuration to enable the feature

**Gate Checklist:**
- [ ] Code works for intended behavior
- [ ] Edge cases for changed logic are covered by tests
- [ ] Non-activation slices remain unreachable in production paths
- [ ] Backward compatibility of additions is validated
- [ ] Each commit represents one logical change
- [ ] Task-commit-map is updated after each commit

### Stage 3: Lean Verification - Detailed

**Standard Tier Verification:**

1. **Lint:** Run linter and fix all issues
2. **Type/static checks:** Run type checker or static analysis
3. **Unit tests:** Run unit tests for changed logic
4. **Integration tests:** Run integration tests when interfaces changed
5. **Build/package:** Verify build succeeds, package is valid
6. **Behavior diff:** Compare behavior vs `main` when expected
7. **Backward compat:** Verify old consumers still work

**Critical Tier Additional Checks:**

1. **Security/privacy review:**
   - Input validation
   - Authentication/authorization
   - Data exposure
   - Injection vulnerabilities
   - Sensitive data handling

2. **Performance regression:**
   - Benchmark critical paths
   - Compare vs baseline
   - Identify degradation

3. **Resilience/failure-path:**
   - Test error handling
   - Test timeout behavior
   - Test partial failure scenarios
   - Test rollback procedures

**Evidence Documentation:**

In `test-evidence.md`, record:
- Commands run
- Output/results
- Pass/fail status
- Any anomalies observed
- Behavior diff notes

**Gate Checklist:**
- [ ] All required checks pass for the risk tier
- [ ] `test-evidence.md` has commands and outcomes
- [ ] Verification notes answer: "Would a staff engineer approve this?"
- [ ] No unnecessary or unrelated changes remain in the slice diff
- [ ] All linter warnings resolved
- [ ] All test failures investigated and resolved

### Stage 4: Review Package and User Approval - Detailed

**Reviewer Package Contents:**

For each slice, prepare:

1. **Slice goal:** What this PR accomplishes
2. **Explicit non-goals:** What this PR deliberately does NOT do
3. **Tests run:** List of tests with results
4. **Risk assessment:** Risk tier + specific risks
5. **Rollback notes:** How to revert if problems
6. **Exposure impact:** `none` or `activation`
7. **Backward-compatibility impact:** What changes for existing consumers
8. **Verification evidence:** Link to test-evidence.md

**Approval Gate Process:**

1. Present reviewer package to user
2. **Ask explicitly:** "Should I open the PR now?"
3. Wait for explicit approval (see main skill for examples)
4. Only after approval: create PR with proper title/body

**PR Description Sections:**

Required sections in PR description:
- Feature Description
- Slice Goal
- Implementation Summary
- Tests Run
- Production Readiness Checklist
- Alert Metrics
- Exposure Impact (`none` or `activation`)
- Rollback Plan
- Backward Compatibility
- Task-to-Commit Mapping

**Gate Checklist:**
- [ ] Reviewer package prepared
- [ ] User explicitly asked for approval
- [ ] Explicit approval received (not just "looks good")
- [ ] Approval recorded
- [ ] PR title/body follow governance rules
- [ ] All required PR description sections included

### Stage 5: Activation Slice - Detailed

**Characteristics of Activation Slice:**

- **Minimal:** Only wiring/routing code
- **Focused:** Single purpose (enable the feature)
- **Well-tested:** Even wiring needs tests
- **Documented:** Clear rollout/rollback instructions
- **Monitored:** Alert metrics defined

**What NOT to Bundle:**

- Refactors (do in separate PRs)
- Unrelated features
- "While I'm here" changes
- Documentation updates (unless directly related)

**Rollout Controls:**

Include in activation slice:
- Feature flags (if applicable)
- Configuration to enable/disable
- Monitoring/alerting setup
- Rollback procedure documentation

**Gate Checklist:**
- [ ] Activation slice is the final slice
- [ ] Slice contains only wiring/routing code
- [ ] No unrelated refactors bundled
- [ ] Rollout controls included
- [ ] Rollback steps documented
- [ ] Feature flags default to `OFF` (if used)

### Stage 6: Release - Detailed

**Progressive Exposure Strategy:**

1. **Canary:** Deploy to 1-5% of traffic
2. **Monitor:** Watch metrics for 10-30 minutes
3. **Expand:** Gradually increase to 25%, 50%, 100%
4. **Abort threshold:** Define metrics that trigger immediate rollback

**Baseline Alert Metrics:**

For production-impacting slices, define thresholds + owners for:
- **Latency (p95):** Acceptable response time
- **Error rate:** Acceptable error percentage
- **Throughput/traffic:** Expected request volume
- **Saturation:** Resource utilization (CPU, memory, connections)
- **Domain/business KPI:** One metric specific to the feature

**Rollback Procedure:**

1. Define rollback trigger (metric threshold)
2. Document rollback steps
3. Test rollback in staging (if possible)
4. Have rollback command ready
5. Set monitoring alerts
6. Establish decision-maker for rollback

**Handling Deployment Unavailability:**

If deployment system is unavailable:
1. Set status: `RELEASE_PENDING`
2. Keep release plan documented
3. Keep rollback details ready
4. Monitor for deployment system recovery
5. Proceed when available

**Non-Runtime Changes:**

For changes that don't require deployment:
1. Record rationale for why release is N/A
2. Set status: `RELEASE_NOT_APPLICABLE`
3. Document in release-rollout-plan.md
4. Ensure no runtime impact

**Gate Checklist:**
- [ ] Progressive exposure plan defined
- [ ] Alert metrics defined with thresholds
- [ ] Rollback procedure documented
- [ ] Rollback trigger defined
- [ ] Monitoring alerts configured
- [ ] Status updated appropriately

## Minimal Alert Metrics Baseline

For production-impacting slices, define:

| Metric | Threshold | Owner | Action |
|--------|-----------|-------|--------|
| Latency (p95) | < 200ms | Platform | Rollback if > 500ms |
| Error rate | < 0.1% | Platform | Rollback if > 1% |
| Throughput | > 1000 req/s | Platform | Investigate if < 500 |
| CPU saturation | < 70% | Platform | Scale if > 85% |
| Business KPI | [Feature-specific] | Product | [Feature-specific] |

## Common Anti-Patterns

### Anti-Pattern: "Big Bang" PR

**Symptom:** One PR with 2000+ LOC, 50+ files
**Problem:** Hard to review, high risk, difficult to rollback
**Fix:** Slice into multiple PRs using dormant-first approach

### Anti-Pattern: Premature Activation

**Symptom:** Feature accessible in production before final slice
**Problem:** Broken/incomplete feature exposed to users
**Fix:** Keep all non-final slices with Exposure = `none`

### Anti-Pattern: Skipping Plan Mode

**Symptom:** Jumping straight to code for "simple" features
**Problem:** Miss edge cases, poor architecture, rework needed
**Fix:** Always plan non-trivial work (3+ steps or architecture decisions)

### Anti-Pattern: Backward Compatibility Violation

**Symptom:** Removing fields, changing signatures, breaking existing consumers
**Problem:** Breaks existing integrations, causes production incidents
**Fix:** Use additive approach, deprecation path, compatibility tests

### Anti-Pattern: Bundling Refactors

**Symptom:** "While I'm here, let me also clean up..."
**Problem:** Mixes risk profiles, harder to review, harder to rollback
**Fix:** Keep refactors in separate PRs

## Lessons Learned Template

In `lessons-learned.md`, use this format:

```markdown
## [Date] - [Issue Description]

**What happened:**
[Brief description of the mistake or issue]

**Why it happened:**
[Root cause analysis]

**Prevention rule:**
[Specific rule or check to prevent recurrence]

**Example:**
[Concrete example of applying the prevention rule]
```

**Example:**

```markdown
## 2025-01-15 - Opened PR without explicit approval

**What happened:**
User said "looks good" and I opened the PR immediately.

**Why it happened:**
I interpreted casual approval as explicit approval.

**Prevention rule:**
Always ask explicitly "Should I open the PR now?" and wait for a "yes" response.

**Example:**
User: "Looks good!"
Me: "Should I open the PR now?"
User: "Yes, open the PR."
Me: [Opens PR]
```

## Workflow Directory Naming Convention

**Directory:** `.local/pr-engineer/<yyyy-mm-dd-feature>/`

**Examples:**
- `.local/pr-engineer/2025-01-15-user-authentication/`
- `.local/pr-engineer/2025-01-20-payment-processing/`
- `.local/pr-engineer/2025-01-25-api-rate-limiting/`

**Rules:**
- Use lowercase kebab-case for all directory and file names
- Include date for chronological ordering
- Use descriptive feature name
- Keep all workflow files in this directory
- NEVER commit this directory to the repository
