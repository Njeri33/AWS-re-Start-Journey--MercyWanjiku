# Week 4: Secure, Audit, Detect, Monitor

## Task
Layer security onto the Week 3 VPC: least-privilege IAM, audit logging, threat detection, config compliance, encryption, and infrastructure monitoring.

## What I Did
| Requirement | Service | Config |
|---|---|---|
| Identity & access | IAM | 3 roles: Cloud Admin, Developer, Security/Audit — scoped separately |
| Auditing | CloudTrail | Enabled; traced a real action to identity + resource + timestamp |
| Threat detection | GuardDuty | Enabled; reviewed finding types |
| Compliance | AWS Config | 1 rule evaluating a resource against a defined standard |
| Encryption | KMS | Encrypted a resource; restricted key usage to specific identities |
| Monitoring | CloudWatch | 1 metric, 1 alarm with threshold, 1 dashboard |

- Confirmed Developer role genuinely **could not** perform Admin actions (tested, not assumed)
- Diagram updated to show security/monitoring layer over the network layer

## What I Learned
- Least privilege only counts if you test that denial actually happens
- The six services form one pipeline: Identity → Protection → Detection → Audit → Compliance → Monitoring
- Encryption without key-access restriction is theater — anyone with key access bypasses it

## What Went Wrong
Scoped the Developer IAM policy too tight — locked myself out of resource views I actually needed later.

## How I Solved It
Switched back to Admin, mapped every action the Developer role actually needed across the full project, rebuilt the policy from a minimal base, added only justified permissions, re-tested end-to-end.

## Key Takeaways
- Least privilege is proven by testing denial, not by writing the policy.
- Security services work best as a connected pipeline, not standalone tools.
- Start restrictive and expand deliberately ;it's easier than starting open and locking down later.

---
[⬅ Back to overview](../README.md) | [⬅ Week 3](week-03-networking.md) | [Next: Week 5 — Python ➡](week-05-python.md)