# 004. `systemctl stop` fails: "Interactive authentication required" (WSL)

> Date: 2026-09-29 ・ Phase 1 / Week 3 ・ Status: ✅ resolved

## 现象 / Symptom

```bash
$ systemctl stop cron
Failed to stop cron.service: Interactive authentication required.
See system logs and 'systemctl status cron.service' for details.
```

But `systemctl status cron` and `systemctl is-enabled cron` work fine **without** sudo.

## 操作 / What I did

As a normal user in WSL, tried to `systemctl stop` / `systemctl start` a service. Read-only commands worked; write commands were refused.

## 排查 / Debugging

- Read-only commands (`status`, `list-units`, `is-enabled`) don't change system state → no privilege needed.
- `stop` / `start` / `restart` / `enable` / `disable` **change system state** → require root (or a polkit authentication agent).
- On a normal Ubuntu desktop, a GUI polkit prompt pops up and authenticates you. **WSL has no such graphical agent**, so the non-interactive attempt fails with "Interactive authentication required".

## 根因 / Root cause

The permission model again: **read ops are free, write ops need root.** WSL's systemd has no polkit agent to grant the elevation interactively, so the only path is explicit `sudo`.

## 解法 / Fix

```bash
sudo systemctl stop cron
sudo systemctl start cron
```

Password typo → `Sorry, try again.` is normal; sudo never reveals *where* you were wrong (anti brute-force design).

## 通用化 / Takeaway

- Before running a system command, ask: **read or write?** `status`/`list` are reads; anything that *changes* state needs root.
- WSL-specific: expect `Interactive authentication required` for any privileged write; reach for `sudo` directly instead of hunting for a polkit agent.
- `systemctl status` line 1: **● = active, ○ = inactive** — the dot is a status lamp.
- `ps aux | grep bash` shows multiple bash instances = one program, many processes (one per terminal).
