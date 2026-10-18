# Module 03 – Compute: Decision Log

## Decision 1 — Spot Fleet over On-Demand for batch workloads
**Date:** 2026-10-18
**Decision:** Use EC2 Spot Fleet with diversified instance pools for cost-optimized batch processing
**Rationale:** Spot instances reduce compute costs 60–90% vs On-Demand; diversified pools minimize interruption risk
**Trade-off:** Workloads must tolerate interruption; not suitable for stateful or latency-sensitive tasks
**Result:** Fleet active with t3.micro + t2.micro, saved 73% vs On-Demand ($0.01 total cost)

## Decision 2 — Lambda + SQS fan-out for async job processing
**Date:** 2026-10-18
**Decision:** Trigger Lambda from SQS queue with batch processing (batchSize=5)
**Rationale:** Decouples producers from consumers; Lambda scales to zero when idle (no cost); SQS provides durable message buffer and retry logic
**Trade-off:** Added latency vs direct invocation; SQS visibility timeout must exceed Lambda execution time
**Result:** eli-compute-fanout triggered by eli-compute-jobs queue, verified enabled in Console

## Decision 3 — ASG mixed instances policy for resilient scaling
**Date:** 2026-10-18
**Decision:** Auto Scaling Group with 25% On-Demand base + 75% Spot, price-capacity-optimized strategy
**Rationale:** Blends reliability (On-Demand) with cost savings (Spot); price-capacity-optimized reduces interruptions vs lowest-price strategy
**Trade-off:** SpotAllocationStrategy "diversified" is Spot Fleet only — ASG requires "price-capacity-optimized"; slight complexity vs pure On-Demand
**Result:** eli-compute-asg created with t3.micro + t2.micro pool, 1-3 instance range, verified at desired capacity in Console
