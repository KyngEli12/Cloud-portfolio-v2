# Module 01 - Databases: Decision Log

**Stack:** ElastiCache Redis - DynamoDB - Aurora Serverless v2
**Pattern:** Cache-Aside
**Date:** 2026-10-16

## Architecture Overview

Read requests flow through three layers:
Client > ElastiCache Redis > DynamoDB > Aurora Serverless v2

- Warm path (cache hit): 1 hop, sub-millisecond
- Cold path (cache miss): 3 hops - Redis miss > DynamoDB read > cache populate > return

## Decision Log

### 1. Cache-Aside Pattern
Why: Application controls cache population - only requested data gets cached. No wasted memory on cold data.
Trade-off: First request always pays the miss penalty.

### 2. ElastiCache Redis Serverless
Why: Scales to zero, no cluster sizing, zero idle cost between sessions.
Trade-off: Higher per-request cost vs provisioned cluster at scale.
Cost control: Delete cluster after each session.

### 3. DynamoDB (Free Tier)
Why: Single-digit ms reads, PK on productId maps 1:1 to cache key.
Trade-off: No joins. Relational workloads go to Aurora.

### 4. Aurora Serverless v2 (Deferred)
Why: Needed for relational workloads. Serverless v2 scales to zero ACUs.
Trade-off: Deferred to isolate Aurora setup and control cost per session.

### 5. TTL: 5 Minutes
Why: Product catalog is mostly static. Balances freshness vs hit rate.
Trade-off: Stale reads possible within window.

### 6. Cache Key: productId (matches DynamoDB PK)
Why: 1:1 mapping eliminates translation logic. Cache hit/miss is O(1).
Trade-off: Not suitable for list queries.

## Cost Per Session

| Resource | Cost |
|---|---|
| DynamoDB (free tier) | $0.00 |
| ElastiCache Redis Serverless | ~$0.50-2.00 |
| S3 + CloudFront | ~$0.01/mo |
| Total | < $2.00/session |
