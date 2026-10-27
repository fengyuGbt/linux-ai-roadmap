# systemd Cheatsheet — the "Housekeeper & Phone" model

> Companion to journal/2026-09-w3.md. Quick reference for `systemctl` grouped by what it controls.

## The model in one line

`systemd` (PID 1) is the system's housekeeper that starts and supervises all services; `systemctl` is your phone to give it orders. Verbs group by **time dimension**, not by function:

| Dimension | Verbs | What they control | sudo? |
|---|---|---|---|
| 🔍 Observe (查房) | `status` `is-active` `is-enabled` `list-units` | How is it now? Is it running? Auto-start at boot? | ❌ read-only |
| ⚡ Today (今天) | `start` `stop` `restart` `reload` | Run / stop / restart **right now** | ✅ write |
| 📅 Tomorrow (明天) | `enable` `disable` `enable --now` | Start automatically at **boot** | ✅ write |

Memory hook: **start/stop 管今天，enable/disable 管明天，status 管查房。**
Read ops are free; anything that *changes* system state needs root (see pitfalls/004 — WSL has no polkit agent).

## Observe — no sudo

```bash
systemctl status cron                       # full status + recent journal
systemctl is-active ssh                     # active | inactive
systemctl is-enabled cron                   # enabled | disabled
systemctl list-units --type=service --state=running   # only running services
```

## Today — sudo

```bash
sudo systemctl start cron                   # start now
sudo systemctl stop cron                    # stop now
sudo systemctl restart ssh                  # restart (most common: after config change)
sudo systemctl reload nginx                 # hot-reload config, no service interruption
```

## Tomorrow — sudo

```bash
sudo systemctl enable cron                  # start at boot
sudo systemctl disable cron                 # don't start at boot
sudo systemctl enable --now cron            # ⭐ start now AND at boot, one shot
```

## Real-life scenes

- **SSH won't connect** → `systemctl status ssh` → see `Active: failed` + journal reason.
- **Edited `/etc/ssh/sshd_config`** → `sudo systemctl restart ssh` for it to take effect.
- **Your Python service must stay up** → `sudo systemctl enable --now myapp` — runs today and every boot.

## Status lamp

Line 1 of `systemctl status`: **● = active (running), ○ = inactive (dead)**.
`exited` is NOT an error — it's the normal end state of one-shot tasks (e.g. `console-setup.service`).
