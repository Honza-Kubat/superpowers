# Ruby on Rails Best Practices (Strict)

Last researched: 2026-02-14

Use this reference when implementation targets Rails applications.

## Non-Negotiable Rules

1. Enforce strong parameter handling for mass assignment safety.
2. Keep database integrity in the database with keys/indexes/constraints where applicable.
3. Instrument important application paths using Active Support notifications.
4. Follow Rails security guidance for sessions, CSRF, and injection protections.
5. Test controller/model/integration behavior for all changed critical flows.

## Required Architecture and Data Rules

- Use reversible, explicit migrations.
- Keep schema artifacts in source control.
- Prefer explicit referential integrity strategy (DB constraints + app validations where needed).
- Document migration rollback path for every non-trivial change.

## Observability Rules

- Use Rails logger with structured context where possible.
- Subscribe to or emit instrumentation events for key operations.
- Track query counts/timings in performance-sensitive flows.

## Quality Gates

- Rails test suite passes for changed areas.
- Migration up/down path validated for changed schema.
- Security-sensitive paths have explicit tests.
- Performance-sensitive endpoints checked for N+1 and query bloat.

## Researched Authoritative Sources

- Active Support Instrumentation guide: https://guides.rubyonrails.org/active_support_instrumentation.html
- Action Controller overview (Strong Parameters): https://guides.rubyonrails.org/action_controller_overview.html
- Active Record Migrations guide: https://guides.rubyonrails.org/active_record_migrations.html
- Rails security guide: https://guides.rubyonrails.org/security.html
- Rails testing guide: https://guides.rubyonrails.org/testing.html
