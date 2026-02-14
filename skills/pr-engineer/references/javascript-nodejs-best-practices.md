# JavaScript with Node.js Best Practices (Strict)

Last researched: 2026-02-14

Use this reference when implementation targets JavaScript or TypeScript on Node.js runtime.

## Non-Negotiable Rules

1. Handle errors explicitly; prefer stable identifiers like `error.code` for classification.
2. Enforce Node security best practices in production configurations.
3. Use deterministic automated testing with Node's test runner or equivalent framework.
4. Add structured diagnostics hooks for critical paths.
5. Disallow unsafe production defaults (e.g., insecure inspector usage in production).

## Reliability and Safety Rules

- Define timeout/retry behavior for network and I/O boundaries.
- Fail fast on unhandled fatal conditions.
- Keep dependency versions pinned via lockfiles and CI reproducibility.

## Observability Rules

- Emit structured logs with request/correlation IDs.
- Publish internal diagnostics events for key lifecycle operations.
- Capture latency and failure metrics for critical endpoints and jobs.

## Quality Gates

- Lint/typecheck/tests pass.
- Security checks pass (dependency and config scans where available).
- Error paths tested (including async failures).
- Performance-sensitive paths have regression checks.

## Researched Authoritative Sources

- Node.js security best practices: https://nodejs.org/en/learn/getting-started/security-best-practices
- Node.js test runner API: https://nodejs.org/api/test.html
- Node.js test runner guide: https://nodejs.org/en/learn/test-runner/using-test-runner
- Node.js errors API: https://nodejs.org/api/errors.html
- Node.js diagnostics channel API: https://nodejs.org/api/diagnostics_channel.html
