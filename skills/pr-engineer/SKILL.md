---
name: pr-engineer
description: Use when shipping engineering changes quickly with strong quality controls, semantic commit governance, small reviewable PRs, and production-safe rollout while avoiding heavyweight process overhead.
---

# PR Engineer (Fast to Production)

Ship production-ready code fast with AI while keeping quality, safety, and reviewability high.

## Priority Order (Strict)

When tradeoffs conflict, resolve in this exact order:

1. **Quality** (highest)
2. High number of PRs per week
3. Simplicity
4. No unnecessary changes
5. Backward compatibility for additions

**Rule:** Lower-priority goals must never reduce a higher-priority goal.

## Non-Negotiable Rules

- **Quality first:** Merged code must pass required tests and checks
- **Plan non-trivial work:** For 3+ steps or architecture decisions, enter plan mode
- **Semantic commits:** One logical change per commit with concise title + mandatory body
- **No history rewriting:** `rebase`, `commit --amend`, force push forbidden for implementation commits
- **No unapproved PRs:** Do not open any PR (including draft) without explicit user approval
- **No unapproved merges:** Do not merge any PR without explicit user approval
- **No committed workflow docs:** Workflow-generated Markdown artifacts must not be committed
- **No premature activation:** Non-final PRs must not expose runtime-visible functionality
- **Root-cause fixes:** Prefer root-cause fixes over temporary patches
- **Minimal changes:** Keep changes minimal; avoid broad/unnecessary edits
- **Backward compatible:** New additions must be backward compatible by default

## Red Flags - STOP

If you catch yourself thinking these, you're about to violate the rules:

| Red Flag | Reality |
|----------|---------|
| "Just this once" | Rules exist because violations compound |
| "Being pragmatic vs dogmatic" | Pragmatism doesn't mean skipping quality |
| "Following spirit not letter" | Violating letter IS violating spirit |
| "User said it's okay" | You're responsible for enforcing rules |
| "This case is different" | Every violation thinks it's special |
| "No time to plan" | Planning saves time, not wastes it |
| "Just open the PR" | Explicit approval required, not casual agreement |

## Explicit User Approval

**Explicit approval means:**
- "Yes, open the PR"
- "Go ahead and create the PR"
- "Approved for PR"
- "Open the PR now"

**NOT explicit approval:**
- "Looks good"
- "I trust you"
- "Just do it"
- "Seems fine"

**When in doubt:** Ask explicitly: "Should I open the PR now?"

## Quick Reference

| Aspect | Standard Tier | Critical Tier |
|--------|---------------|---------------|
| **Risk level** | Default | Security/privacy/data migrations/external contracts/financial impact |
| **Verification** | Lint + types + unit tests + integration tests | + security review + performance check + resilience test |
| **PR size** | <= 400 net LOC, <= 12 files | Same (never larger) |
| **Slicing** | Dormant-first, one activation slice last | Same (mandatory) |
| **Backward compat** | Required for additions | Required for additions |

## Workflow Stages

### Stage 1: Scope and Slice

**Goal:** Define minimum viable implementation and review slices

**Actions:**
1. Capture scope, non-goals, risk tier, acceptance criteria
2. Build slice plan with dormant-first approach:
   - All slices S1..Sn-1: Exposure Impact = `none`
   - Final slice Sn: Exposure Impact = `activation`
3. Keep slices small (<= 400 LOC, <= 12 files)
4. Check in before implementation begins

**Gate:** Every task belongs to one slice. Activation slice is last.

### Stage 2: Build and Test

**Goal:** Implement quickly with sufficient confidence

**Rules:**
- Bug fixes: write failing regression test first
- Behavior changes: add/update unit tests
- Keep non-activation slices non-exposing (no live routes, feature flags OFF)
- Commit frequently by logical change
- Update task-commit-map after each commit

**Gate:** Code works, edge cases covered, non-activation slices unreachable.

### Stage 3: Lean Verification

**Goal:** Verify quality without excessive delay

**Required checks:**
- Lint + type/static checks
- Unit tests for changed logic
- Integration tests when interfaces changed
- Build/package validation
- Behavior diff vs `main` when expected
- Backward-compatibility checks

**Critical tier adds:**
- Security/privacy review
- Performance regression check
- Resilience/failure-path check

**Gate:** All checks pass. Test evidence documented. "Would a staff engineer approve this?"

### Stage 4: Review Package and User Approval

**For each slice:**
1. Prepare reviewer package (goal, non-goals, tests, risk, rollback notes)
2. **Ask user explicitly for PR creation approval**
3. Create PR **only after explicit approval**

**Gate:** Approval recorded. PR title/body follow governance rules.

### Stage 5: Activation Slice

**Goal:** Expose functionality only in final slice

**Rules:**
- Final slice performs minimal wiring
- Include rollout controls and rollback steps
- No unrelated refactors

**Gate:** Activation slice is last and focused.

### Stage 6: Release

**Goal:** Safe and fast production rollout

**Actions:**
1. Progressive exposure (canary/partial)
2. Monitor baseline alert metrics
3. Roll back immediately on abort threshold breach

**If deployment unavailable:** Set status `RELEASE_PENDING`, keep plan ready

**Non-runtime changes:** Set status `RELEASE_NOT_APPLICABLE`

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Opening PR without approval | Always ask explicitly, wait for "yes, open the PR" |
| Committing workflow docs | Keep `.md` files in `.local/` directory |
| One giant PR | Slice into <= 400 LOC chunks, dormant-first |
| Breaking backward compat | Add new fields/endpoints, don't remove old ones |
| Skipping plan mode | Non-trivial = 3+ steps or architecture decisions |
| Bundling refactors with features | Keep slices focused on single logical change |
| Premature activation | All non-final slices must have Exposure = `none` |

## Workflow Artifacts (Local Only)

Store in `.local/pr-engineer/<yyyy-mm-dd-feature>/`

**Required files:**
- `delivery-plan.md`
- `pr-slice-plan.md`
- `implementation-plan.md`
- `test-evidence.md`
- `release-rollout-plan.md`
- `task-commit-map.md`
- `lessons-learned.md`

**CRITICAL:** These files must NEVER be committed to the repository.

**Check before every commit:** Ensure workflow `.md` files are not staged. If staged, unstage immediately.

## Risk Tiers

Assign one in the plan:
- `standard`: Default for most internal or low/medium blast-radius changes
- `critical`: Security/privacy-sensitive, data migrations, external contracts, financial/compliance impact

**Rule:** If uncertain, use `critical`.

## References

**Always use:**
- `references/semantic-commits-and-pr-governance.md`
- `references/small-pr-slicing-and-reviewability.md`
- `references/production-alert-metrics-catalog.md`

**Critical tier adds:**
- `references/bigtech-production-readiness-best-practices.md`
- Stack-specific references (Go, AWS, Bun/TS, Node.js, Ruby/Rails, Postgres, DynamoDB, Kafka)

**Detailed workflow guidance:**
- `references/workflow-details.md` (comprehensive stage-by-stage instructions)

## Done Definition

Status can be `RELEASED` (or `RELEASE_NOT_APPLICABLE`) only when:

- [ ] Required checks for risk tier pass
- [ ] Slice PR approvals are explicit
- [ ] Activation happened only in final slice
- [ ] Rollback path defined
- [ ] Backward compatibility preserved
- [ ] Workflow Markdown files NOT committed
- [ ] Implementation plan has progress and review notes
- [ ] Lessons learned updated (if corrections occurred)
