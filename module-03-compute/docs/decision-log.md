# Module 03 – Compute: Decision Log

## Decision 1 — Spot Fleet over On-Demand for batch workloads
**Date:** 2026-10-18
**Decision:** Use EC2 Spot Fleet with diversified instance pools for cost-optimized batch processing
**Rationale:** Spot instances reduce compute costs 60–90% vs On-Demand; diversified pools minimize interruption risk
**Trade-off:** Workloads must tolerate interruption; not suitable for stateful or latency-sensitive tasks
