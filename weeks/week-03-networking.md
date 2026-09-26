# Week 3: VPC Networking

## Task
Build a VPC (`192.168.0.0/16`) with 2 public + 2 private subnets, an IGW, correct routing, 4 EC2 instances, and security groups — then prove public instances are reachable and private ones aren't.

## What I Did
| Resource | Config |
|---|---|
| VPC | `192.168.0.0/16` |
| Public Subnet 1/2 | `192.168.1.0/24` (AZ1), `192.168.2.0/24` (AZ2) |
| Private Subnet 1/2 | `192.168.3.0/24` (AZ1), `192.168.4.0/24` (AZ2) |
| Public route table | `0.0.0.0/0 → IGW`, attached to public subnets |
| Private route table | no IGW route, attached to private subnets |
| Public SG | HTTP 80 from 0.0.0.0/0, SSH 22 from my IP only |
| Private SG | HTTP 80 only from Public SG |

- Web server on each instance, public IP displayed dynamically via instance metadata
- Verified: public IPs load in browser; private IPs time out from outside the VPC

## What I Learned
- "Public" vs "private" is entirely determined by **route table association** — not a subnet property or a label
- SG-to-SG rules let private instances trust public instances without opening to the internet
- No route to an IGW = no path out, full stop

## What Went Wrong
A private subnet was still internet-reachable on first test.

## How I Solved It
Found it was associated with the **public** route table by mistake. Re-associated it with the private route table (no IGW route). Re-tested — timed out as expected.

**Lesson:** the subnet name means nothing. The route table association is the actual control.

## Key Takeaways
- Route table association — not subnet naming — is what actually makes a subnet public or private.
- Always re-test after "fixing" a network config; assumptions are not verification.
- Security-group-to-security-group rules are cleaner than opening ports to the whole internet.

---
[⬅ Back to overview](../README.md) | [⬅ Week 2](week-02-linux.md) | [Next: Week 4 — Security ➡](week-04-security.md)