# Semantic Commits and PR Governance (Fast-Flow Strict)

Last researched: 2026-02-14

Use this reference for commits and pull requests in fast-to-production workflow.

## Non-Negotiable Rules

1. Every commit MUST follow Conventional Commits format.
2. Every commit MUST include:
   - semantic title line,
   - body with broader implementation context.
3. Every commit MUST represent one logical change.
4. PR title MUST follow Conventional Commit format.
5. PR body MUST include required review/production sections.
6. PR creation is forbidden without explicit user approval.
7. Draft PR creation is also forbidden without explicit user approval.
8. History rewrite for implementation commits is forbidden (`rebase`, `commit --amend`, force push).
9. Workflow-generated Markdown artifacts MUST NOT be committed.

## Commit Message Standard

Required format:

```text
<type>(<scope>): <concise summary>

<broader implementation description>

[optional footer(s)]
```

Rules:

- Use semantic `type` (`feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `build`, `ci`, `chore`, `revert` or project-approved types).
- Keep title concise (prefer <= 72 chars), imperative, no trailing period.
- Body explains what changed and why it is correct.
- Include task/slice traceability when applicable.

## PR Title and Body Standard

PR title format:

```text
<type>(<scope>): <concise slice summary>
```

PR body sections (required):

1. `Feature Description`
2. `Slice Goal`
3. `Implementation Summary`
4. `Tests Run`
5. `Production Readiness Checklist`
6. `Alert Metrics` (metric + threshold + owner)
7. `Exposure Impact` (`none` or `activation`)
8. `Rollback Plan`
9. `Task-to-Commit Mapping`
10. `Backward Compatibility`

## PR Approval Gate

Before creating each PR:

1. Present slice evidence summary.
2. Ask explicitly: "Do you approve creating the PR now?"
3. Create PR only if approval is explicit and unambiguous.
4. If approval is missing or unclear, do not create PR.

## Markdown No-Commit Rule

Workflow docs (for example `delivery-plan.md`, `pr-slice-plan.md`, `implementation-plan.md`, `test-evidence.md`, `task-commit-map.md`, `lessons-learned.md`) are local process artifacts stored under `./.local/pr-engineer/<yyyy-mm-dd-feature>/`.

- They MUST NOT be committed.
- They MUST use lowercase kebab-case file names.
- Check staging before commit and unstage workflow `.md` files if present.

## Repository Governance Alignment

Enforce with repository settings and CI:

- Require reviews before merge.
- Require passing status checks.
- Validate semantic PR titles.
- Use PR template with required sections.

## Researched Authoritative Sources

- Conventional Commits 1.0.0: https://www.conventionalcommits.org/en/v1.0.0/
- Git commit docs: https://git-scm.com/docs/git-commit
- Semantic Versioning 2.0.0: https://semver.org/
- GitHub PR templates: https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository
- GitHub protected branches/checks: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches
- Semantic PR title validation action: https://github.com/amannn/action-semantic-pull-request
