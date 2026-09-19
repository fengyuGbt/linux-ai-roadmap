# Pitfalls — Problem Archive

A structured, searchable archive of **real problems, real root causes, and reusable solutions** encountered while following this roadmap.

> Uniform tutorials teach what knowledge *should* look like.
> This archive records what knowledge looks like when it *actually breaks* — which is different for every learner.

## Why this exists

- Everyone hits **different** problems in **different** contexts. A personal journey of mistakes is more valuable to other learners than another generic tutorial.
- Errors are the best teachers — but only if they are **captured, diagnosed, and generalized**.
- This turns a private learning log into public reference material.

## How it works

- `journal/` = **timeline** (what happened each week, in order)
- `pitfalls/` = **knowledge base** (problems archived by topic, searchable across weeks)

When something in your journal breaks and you figure out why — **promote it** into a pitfall card.

## Card structure (see `NNN-slug.md`)

| Field | What to write |
|---|---|
| 现象 / Symptom | Paste the actual error output — don't paraphrase |
| 操作 / What I did | The steps that led there |
| 排查 / Debugging | What you tried and observed |
| 根因 / Root cause | Why it happened, in one clear sentence + the principle |
| 解法 / Fix | The correct command/approach |
| 通用化 / Takeaway | The *general* lesson: how to see this class of problem next time |

## Rules

1. **One card = one real problem.** Paste real output; don't clean it up.
2. **Write in English when you can** (it's also English practice); Chinese glosses welcome.
3. **Always include the Takeaway** — that's the part that helps strangers.
4. Keep cards **short**: 5–10 minutes to write, scannable in 30 seconds.

## Index

| # | Problem | Root cause (one line) | Phase |
|---|---|---|---|
| 001 | [`head 10` → `cannot open '10'`](001-argument-without-dash.md) | Command args need `-`; a bare word is a filename | 1 / W1 |
| 002 | [`cut -d,` splits quoted CSV fields wrong](002-csv-quotes-cut.md) | CSV quotes escape commas; text tools don't parse syntax | 1 / W1 |

---

*New cards: copy `001-*.md`'s structure, number by next integer, add a row here.*
