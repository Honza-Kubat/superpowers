# Production Alert Metrics Catalog (Strict)

Last researched: 2026-02-14

Use this reference to define mandatory alert metrics in PR descriptions and runbooks.

## Core Alerting Principle

At minimum, monitor and alert on the four golden signals for user-impacting systems:

- Latency
- Traffic
- Errors
- Saturation

## Required PR Metrics Section

Every production-bound PR must list:

1. Metric name
2. Source/system
3. Alert condition/threshold (numeric)
4. Why it detects likely regression
5. Owner/on-call routing
6. Evaluation window and severity level

## Baseline Cross-Stack Metrics

- Request latency (p50/p95/p99) for critical operations
- Request/throughput rate
- Error rate (5xx, domain failures, timeout failures)
- Saturation (CPU, memory, queue depth, connection pools, storage pressure)
- Dependency failure rate and latency (DB, cache, external API)

## Database and Messaging Specific Metrics

### PostgreSQL

- Long-running query rate and latency distribution
- Deadlocks and lock wait pressure
- Table/index scan and I/O pressure indicators
- WAL/checkpointer stress indicators
- Top query classes via `pg_stat_statements`

### DynamoDB

- `ThrottledRequests`
- `ReadThrottleEvents` / `WriteThrottleEvents`
- `SuccessfulRequestLatency`
- `SystemErrors`
- `UserErrors`
- `TransactionConflict`

### Kafka

- Topic ingress (`MessagesInPerSec`, `BytesInPerSec`)
- Replication health (`UnderReplicatedPartitions`, `UnderMinIsrPartitionCount`)
- Consumer lag (`records-lag-max`, per-partition lag)
- Commit latency and poll health metrics for consumers

## Alert Design Rules

- Alerts must map to actionable runbook steps.
- Use multi-window and severity-aware thresholds when possible.
- Prioritize user-impacting symptoms before low-level causes.
- Avoid alert noise: deduplicate and ensure clear ownership.
- Avoid non-actionable alerts; each alert must have a concrete first-response action.
- If a metric is not applicable, explicitly state `N/A` with justification in PR.

## Observability Conventions

- Use consistent metric naming and attributes.
- Keep metric cardinality controlled.
- Ensure trace/log correlation identifiers exist for triage.

## Post-Release Quality KPIs

Track these per release to drive continuous improvement:

- Defect escape rate
- MTTR for rollout-related incidents
- Rollback frequency
- SLO compliance delta (pre/post release)
- Alert quality ratio (actionable alerts / total alerts)

## Researched Authoritative Sources

- Google SRE, Monitoring Distributed Systems (golden signals): https://sre.google/sre-book/monitoring-distributed-systems/
- OpenTelemetry metrics semantic conventions: https://opentelemetry.io/docs/specs/semconv/general/metrics/
- AWS CloudWatch recommended alarms: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Best_Practice_Recommended_Alarms_AWS_Services.html
- DynamoDB metrics and dimensions: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/metrics-dimensions.html
- Apache Kafka monitoring docs: https://kafka.apache.org/41/operations/monitoring/
- PostgreSQL monitoring statistics: https://www.postgresql.org/docs/current/monitoring-stats.html
- PostgreSQL `pg_stat_statements`: https://www.postgresql.org/docs/current/pgstatstatements.html
