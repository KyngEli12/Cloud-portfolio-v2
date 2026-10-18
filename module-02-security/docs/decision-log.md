# Decision Log — Module 02: Security (Least-Privilege IAM Architecture)

## Overview
This log captures the key architectural decisions made while designing a least-privilege IAM architecture on AWS. Each decision reflects a real trade-off between security, operational overhead, and blast-radius containment.

---

## Decision 1: No long-lived IAM user access keys
**Choice:** Developers assume roles via STS for temporary credentials instead of using static access keys.
**Why:** Long-lived keys are the leading cause of AWS credential breaches. Temporary credentials expire automatically — no manual rotation required.
**Trade-off:** Developers must re-authenticate hourly, adding minor friction to the workflow.
**Tag:** Security

---

## Decision 2: Permission boundaries on all roles
**Choice:** Attach a permission boundary to every IAM role, independent of the role's identity policy.
**Why:** A second guardrail that caps what the role can do even if an admin accidentally grants a wildcard policy later. Two independent locks.
**Trade-off:** More IAM objects to manage. Boundaries must be updated whenever the role legitimately needs new permissions.
**Tag:** Security

---

## Decision 3: Separate execution role per Lambda function
**Choice:** Each Lambda function gets its own dedicated IAM execution role — no shared roles between workloads.
**Why:** A shared role means a compromised function can affect every other workload using that role. Isolation limits blast radius to one function.
**Trade-off:** More roles to maintain at scale. Manageable with IaC (CDK or Terraform).
**Tag:** Design

---

## Decision 4: SCP denies CloudTrail disable at org level
**Choice:** Service Control Policy at the AWS Organization root explicitly denies cloudtrail:StopLogging and cloudtrail:DeleteTrail for all accounts.
**Why:** An attacker's first move after gaining access is often to disable logging. Making it undeniable at the org level removes that option entirely.
**Trade-off:** None. There is no legitimate production reason to disable CloudTrail.
**Tag:** Security

---

## Decision 5: Resource-based policies on S3 and DynamoDB
**Choice:** Each resource has its own resource policy allowing only the specific role and specific actions needed.
**Why:** Defense in depth. Even if IAM is misconfigured, the resource itself won't serve data to an unexpected principal. Two independent checks.
**Trade-off:** Policy changes must happen in two places (IAM role policy + resource policy) when access requirements change.
**Tag:** Ops

---

## Decision 6: Explicit Deny on s3:DeleteObject
**Choice:** An explicit Deny statement blocks s3:DeleteObject for all principals on the artifacts bucket.
**Why:** Accidental deletions are irreversible without versioning. An explicit deny blocks it for everyone, including future admins.
**Trade-off:** Cleanup operations require a deliberate policy change — intentional friction that forces review before deletion.
**Tag:** Security
