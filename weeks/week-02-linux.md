# Week 2: Linux & Bash Shell Scripting

## 🎯 Task
Connect to an Amazon Linux EC2 instance via SSH and write a Bash script that:
- Creates 25 empty (0 KB) files per run, using `touch`
- Names them `<yourName><number>` (e.g. `jane101`, `jane102`, ...)
- Automatically continues numbering from the **last/highest number that already exists** each time it's run — no hardcoded numbers allowed
- Displays a long listing (`ls -l`) of the directory afterward to confirm the files were created correctly

## 🛠️ What I Did
- Connected to the Amazon Linux EC2 instance over SSH using the provided key pair (`chmod 400` on the `.pem` file first, then `ssh -i labsuser.pem ec2-user@<public-ip>`)
- Wrote a Bash script that:
  1. Scans the current directory for existing files matching my naming pattern
  2. Extracts the numeric suffix from each matching filename
  3. Finds the **maximum** number among them (defaulting to 0 if no files exist yet)
  4. Loops 25 times, using `touch` to create files starting at `max + 1`
- Ran the script multiple times in a row to confirm each run correctly picked up where the last one left off
- Verified the output with `ls -l` to check file count, naming, and 0 KB size

## 📚 What I Learned
- **SSH key permissions matter** — `chmod 400` on the `.pem` file isn't optional; SSH will refuse to use an overly permissive key
- Bash is genuinely good at **text processing on the fly** — combining `ls`, pattern matching, and numeric extraction to dynamically calculate "the next number" felt like a small superpower once it worked
- **Idempotent automation** (scripts that behave correctly no matter how many times you run them) is a core DevOps mindset — you don't want a script that breaks or duplicates work on a second run
- `touch` is deceptively simple but genuinely useful for scaffolding files before you populate them with real content

## 🐛 What Went Wrong
My first version of the script *technically* worked — but only once. Running it a second time either:
- Restarted numbering from 1 again (overwriting/skipping existing files), or
- Errored out because my number-extraction logic assumed there were always existing files to compare against

The core issue: I hardcoded the starting number logic to assume a fixed starting point instead of **dynamically detecting** the actual current state of the directory.

## ✅ How I Solved It
Rewrote the logic so the script:
1. First checks whether any matching files exist at all
2. If none exist, starts numbering at 1
3. If some exist, extracts all their numeric suffixes, finds the maximum with proper numeric (not alphabetical/string) comparison, and starts the new batch at `max + 1`

The numeric-vs-string comparison bug was the sneaky part — string sorting put `"9"` after `"10"`, which quietly threw off my "highest number" logic until I forced proper numeric comparison.

**Takeaway:** "Don't hardcode it" isn't just a lab instruction — it's the difference between a script and a *real* automation tool. Always design for the script being run more than once.

---
[⬅ Back to overview](../README.md) | [⬅ Previous: Week 1 — EC2](week-01-ec2.md) | [Next: Week 3 — Networking ➡](week-03-networking.md)
