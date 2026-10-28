# 005. `docker run` fails to pull: "A specified logon session does not exist" (Docker Desktop + WSL)

> Date: 2026-10-28 ・ Phase 1 / Week 4 ・ Status: ✅ resolved

## 现象 / Symptom

```bash
$ docker run -v /:/host -it ubuntu bash
Unable to find image 'ubuntu:latest' locally
docker: error getting credentials - err: exit status 1, out: `A specified logon session does not exist. It may already have been terminated.`
```

Even pulling a **public** image from Docker Hub fails.

## 操作 / What I did

From inside WSL (Ubuntu-24.04), ran a normal `docker run` that needed to pull `ubuntu:latest`. The engine itself was fine (`docker ps` worked, existing containers running) — only image pulls broke.

## 排查 / Debugging

- `docker ps` → normal → the daemon is not the problem.
- `grep credsStore ~/.docker/config.json` → found `"credsStore": "desktop.exe"`.
- That setting tells the docker CLI to fetch registry credentials from the **Windows credential manager** via Docker Desktop's helper (`desktop.exe`).
- From a WSL session, the helper tries to open a Windows credential-manager logon session; in this setup that session is unavailable → the whole pull aborts, even though public images need no credentials at all.

## 根因 / Root cause

A configured credential helper is **consulted on every registry access, public or not**. Docker Desktop + WSL bridges credentials through the interactive Windows logon session; when that session context is missing (e.g. Docker Desktop started from a non-interactive context), the helper dies with "logon session does not exist" and takes the pull down with it.

## 解法 / Fix

```bash
# 1. back up
cp ~/.docker/config.json ~/.docker/config.json.bak
# 2. remove the "credsStore" line (keep "auths" untouched)
python3 -c 'import json,os;p=os.path.expanduser("~/.docker/config.json");d=json.load(open(p));d.pop("credsStore",None);json.dump(d,open(p,"w"),indent=2)'
# 3. verify end-to-end
docker run --rm hello-world
```

Result: pull works, `auths` entries (e.g. a private Aliyun registry) are preserved. If you later `docker login` a private registry, credentials will be stored in `config.json` instead of the Windows vault — acceptable on a single-user machine.

## 通用化 / Takeaway

- Any docker error containing "credentials" → check `~/.docker/config.json` first: `credsStore` pointing at a Windows helper is the classic Docker Desktop + WSL mismatch.
- Public images don't need credentials; a broken *helper config* can still block them. When the helper is the problem, removing it is safe.
- The permission model again: `docker ps` (read, already-authed context) ≠ `docker pull` (needs registry auth path).
