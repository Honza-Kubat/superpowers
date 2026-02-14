# DynamoDB Table Design Best Practices (Strict)

Last researched: 2026-02-14

Use this reference when designing DynamoDB tables and indexes.

## Non-Negotiable Rules

1. Start with access patterns; schema follows access patterns, not entity-first normalization.
2. Use high-cardinality partition keys to prevent hot partitions.
3. Use sort-key modeling patterns deliberately (hierarchies, time, entity grouping).
4. Treat GSIs as critical cost/performance architecture, not as ad-hoc fixes.
5. Validate throughput, consistency, and failure behavior under expected load.

## Required Data Modeling Rules

- Define key schema from query requirements first.
- Use known building blocks where appropriate:
  - composite sort key
  - sparse indexes
  - TTL and archival patterns
  - write sharding for hot key mitigation
  - multi-tenancy isolation strategy
- Minimize table count unless strong isolation constraints require otherwise.

## Operability Rules

- Define capacity mode and autoscaling strategy.
- Define item size limits and offloading strategy for large blobs.
- Define alarm thresholds for throttling, latency, and error rates.

## Quality Gates

- Access-pattern matrix reviewed against schema.
- Partition hot-spot risk reviewed.
- GSI cost/write amplification reviewed.
- Load and failure-mode checks documented.

## Researched Authoritative Sources

- DynamoDB table design best practices: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-table-design.html
- DynamoDB data modeling building blocks: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/data-modeling-blocks.html
- AWS prescriptive DynamoDB best practices: https://docs.aws.amazon.com/prescriptive-guidance/latest/dynamodb-data-modeling/best-practices.html
- DynamoDB developer guide index: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html
