# Week 2: Linux & Bash Scripting Challenge

## Task
SSH into an EC2 instance. Write a Bash script that creates 25 empty files per run (`<name><number>`), continuing numbering from the highest existing number — no hardcoded starting point.

## What I Did
```bash
#!/bin/bash
NAME="MercyWanjikuNjeri"
last=$(ls | grep -E "^${NAME}[0-9]+$" | sed "s/${NAME}//" | sort -n | tail -1)
start=$(( ${last:-0} + 1 ))
for i in $(seq $start $((start+24))); do
  touch "${NAME}${i}"
done
ls -l
```
- Ran it multiple times back-to-back to confirm it continues from the last number each time

## What I Learned
- `chmod 400` on the `.pem` key is mandatory — SSH refuses overly-permissive keys
- Default `sort` is alphabetical, not numeric — `10` sorts before `9` unless you use `sort -n`
- Idempotent scripts (safe to re-run) are a core automation habit, not a nice-to-have

## What Went Wrong
Second run either restarted numbering at 1, or errored out.

## How I Solved It
Root cause: string sort instead of numeric sort when finding the max existing number. Fixed with `sort -n`, plus a fallback (`${last:-0}`) for when no files exist yet.

## Key Takeaways
- Always test automation scripts by running them more than once, not just once.
- String sort vs. numeric sort is a classic silent bug — `sort -n` matters.
- Small shell scripts can hide big logic bugs; verify output, don't just check "it ran".

---
[⬅ Back to overview](../README.md) | [⬅ Week 1](week-01-ec2.md) | [Next: Week 3 — Networking ➡](week-03-networking.md)