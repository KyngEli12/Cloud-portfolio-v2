# Module 06 — High Availability: Decision Log

## Component 1: Multi-Region Active-Active App

**Architecture**: Two S3 static website buckets — `eli-ha-primary` (us-east-1) and `eli-ha-secondary` (us-west-2) — fronted by Route 53 latency-based routing with health checks.

**Key decisions**:
- Used S3 static website hosting as a lightweight, cost-free stand-in for a real application tier
- Disabled S3 Block Public Access per bucket before applying public bucket policies (account-level default blocks public policies)
- Created Route 53 hosted zone `eli-ha-demo.internal` as a public zone using a non-registered domain to demonstrate routing logic without incurring domain registration cost
- Latency routing policy sends traffic to whichever region responds faster from the client's location
- Health checks on both S3 website endpoints — 30s interval, 3 failure threshold — trigger automatic failover if a region goes down

**Verified**: Route 53 Console showed two CNAME latency records (US East, US West) with health check IDs attached.

## Component 2: Chaos Engineering with AWS FIS

**Architecture**: FIS experiment template targeting EC2 instances tagged `Environment=eli-ha-demo` with `aws:ec2:stop-instances` action, 2-minute auto-recovery duration.

**Key decisions**:
- Used tag-based resource targeting so the experiment scope is explicit and bounded
- Set `startInstancesAfterDuration: PT2M` so instances auto-recover — simulates a transient AZ outage, not permanent failure
- Stop condition set to `none` since this is a controlled demo with no CloudWatch alarms wired up
- IAM role `eli-fis-role` with PowerUserAccess attached to FIS service principal

**Verified**: FIS Console showed experiment template `EXT47iNVPThdAUzc` created successfully.

## Teardown
All resources deleted: S3 buckets, Route 53 hosted zones, health checks, FIS experiment template, IAM role.
