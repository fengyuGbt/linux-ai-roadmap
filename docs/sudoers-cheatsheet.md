# sudoers Cheatsheet — Four Tiers of Least Privilege

> Companion to journal/2026-09-w2.md (permissions) and journal/2026-09-w3.md (services). How to give people *exactly* the sudo power they need on a production server.

## The one-line syntax

```
erp   ALL   =   (ALL:ALL)   ALL
 │    │         │    │      │
 who  hosts   identity    commands
```

Memory hook: **谁 · 在哪 · 变成谁 · 能干啥** — tightening = changing only the last field.

- ① who: username, or `%group` (e.g. `%sudo`)
- ② hosts: `ALL` = anywhere, or a hostname
- ③ identity: `(ALL:ALL)` = any user:any group; `(root)` = only run as root
- ④ commands: `ALL`, a comma-separated whitelist, `NOPASSWD:`, or `!` negation

## The four tiers (wide → strict)

| Tier | Rule | Can do | Use for |
|---|---|---|---|
| ① Full | `erp ALL=(ALL:ALL) ALL` | everything | personal single machine only — **never on a shared/public server** |
| ② Whitelist | `erp ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl` | only listed commands | production ops accounts |
| ③ NOPASSWD | `erp ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart myapp` | one command, no password | CI/CD, deploy scripts, cron |
| ④ Negation | `erp ALL=(ALL) /usr/bin/apt, !/usr/bin/apt remove` | whitelist minus dangerous subcommands | fine-grained control |

Why tighten?
- **Least privilege**: give exactly enough, nothing more.
- **Accident-proof**: full sudo turns one `rm -rf /` slip into a disaster; a whitelist makes the dangerous command *unavailable*.
- **Breach containment**: a compromised account inherits only the whitelisted power, not root.

## The three traps

1. **Commands must be absolute paths** (`/usr/bin/apt`, not `apt`) — otherwise a fake `apt` earlier in PATH hijacks your sudo.
2. **The whitelist matches the command, not its arguments** — `apt` allowed still lets `sudo apt remove xxx` through (remove is an argument). Argument globs (`!/usr/bin/apt remove`) leak badly (`apt purge`, `dpkg -r`…).
3. **Last matching rule wins** — negation must come **last**:
   ```bash
   # ✗ wrong: the later ALL overrides the earlier !
   erp ALL=(ALL) !/usr/bin/apt remove, /usr/bin/apt
   # ✓ right
   erp ALL=(ALL) /usr/bin/apt, !/usr/bin/apt remove
   ```

## Production best practice: the wrapper-script pattern

Because command whitelists can't control arguments, wrap the risky operation in a script that does *exactly one thing*, and allow only that script:

```bash
# /usr/local/bin/restart-odoo.sh
#!/bin/bash
systemctl restart odoo
journalctl -u odoo --tail 50
```
```bash
# sudoers
erp ALL=(ALL) /usr/local/bin/restart-odoo.sh
```

Now there is no argument to abuse and no other command available. **"Encapsulate + allow the script" is the cleanest way to control dangerous operations.**

## Real-world walkthrough: giving an outsourced ops guy access

```
v1 (too wide):  ops ALL=(ALL) ALL                    # he can change your root password
v2 (reasonable):
   ops ALL=(ALL) /usr/bin/systemctl restart odoo,
                 /usr/bin/journalctl,
                 /usr/bin/apt install, !/usr/bin/apt remove
   ops ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart odoo   # for deploy scripts
v3 (tightest):  ops ALL=(ALL) /usr/local/bin/restart-odoo.sh
```

Decision flow: list the tasks he needs → allow exactly those → NOPASSWD the automated one → wrap the risky one in a script.

## Safe practice commands (read-only, on your machine)

```bash
sudo cat /etc/sudoers           # see the default file (%sudo line is your authorization)
ls -l /etc/sudoers.d/           # drop-in directory
sudo cat /etc/sudoers.d/README  # Ubuntu's official note
sudo -l                         # show exactly what YOU are allowed to run
```

## Editing rules

- **Always `sudo visudo -f /etc/sudoers.d/<name>`** — it runs a syntax check; one bad line breaks sudo for everyone (some distros then require root to recover).
- Verify with `sudo -l` right after editing.
- Use `Cmnd_Alias` to group commands when the list grows.
