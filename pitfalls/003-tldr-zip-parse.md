# 003. `tldr` fails: "Did not find end of central directory signature"

> Date: 2026-09-20 ・ Phase 1 / Week 2 ・ Status: ✅ resolved

## 现象 / Symptom

```bash
$ tldr find
Downloading tldr pages to /home/erp/.local/share/tldr
tldr: Data.Binary.Get.runGet at position 4: Did not find end of central directory signature
CallStack (from HasCallStack): ...
```

## 操作 / What I did

Installed tldr via `sudo apt install -y tldr` (the Haskell client, tldr-hs), then ran `tldr find` — it tried to download the page library and crashed while "unzipping" it.

## 排查 / Debugging

The error means: the downloaded data is **not a valid ZIP file** (no central-directory signature at the end). So the client fetched *something*, but it wasn't the archive.

Checked the default download source with `curl -I`:

```bash
curl -sI https://tldr.sh/assets/tldr.zip
# → HTTP/2 301  Location: https://tldr.sh/assets/tldr.zip/
curl -L -sI https://tldr.sh/assets/tldr.zip
# → HTTP/2 200  Content-Type: text/html   ← HTML, not zip!
```

The official source had **gone stale** — it now returns an HTML page (even after following the 301). The client unzipped that HTML → crash.

## 根因 / Root cause

A tool's default download source can silently become dead/redirected. The client trusted the response and tried to parse HTML as a ZIP. This is the same `-L`/redirect class of problem as pitfall in Week 1, but on the *server side*.

## 解法 / Fix

1. Download the archive from the canonical GitHub source instead (route through proxy, long timeout — it's 17MB):

```bash
curl -x <your-proxy> -L -o ~/tldr.zip -m 600 \
     https://codeload.github.com/tldr-pages/tldr/zip/refs/heads/main
file ~/tldr.zip   # → Zip archive data ✓
```

2. Populate the client's cache manually:

```bash
mkdir -p ~/.local/share/tldr
cp ~/tldr.zip ~/.local/share/tldr/tldr.zip
unzip -o -q ~/tldr.zip -d ~/.local/share/tldr     # yields tldr-main/
mv ~/.local/share/tldr/tldr-main/pages ~/.local/share/tldr/pages
rm -rf ~/.local/share/tldr/tldr-main
tldr find   # ✅ works
```

## 通用化 / Takeaway

- **"Not a valid ZIP/archive" errors almost always mean you downloaded the wrong content** (an HTML error page, a 404, or a redirect). Before debugging the tool, check what the server actually returned: `curl -I` for status/`Content-Type`, `file <download>` after fetching.
- **A tool's default upstream can die silently.** Manual cache population (`~/.local/share/<tool>/`) is a universal escape hatch.
- `codeload.github.com` does **not** support byte ranges → `curl -C -` resume fails with error 33. Download big archives in one shot with a generous `-m`.
- Long downloads through a slow proxy: give `-m 600` and run in the background, don't guess the default 60s timeout.
