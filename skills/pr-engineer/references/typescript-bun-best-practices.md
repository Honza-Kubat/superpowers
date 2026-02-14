# TypeScript with Bun Runtime Best Practices (Strict)

Last researched: 2026-02-14

Use this reference when implementation targets TypeScript running on Bun.

## Non-Negotiable Rules

1. Use Bun-native TypeScript setup and install `@types/bun`.
2. Use strict TypeScript compiler settings; do not disable strictness to "make builds pass".
3. Enforce reproducible dependency installs with `bun.lock` and `bun install --frozen-lockfile` in CI.
4. Use Bun test coverage and fail builds on low coverage using explicit thresholds.
5. Prefer isolated installs in monorepos/workspaces to prevent phantom dependencies.

## Required Configuration Baseline

- `tsconfig.json` aligned with Bun recommended options:
  - `target: ESNext`
  - `moduleResolution: bundler`
  - `allowImportingTsExtensions: true`
  - `verbatimModuleSyntax: true`
  - `strict: true`
  - `noUncheckedIndexedAccess: true`
  - `noImplicitOverride: true`
- `bunfig.toml` test policy:
  - `coverage = true`
  - `coverageThreshold` set per repo policy (minimum 0.8 unless stricter)

## Quality Gates

- Tests: `bun test` must pass.
- Coverage: `bun test --coverage` must meet threshold.
- Install determinism: CI must use `bun install --frozen-lockfile`.
- Monorepo hygiene: isolated installs enabled for workspaces when applicable.

## Observability Guidance

- Use structured logging with request/correlation IDs.
- Emit success/error/latency metrics for critical operations.
- For distributed systems, propagate trace context across boundaries.

## Researched Authoritative Sources

- Bun TypeScript docs: https://bun.sh/docs/runtime/typescript
- Bun guide for TypeScript declarations: https://bun.sh/docs/guides/runtime/typescript
- Bun test coverage docs: https://bun.sh/docs/test/coverage
- Bun coverage threshold guide: https://bun.sh/docs/guides/test/coverage-threshold
- Bun isolated installs: https://bun.sh/docs/pm/isolated-installs
- Bun install and frozen lockfile: https://bun.sh/docs/pm/cli/install
- TypeScript Handbook (official): https://www.typescriptlang.org/docs/
