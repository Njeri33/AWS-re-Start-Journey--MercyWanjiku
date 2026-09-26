# Week 5: Python

## Task
Core data types, control flow, functions, file I/O, and debugging — applied to a Caesar cipher and a human-insulin dataset. Final exercise: fix 4 intentionally broken cipher programs.

## What I Did
- Data types: `int`, `float`, `complex`, `bool`, `str`, `list`, `tuple`, `dict`, mixed-type lists, nested composite types
- Control flow: `if`/`elif`/`else`, `while`, `for`
- Molecular weight + net charge of insulin using dictionaries, `count()`, list comprehension
- Built a Caesar cipher: `getDoubleAlphabet()`, `getMessage()`, `getCipherKey()`, `encryptMessage()`, `decryptMessage()`
- `os.system()` / `subprocess.run()` to call Bash from Python
- Used VS Code debugger: breakpoints, step-over, watch expressions

## What I Learned
- `+` on strings ≠ `+` on numbers — Python cares about type even when syntax looks identical
- List indices start at 0 — source of more bugs than anything else
- Not every bug crashes. Some run clean and just produce the wrong answer silently.

## What Went Wrong → How I Solved It

| Bug | Symptom | Cause | Fix |
|---|---|---|---|
| #1 | `TypeError: int + str` | `cipherKey` from `input()` is a string | Cast with `int(cipherKey)` |
| #2 | Only part of message encrypted | Missing `.upper()` on message | Restored `.upper()` before encrypting |
| #3 | Decryption returns garbage | `decryptMessage()` passed original `cipherKey`, not negated `decryptKey` | Pass `decryptKey` into the call |
| #4 | "Decrypted" output == encrypted output | Final `print()` referenced `myEncryptedMessage` instead of `myDecryptedMessage` | Fixed variable name in print statement |

**Lesson:** read tracebacks bottom-up. When there's no traceback, the bug is usually a wrong variable, not wrong logic.

##  Key Takeaways
- A loud error (traceback) is easier to fix than a silent wrong answer
- Always check variable types coming from `input()` — they're strings by default
- Debugging is a skill of isolation: small, single-purpose functions make bugs easy to locate

---
[⬅ Back to overview](../README.md) | [⬅ Week 4](week-04-security.md)

Week 6 here we come!!