# 002. `cut -d,` splits quoted CSV fields wrong

> Date: 2026-09-19 ・ Phase 1 / Week 1 ・ Status: ✅ resolved

## 现象 / Symptom

```bash
$ cut -d, -f2 world-cities.csv | sort -u | head
 Deyang
 Saitama"
 Santa Caterina i La Ribera"
"Bolivia
"Bonaire
```

Quote fragments (`"Bolivia`, `Saitama"`) leaked into the country column, and `-f5` (5th column) returned numbers even though the file only has 4 columns.

## 操作 / What I did

Used `cut -d, -f5` to extract the 5th column, assuming the CSV was plain comma-separated. Then `-f2` to get `country`.

## 排查 / Debugging

- `head -5` showed the header: `name,country,subcountry,geonameid` — only **4** columns.
- The quote fragments meant some fields themselves contain commas, and the CSV protects them with double quotes.
- Example shape: `"Saitama, Saitama",Japan,Saitama,1148425` — the comma *inside quotes* is part of the field, not a separator.
- `cut` doesn't know this rule: it splits on **every** comma, so quoted fields get cut in half, columns shift, and quote characters remain as data.

## 根因 / Root cause

CSV has a quoting syntax (RFC 4180): fields containing commas/newlines/quotes are wrapped in `"..."`, and inner quotes are doubled. `grep`/`cut`/`awk` are **plain-text tools** — they split on characters, not CSV syntax. The quotes are *syntax*, but text tools treat them as *data*.

## 解法 / Fix

Use a parser that understands CSV — Python's stdlib works, zero install:

```bash
cd ~/lab/data
python3 -c "
import csv
rows = list(csv.DictReader(open('world-cities.csv')))
print('total:', len(rows))                       # 34128
print('China cities:', sum(1 for r in rows if r['country'] == 'China'))   # 2122
print('countries:', len({r['country'] for r in rows}))                    # 244
"
```

`csv.DictReader` reads the header, strips quote syntax, and lets you address columns by name (`r['country']`) — solving both the quoting problem *and* the "grep matches whole rows" problem.

## 通用化 / Takeaway

- **Know your tool tier:** plain-text tools (`grep`/`cut`/`head`) are for *text*; structured data (CSV/JSON) needs a *parser* (`csv` module, `pandas` from Week 3).
- **Quick smell test:** if `head -5` of a CSV shows `"` in unexpected places, expect quote-escaping → don't trust `cut -d,`.
- **Column addressing by name** (`r['country']`) beats by number (`-f2`): headers change, names rarely do.
- Same principle scales: SQL for databases, `jq` for JSON, parsers for anything with syntax. Learn which tool "speaks the language" of the data.
