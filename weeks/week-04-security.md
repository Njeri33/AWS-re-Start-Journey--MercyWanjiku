# Week 4: Securing, Auditing, Detecting & Monitoring AWS

## 🎯 Task
Build on the Week 3 VPC environment and layer on security, auditing, detection, and monitoring capabilities to satisfy six business requirements:
1. **Identity & Access** — least-privilege access for a Cloud Administrator, Developer, and Security/Audit user
2. **Accountability & Auditing** — track who did what, when, and to which resource
3. **Threat Detection** — continuous assessment for suspicious activity
4. **Configuration Compliance** — evaluate resources against a defined standard
5. **Data Protection** — encrypt data using an AWS-managed key management solution
6. **Infrastructure Monitoring** — metrics, alarms, thresholds, and a dashboard for EC2 health

Then tie it all together: given a scenario where a developer makes a non-compliant change, show how to identify who did it, investigate it, check compliance, detect threats, monitor the resource, and take corrective action.

## 🛠️ What I Did
- Designed **IAM roles/policies** for three distinct personas — Cloud Administrator, Developer, and Security/Audit user — each scoped to only the permissions their role actually needs
- Tested that a Developer identity could **not** perform Administrator-level actions, confirming least privilege was actually enforced (not just configured on paper)
- Enabled **CloudTrail** to capture account activity, then performed a real administrative action and used the trail to identify the identity, action, resource, and timestamp involved
- Enabled **GuardDuty** for continuous threat detection and reviewed what types of findings it's designed to surface
- Set up an **AWS Config** rule to continuously evaluate a resource against a defined configuration standard, and documented what remediation would look like for a non-compliant resource
- Used **AWS KMS** to encrypt a protected resource, and restricted which identities/services could use that key
- Built **CloudWatch** metrics, an alarm with a meaningful threshold, and a monitoring dashboard for the EC2 environment from Week 3
- Updated my architecture diagram to show the network layer *and* the new security/monitoring layer wrapped around it

## 📚 What I Learned
- **Least privilege isn't a checkbox — it's a test.** The real proof isn't the policy JSON, it's confirming a restricted identity genuinely *cannot* do something it shouldn't be able to
- **CloudTrail, GuardDuty, Config, KMS, and CloudWatch aren't separate silos** — they form a pipeline: *Identity → Protection → Detection → Audit → Compliance → Monitoring → Assessment*
- Security is as much about **being able to answer questions after the fact** ("who did this, and when?") as it is about preventing things upfront
- Encryption is only as strong as **key access control** — encrypting data means nothing if every identity can freely use the KMS key
- A good monitoring setup isn't just "metrics exist" — it's a **meaningful threshold** plus a **visible/actionable alert** when that threshold is crossed

## 🐛 What Went Wrong
While testing least-privilege access, I locked myself out of being able to view resource details I actually needed to complete later parts of the project — I had scoped the Developer role permissions too tightly, based on my own assumptions about what a developer "should" need, without actually testing the full workflow first.

## ✅ How I Solved It
- Switched back to the Cloud Administrator identity to regain full access
- Reviewed exactly which actions the Developer persona needed to perform across the *entire* project (not just the obvious ones), then rebuilt the policy incrementally — starting minimal and adding only the specific permissions that were actually exercised and justified
- Re-tested the Developer identity end-to-end afterward to confirm it could do everything it needed — and nothing more

**Takeaway:** Least privilege is a balancing act, not a one-shot guess. It's better to start too restrictive and expand deliberately than to start too permissive — but "too restrictive" still needs a fast way to diagnose and fix, which is exactly what good audit logging (CloudTrail) is for.

---
[⬅ Back to overview](../README.md) | [⬅ Previous: Week 3 — Networking](week-03-networking.md) | [Next: Week 5 — Python ➡](week-05-python.md)
