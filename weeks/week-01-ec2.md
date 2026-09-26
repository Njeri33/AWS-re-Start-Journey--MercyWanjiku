# Week 1: Amazon EC2

## Task
Launch an EC2 instance with termination protection, install a web server via User Data, fix access, resize it, then terminate it.

## What I Did
- Launched `t3.micro`, Amazon Linux 2023 AMI, no key pair (no SSH needed)
- User Data:
  ```bash
  #!/bin/bash
  yum -y install httpd
  systemctl enable httpd
  systemctl start httpd
  echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
  ```
- Enabled termination protection at launch
- Checked status checks (2/2) and CloudWatch metrics
- Fixed security group to allow HTTP
- Stopped instance → changed type `t3.micro` → `t3.small` → grew EBS 8GiB → 10GiB → restarted
- Disabled termination protection → terminated

## What I Learned
- Security groups are stateful, default-deny firewalls — nothing gets in without an explicit rule
- Changing instance type requires the instance to be stopped first
- EBS volume size and instance type scale independently

## What Went Wrong
Instance was `Running`, 2/2 checks passed, but the public IP loaded nothing in the browser.

## How I Solved It
Security group had zero inbound rules. Added:
- Type: HTTP, Port 80, Source: 0.0.0.0/0

Refreshed → worked.

**Lesson:** running ≠ reachable. The server and the firewall are two different problems.

##  Key Takeaways
- A healthy instance and an accessible instance are not the same thing.
- Security groups default to deny-all — you must explicitly open every port you need.
- Resizing compute and storage are two separate operations, and compute resizing needs a stop first.

---
[⬅ Back to overview](../README.md) | [Next: Week 2 — Linux ➡](week-02-linux.md)