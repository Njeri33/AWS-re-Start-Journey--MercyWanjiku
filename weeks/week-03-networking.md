# Week 3: Networking in the AWS Cloud

## 🎯 Task
Design and deploy a secure AWS VPC for an organization supporting ~5,000 users, including:
- A VPC (`192.168.0.0/16`) with **2 public** and **2 private** subnets across two Availability Zones
- An Internet Gateway, with routing so only public subnets can reach the internet
- Two separate route tables (public and private)
- 4 EC2 instances (2 public, 2 private), each running a simple web server
- Security groups following least-privilege principles
- Proof that public instances are reachable from the internet — and that private instances are **not**

## 🛠️ What I Did
- Created the VPC with CIDR `192.168.0.0/16`, sized to comfortably support thousands of hosts
- Created 4 subnets:
  - Public Subnet 1 (`192.168.1.0/24`, AZ1) and Public Subnet 2 (`192.168.2.0/24`, AZ2)
  - Private Subnet 1 (`192.168.3.0/24`, AZ1) and Private Subnet 2 (`192.168.4.0/24`, AZ2)
- Created and attached an Internet Gateway to the VPC
- Built **two separate route tables**:
  - Public route table: `0.0.0.0/0 → Internet Gateway`, associated with both public subnets
  - Private route table: no internet route at all, associated with both private subnets
- Launched 4 EC2 instances — 2 in public subnets (with public IPs), 2 in private subnets (no public IPs)
- Installed a web server on each, configured to dynamically display `Hello from the <PUBLIC-IP> address` using the instance metadata service
- Created two security groups:
  - **Public SG:** HTTP (80) from anywhere, SSH (22) only from my own IP
  - **Private SG:** no direct internet access; optionally allowed HTTP only from the public SG (SG-to-SG communication)
- Tested connectivity from outside the VPC: public IPs loaded successfully in the browser; private IPs timed out as expected

## 📚 What I Learned
- **"Public" and "private" aren't properties of a subnet by themselves** — they're the *result* of route table configuration + whether an IGW route exists + whether the instance has a public IP
- A route table is really just a set of instructions: "for traffic going to X, send it via Y" — and the absence of a route is just as important as the presence of one
- **Security-group-to-security-group rules** are a clean way to allow internal communication (e.g. public → private) without opening anything to the broader internet
- CIDR planning matters early — a `/16` for the VPC and `/24`s for each subnet gave plenty of headroom for 5,000+ hosts without overlap headaches
- The instance metadata service is a handy way to let an instance dynamically discover its own public IP, rather than hardcoding it

## 🐛 What Went Wrong
After everything appeared to be configured correctly, my "private" EC2 instance was still unexpectedly reachable when I first tested it — which completely defeated the point of the private subnet.

## ✅ How I Solved It
The root cause was a **route table association mistake** — I had accidentally associated one of my private subnets with the *public* route table (which included the `0.0.0.0/0 → Internet Gateway` route), instead of the private route table.

**Fix:**
1. Checked each subnet's associated route table in the VPC console
2. Re-associated the mis-configured private subnet with the correct **private route table** (no IGW route)
3. Re-tested from outside the VPC — the private instance's IP now timed out exactly as expected, confirming isolation

**Takeaway:** Public vs. private isn't decided at the subnet level alone — it's enforced by which route table is *associated* with that subnet. A subnet with the "private" name but the wrong route table association is not actually private at all.

---
[⬅ Back to overview](../README.md) | [⬅ Previous: Week 2 — Linux](week-02-linux.md) | [Next: Week 4 — Security ➡](week-04-security.md)
