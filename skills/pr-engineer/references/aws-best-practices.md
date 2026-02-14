# AWS Best Practices (Authoritative)

Use this file during Design, Criticize, and Execute stages.

## Core Engineering Guidance

1. Apply AWS Well-Architected pillars explicitly: operational excellence, security, reliability, performance efficiency, cost optimization, sustainability.
2. Enforce least privilege IAM with scoped roles/policies and short-lived credentials.
3. Encrypt data in transit and at rest; manage keys with KMS and rotate where required.
4. Make distributed operations idempotent and resilient with retries, exponential backoff, and jitter.
5. Build observable systems with CloudWatch metrics/logs/alarms, tracing, and actionable dashboards.
6. Design for failure: multi-AZ where relevant, health checks, graceful degradation, and clear recovery paths.
7. Prefer managed services when they reduce operational burden without violating constraints.
8. Control spend with cost allocation tags, budgets/alerts, and right-sized resources.

## Review Checklist

- Is IAM least privilege enforced for each component?
- Are retry/backoff and timeout strategies explicitly defined?
- Are critical workloads fault-tolerant and monitored with alerts?
- Are security controls documented (encryption, secrets, network boundaries)?
- Is there a cost-impact note and guardrails for scaling behavior?

## Primary Sources

- AWS Well-Architected Framework: https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html
- Well-Architected Pillars: https://docs.aws.amazon.com/wellarchitected/latest/framework/the-pillars-of-the-framework.html
- IAM Best Practices: https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html
- AWS Security Best Practices: https://docs.aws.amazon.com/pdfs/wellarchitected/latest/security-pillar/wellarchitected-security-pillar.pdf
- Reliability Pillar: https://docs.aws.amazon.com/pdfs/wellarchitected/latest/reliability-pillar/wellarchitected-reliability-pillar.pdf
- Performance Efficiency Pillar: https://docs.aws.amazon.com/pdfs/wellarchitected/latest/performance-efficiency-pillar/wellarchitected-performance-efficiency-pillar.pdf
- Cost Optimization Pillar: https://docs.aws.amazon.com/pdfs/wellarchitected/latest/cost-optimization-pillar/wellarchitected-cost-optimization-pillar.pdf
