# PostgreSQL Database Design Best Practices (Strict)

Last researched: 2026-02-14

Use this reference when designing or changing PostgreSQL schemas.

## Non-Negotiable Rules

1. Model integrity with explicit constraints (`PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `CHECK` where valid).
2. Design indexes from real query patterns; avoid speculative indexing.
3. Use `EXPLAIN (ANALYZE, BUFFERS)` for performance-critical queries before and after changes.
4. Treat lock behavior and transaction isolation as first-class design concerns.
5. Define migration rollback and operational safety for all schema changes.

## Indexing and Query Rules

- Prefer smallest effective index set.
- Use multicolumn indexes only for matching predicate patterns.
- Use partial indexes only when predicate matching is guaranteed by query shape.
- Revalidate plan changes with real data characteristics, not toy datasets.

## Concurrency Rules

- Keep transactions short.
- Acquire locks in consistent order to reduce deadlocks.
- Document required isolation level per critical transaction path.

## Quality Gates

- Constraint coverage documented.
- Query plans captured for critical queries.
- Locking/deadlock risk reviewed.
- Migration up/down tested in non-production environment.

## Researched Authoritative Sources

- PostgreSQL constraints: https://www.postgresql.org/docs/current/ddl-constraints.html
- PostgreSQL indexes (overview): https://www.postgresql.org/docs/current/indexes.html
- PostgreSQL multicolumn indexes: https://www.postgresql.org/docs/current/indexes-multicolumn.html
- PostgreSQL partial indexes: https://www.postgresql.org/docs/current/indexes-partial.html
- PostgreSQL EXPLAIN usage: https://www.postgresql.org/docs/current/using-explain.html
- PostgreSQL explicit locking: https://www.postgresql.org/docs/current/explicit-locking.html
