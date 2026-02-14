# Small PR Slicing and Reviewability (Strict)

Last researched: 2026-02-14

Use this reference to split implementation into smaller, high-signal pull requests that are easier for humans to review quickly and accurately.

## Non-Negotiable Rules

1. Slice work into coherent PR units with one primary purpose per PR.
2. Keep PRs small by default; split before opening if review scope is broad.
3. Include only code required for the slice objective (no opportunistic refactors).
4. Make every PR independently understandable and merge-safe.
5. Include precise reviewer guidance: what changed, why, how to validate.

## Recommended Slice Size Targets

- Preferred: <= 400 net changed LOC (excluding generated files).
- Preferred: <= 12 changed files (excluding generated files).
- Preferred: <= 30 minutes expected review time.

If any threshold is exceeded, split the slice unless a documented exception exists.

## Slice Construction Strategy

Build slices in this order:

1. Contract or schema slice (if needed)
2. Core behavior slice
3. Integration slice
4. Observability and hardening slice
5. Cleanup or refactor slice only when directly required for safety/quality of the feature

Use feature flags or equivalent guards when full feature behavior requires multiple PRs.

## Dormant-First Multi-PR Strategy (Final Wiring Last)

Use this strategy when maximizing number of small PRs while preventing early feature exposure:

1. Mark exactly one final `activation` slice.
2. Mark all earlier slices as `Exposure Level = none`.
3. In non-activation slices:
   - Add data structures, business logic, tests, and internal adapters.
   - Do NOT wire routes, handlers, schedulers, or production entry points.
   - Keep feature flags default `OFF`.
4. In final activation slice:
   - Perform minimal wiring only.
   - Enable exposure through controlled rollout.
   - Include explicit rollback path.
   - Preserve backward compatibility for existing consumers.

Required proof for non-activation slices:

- `Exposure Impact: none` in PR description.
- Validation evidence that behavior is unreachable from production paths.

## PR Description Requirements

Each PR must include:

- Slice goal
- Explicit non-goals
- Test evidence summary
- Risk and rollback notes
- Metrics/alerts impact
- Exposure impact (`none` or `activation`)

## Reviewability Checks

Before requesting review:

- No unrelated code in diff
- Test evidence is reproducible
- Commit history maps cleanly to planned tasks
- Reviewer can understand the change without reading future slices
- For non-activation slices, reviewer can verify no user-visible/runtime-active behavior changed

## Researched Authoritative Sources

- Google Engineering Practices, Small CLs: https://google.github.io/eng-practices/review/developer/small-cls.html
- Google Engineering Practices, CL Description: https://google.github.io/eng-practices/review/developer/cl-descriptions.html
- Google Engineering Practices, How to Handle Reviewer Comments: https://google.github.io/eng-practices/review/developer/handling-comments.html
- Google Engineering Practices, Reviewer Standard: https://google.github.io/eng-practices/review/reviewer/standard.html
- GitHub Docs, About Pull Requests: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-with-pull-requests/about-pull-requests
- GitHub Docs, Pull Request Templates: https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository
- GitHub Docs, Protected Branches: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches
