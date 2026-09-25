# Week 5: Python Programming

## 🎯 Task
A series of Python labs covering the language fundamentals and applying them to a real-world dataset (human insulin sequences): Hello World, numeric/string/list/tuple/dictionary data types, composite data types, conditionals, loops, Git/GitHub basics, functions (building a Caesar cipher), file handling with JSON, calling Bash from Python, using the debugger — and finally, **debugging four intentionally buggy versions** of the Caesar cipher program.

## 🛠️ What I Did
- Wrote and ran my first Python script (`print("Hello, World")`) to confirm the environment was working
- Practiced Python's core data types: `int`, `float`, `complex`, `bool`, `str`, `list`, `tuple`, `dict` — including mixed-type lists and composite structures (a dictionary nested inside a list, populated from a CSV file)
- Used `if` / `elif` / `else` conditionals and both `while` and `for` loops (including a "Guess the Number" game)
- Created a GitHub account and pushed my early lab files to a private repository
- Calculated the rough molecular weight and net charge of human insulin using dictionaries, `count()`, list comprehensions, and a `while` loop across a pH range — reinforcing that Python is genuinely useful for scientific/data work, not just scripting
- Built a **Caesar cipher encryption program** from scratch using user-defined functions: `getDoubleAlphabet()`, `getMessage()`, `getCipherKey()`, `encryptMessage()`, `decryptMessage()`, and a `runCaesarCipherProgram()` driver function
- Used `os.system()` and `subprocess.run()` to call Bash commands (`ls`, `uname -a`, `ps -x`) directly from Python
- Practiced with the VS Code Python Debugger — breakpoints, step-over, watch expressions
- Debugged **four separate broken versions** of the Caesar cipher program, each with a different bug

## 📚 What I Learned
- Python's flexibility (mixed-type lists, dynamic typing) is a double-edged sword — powerful, but it hides type errors until runtime
- **String concatenation (`+`) and numeric addition (`+`) are not interchangeable** — Python cares deeply about type, even if the syntax looks identical
- List indices start at `0`, and this one fact is responsible for more real bugs than almost anything else in the language
- A **traceback isn't scary once you read it bottom-up** — it points you almost directly to the offending line
- Not every bug throws an error. Some bugs run "successfully" and just produce the *wrong result silently* — those are the dangerous ones, because nothing tells you to go looking
- Writing small, single-purpose functions (like the Caesar cipher's `getDoubleAlphabet()` or `getCipherKey()`) makes debugging dramatically easier, because you can isolate exactly which function is misbehaving

## 🐛 What Went Wrong
Across the four buggy Caesar cipher versions, I hit a mix of failure types:

1. **Bug #1 — Loud failure:** `TypeError: unsupported operand type(s) for +: 'int' and 'str'` — a crash with a clear traceback
2. **Bug #2 — Silent partial failure:** the program ran fine and printed output, but only *part* of the message was actually encrypted
3. **Bug #3 — Silent full failure on decryption:** encryption worked, but decrypting the message produced garbage instead of the original text
4. **Bug #4 — Misleading output:** the "decrypted" message printed was identical to the encrypted message, making it look like decryption did nothing at all

## ✅ How I Solved It
- **Bug #1:** The cipher key from `input()` is always a **string**, but the encryption math needs an **integer**. Fixed by casting with `int(cipherKey)` before doing arithmetic on it.
- **Bug #2:** The message was never converted to uppercase before encryption (`uppercaseMessage = message` instead of `message.upper()`), so lowercase letters weren't found in the (uppercase-only) alphabet string and were passed through unencrypted. Fixed by restoring `.upper()`.
- **Bug #3:** The `decryptMessage()` function was calling `encryptMessage()` with the **original** `cipherKey` instead of the negated `decryptKey` it had just calculated — so "decryption" was actually just encrypting again. Fixed by passing `decryptKey` into the call.
- **Bug #4:** The final `print()` statement was referencing `myEncryptedMessage` again instead of `myDecryptedMessage` — a simple variable mix-up in the output line, not a logic bug at all. Fixed by correcting the variable name being printed.

**Takeaway:** Not all bugs are created equal — some crash loudly, some fail silently, and some aren't even "bugs" in the logic at all, just a mismatched variable name at the very last step. The debugger (and reading your own code slowly, line by line) beats guessing every time.

---
[⬅ Back to overview](../README.md) | [⬅ Previous: Week 4 — Security](week-04-security.md)
