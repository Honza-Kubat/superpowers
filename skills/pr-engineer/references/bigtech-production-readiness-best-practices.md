# BigTech Production Readiness Best Practices (Strict Baseline)

Last researched: 2026-02-14

Use this reference for every production-bound engineering change, regardless of language/runtime.

## Non-Negotiable Baseline

1. Treat production readiness as a formal gate, not an informal judgment.
2. Define SLI/SLO and error-budget expectations before implementation.
3. Require progressive rollout with explicit abort and rollback triggers.
4. Require observability triad coverage for critical flows: logs, metrics, traces.
5. Require threat modeling and security review for changed trust boundaries.
6. Require privacy/data-governance review for sensitive or regulated data paths.
7. Require incident readiness: playbooks, ownership, and escalation paths.
8. Require post-launch validation and follow-up ownership.
9. Scale gate depth by risk tier (`standard` / `critical`) while preserving mandatory controls.
10. Allow exceptions only with documented approver, expiry, compensating controls, and follow-up.
11. Backward compatibility for additions is non-negotiable and not waiveable in this workflow.

## BigTech-Style Readiness Checklist

Use this checklist during architecture review and pre-launch verification.

- Specification quality:
  - Goals and non-goals are explicit.
  - Success criteria are objective and measurable.
  - SLI/SLO targets are defined for impacted critical paths.
- Architecture quality:
  - Contracts are explicit and versioning strategy is defined.
  - Capacity assumptions and failure modes are documented.
  - Rollout and rollback design is documented.
  - Additions remain backward compatible.
- Security and privacy:
  - Threat model updated for changed boundaries.
  - Security controls mapped to threats.
  - Data classification/retention/deletion controls documented.
- Verification rigor:
  - Unit/integration/contract/e2e tests pass when applicable.
  - Static/security scans meet policy.
  - Performance and resilience checks meet spec-defined targets.
- Operability:
  - Dashboards and alerts are updated.
  - On-call runbook includes diagnosis and mitigation steps.
  - Post-launch validation and incident response procedures are explicit.
- Governance:
  - CI enforces commit/PR/policy checks.
  - Exceptions are time-bound and tracked.
  - Post-release KPIs are measured and reviewed.
  - Reviewability standards enforce small, focused PR slices.

## Strict Outcome Rule

If any mandatory item is incomplete, status remains `NOT READY`.

## Researched Authoritative Sources

- Google SRE Book (official): https://sre.google/sre-book/table-of-contents/
- SRE Workbook (official): https://sre.google/workbook/table-of-contents/
- Google SRE: Service Level Objectives chapter: https://sre.google/sre-book/service-level-objectives/
- Google SRE: Monitoring Distributed Systems chapter: https://sre.google/sre-book/monitoring-distributed-systems/
- NIST Secure Software Development Framework (SP 800-218): https://csrc.nist.gov/pubs/sp/800/218/final
- OWASP ASVS project: https://owasp.org/www-project-application-security-verification-standard/
- OpenTelemetry docs: https://opentelemetry.io/docs/
- OpenTelemetry specification: https://opentelemetry.io/docs/specs/otel/
- SLSA framework and specification: https://slsa.dev/
- DORA research program: https://dora.dev/
- Microsoft Security Development Lifecycle: https://www.microsoft.com/en-us/securityengineering/sdl
