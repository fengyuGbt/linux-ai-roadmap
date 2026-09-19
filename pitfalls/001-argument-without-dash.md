# 001. `head 10` → `head: cannot open '10' for reading`

> Date: 2026-09-19 ・ Phase 1 / Week 1 ・ Status: ✅ resolved

## 现象 / Symptom

```bash
$ du -sh 2>/dev/null | sort -rh | head 10
head: cannot open '10' for reading: No such file or directory
```

## 操作 / What I did

Typed `head 10` intending "show the first 10 lines", but dropped the dash.

## 排查 / Debugging

The error says head tried to *open a file named `10`* — so head didn't see `10` as an option, it saw it as a filename.

## 根因 / Root cause

Command options in Linux almost always start with `-` (short) or `--` (long). A bare word on the command line is treated as a **path/filename**, not an option.

`head 10` = "read the file called 10" → no such file → error.
`head -10` (or `head -n 10`) = "first 10 lines" → works.

## 解法 / Fix

```bash
du -sh ~/* 2>/dev/null | sort -rh | head -10
```

## 通用化 / Takeaway

- **Pattern to recognize:** `cannot open 'X' for reading` almost always means a value was interpreted as a **filename** — you missed a `-`, a quote, or an environment variable.
- **General rule:** when a command's argument is a value (number, pattern, name), it needs an option flag (`-n 10`, `--limit 10`); without the flag, commands assume it's a path.
- If unsure about a command's options: `man head` or `head --help` — reading the manual is faster than guessing (and it's English practice).
- ^C (Ctrl+C) is fine to interrupt a command you got wrong — it's normal, not a failure.
