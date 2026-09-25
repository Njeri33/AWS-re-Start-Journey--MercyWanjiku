# Week 1: Introduction to Amazon EC2

## 🎯 Task
Launch an Amazon EC2 instance with termination protection enabled, deploy a simple web server using a User Data script, monitor the instance, modify its security group to allow HTTP access, resize the instance (type + EBS volume), test termination protection, and finally terminate the instance.

## 🛠️ What I Did
- Launched a `t3.micro` EC2 instance named **Web Server** using the Amazon Linux 2023 AMI
- Skipped key pair creation (no SSH login needed for this lab) and removed the SSH inbound rule for better security
- Passed in a User Data script on launch:
  ```bash
  #!/bin/bash
  yum -y install httpd
  systemctl enable httpd
  systemctl start httpd
  echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
  ```
- Enabled **termination protection** under Advanced details before launch
- Monitored the instance's status checks and CloudWatch metrics, and pulled an instance screenshot from the console
- Updated the security group to allow inbound **HTTP (port 80)** traffic
- Stopped the instance, resized it from `t3.micro` → `t3.small`, and grew the EBS volume from 8 GiB → 10 GiB
- Restarted the resized instance
- Attempted to terminate the instance (blocked by termination protection), then disabled protection and terminated it for real

## 📚 What I Learned
- **AMIs** are the blueprint for an instance — OS, launch permissions, and block device mappings all come from it
- **Security groups act as a stateful firewall** — nothing gets in or out unless a rule explicitly allows it
- **User Data scripts** are a clean way to automate first-boot configuration instead of manually SSH-ing in and setting things up
- **Termination protection** is a simple but effective safety net against "oops" moments in production
- You can resize both **compute (instance type)** and **storage (EBS volume)** independently — but the instance must be stopped first for instance type changes
- **CloudWatch** and instance screenshots are useful troubleshooting tools even without direct SSH/RDP access

## 🐛 What Went Wrong
After launching the instance and confirming it was `Running` with `2/2` status checks passed, I opened the public IP in my browser expecting to see my web page.

**Nothing loaded.**

The instance was healthy, the web server was installed and running (per the User Data script) — but the browser just hung.

## ✅ How I Solved It
The issue wasn't the server — it was the **security group**. By default, the security group had no inbound rules, meaning port 80 (HTTP) traffic was being blocked entirely before it ever reached the instance.

**Fix:** Edited the inbound rules on the `Web Server security group` to explicitly allow:
- Type: `HTTP`
- Source: `Anywhere-IPv4`

Refreshed the browser — and there it was: *"Hello From Your Web Server!"*

**Takeaway:** A running server and an *accessible* server are two different things. Security groups control the door, not the building.

---
[⬅ Back to overview](../README.md) | [Next: Week 2 — Linux ➡](week-02-linux.md)
