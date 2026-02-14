# Ruby Best Practices (Strict)

Last researched: 2026-02-14

Use this reference when implementation targets Ruby applications or libraries.

## Non-Negotiable Rules

1. Pin and commit dependency resolution (`Gemfile.lock`) for reproducible builds.
2. Use deployment-safe Bundler mode in production flows.
3. Keep static analysis enabled (RuboCop and relevant cops) and fail CI on high-severity issues.
4. Treat Ruby warnings and deprecations as actionable items, not noise.
5. Enforce explicit tests for behavior and regressions; no untested production changes.

## Required Dependency and Build Discipline

- Commit `Gemfile` and `Gemfile.lock`.
- Use deployment install patterns and frozen lock behavior in CI/CD.
- Avoid implicit dependency drift across environments.

## Quality Gates

- Lint/style checks pass.
- Test suite passes with deterministic setup.
- Security checks on dependencies pass (or documented risk acceptance).
- Logging and metrics hooks exist for critical flows.

## Style and Maintainability Guidance

- Keep methods focused and small.
- Make side effects explicit.
- Use clear exception handling with context-rich messages.
- Keep public APIs narrow and documented.

## Researched Authoritative Sources

- Ruby docs portal: https://docs.ruby-lang.org/
- Ruby documentation guide: https://docs.ruby-lang.org/en/master/contributing/documentation_guide_md.html
- Bundler in applications: https://bundler.io/guides/using_bundler_in_applications.html
- Bundler deployment guide: https://bundler.io/guides/deploying.html
- Bundler config manual: https://bundler.io/man/bundle-config.1.html
- RubyGems security guide: https://guides.rubygems.org/security/
- RuboCop docs: https://docs.rubocop.org/rubocop/
- Ruby Style Guide (community standard): https://rubystyle.guide/
