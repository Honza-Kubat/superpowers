# Kafka Topic Design Best Practices (Strict)

Last researched: 2026-02-14

Use this reference when designing Kafka topics, producer behavior, and consumer semantics.

## Non-Negotiable Rules

1. Define topic purpose, key strategy, partitioning, retention, and compaction policy up front.
2. Set durability expectations explicitly (`acks`, replication factor, ISR policy).
3. Use idempotent producers for production write safety.
4. Choose consumer commit and isolation settings to match delivery guarantees.
5. Design for replay, backfill, and failure recovery from day one.

## Topic and Durability Rules

- Topic config must explicitly define:
  - retention policy (`retention.ms`/`retention.bytes`)
  - cleanup policy (`delete` vs `compact`)
  - durability controls (`min.insync.replicas`)
- Producer durability baseline for critical data:
  - `acks=all`
  - idempotence enabled
- Ensure partition key selection preserves required ordering semantics.

## Consumer Semantics Rules

- Avoid implicit auto-commit assumptions for critical workflows.
- Tune poll/processing parameters to avoid accidental group churn.
- For transactional/exactly-once flows:
  - use transactions and transactional producer IDs
  - use `read_committed` where semantics require it

## Operability Rules

- Define lag/error/throughput monitoring for each critical topic.
- Define DLQ or compensating path for non-retriable failures.
- Define schema evolution strategy and compatibility policy.

## Quality Gates

- Topic config reviewed against SLO and durability goals.
- Producer/consumer settings reviewed against delivery semantics.
- Replay and recovery procedures documented and tested.
- Monitoring and alerts cover lag, errors, and throughput saturation.

## Researched Authoritative Sources

- Kafka design docs: https://kafka.apache.org/documentation/#design
- Kafka design (current versioned docs): https://kafka.apache.org/41/design/design/
- Kafka topic configs: https://kafka.apache.org/41/configuration/topic-configs/
- Kafka producer configs: https://kafka.apache.org/40/configuration/producer-configs/
- Kafka consumer configs: https://kafka.apache.org/41/configuration/consumer-configs/
