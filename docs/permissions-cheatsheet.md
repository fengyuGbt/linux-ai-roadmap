# Permissions Cheatsheet — Groups as Keys, Two Channels to Root

> Companion to journal/2026-09-w2.md (permissions), docs/sudoers-cheatsheet.md (sudo tiers) and docs/systemd-cheatsheet.md (services). The full mental model, with the docker permission chain and its troubleshooting flow.

## 1. Permissions live on files, not on users

A user has **no global read/write rights**. For every file, the kernel answers one question: *which of the three buckets am I in?*

```
-rw-r--r--  1 root root 802  Aug  5 10:27 /etc/group
│││ ││││    │  │    │
│││ ││││    │  │    └ group (所属组) — if you are in this group, use middle triplet
│││ ││││    │  └ owner (属主) — usually the creator
│││ │││└ other (其他): r-- → read only
│││ └ group (组): r--   → members read only
│└ owner (属主): rw-    → owner can read+write
└ type: - file, d directory, s socket, l symlink
```

Check order: **am I the owner? → am I in the group? → otherwise "other" bucket.**

Your identity record (`getent passwd erp`):

```
erp : x : 1000 : 1000 : ,,, : /home/erp : /bin/bash
 │      │    │      │         │          │
name  pwd  uid    gid      home      login shell
```

- `uid` = who you are (owner matches)
- `gid` = **primary group** — the group your new files default to
- `groups` = **supplementary groups** — extra memberships (sudo, docker, …)
- Primary-group membership lives in `/etc/passwd` (gid field), **not** in the member list of `/etc/group`. That's why `getent group erp` shows an empty member list — normal.

Group file format: `groupname : password-placeholder : gid : members` — e.g. `sudo:x:27:erp`, `docker:x:1001:erp`.

## 2. Groups are KEYS, not roles

`sudo` group ≈ root, `docker` group ≈ root — but **the group is not root**. It is a *keycard*; root is the *door*. Remove erp from both groups and he instantly loses the power; root stays root.

Same user, two parallel channels:

```
channel ① sudo  card : sudo <cmd> → password → run ONE command as root
                        source of power: /etc/sudoers  %sudo ALL=(ALL:ALL) ALL
channel ② docker card : docker <cmd> → no password → root dockerd executes it
                        source of power: /var/run/docker.sock owned by group docker
```

Both lead to root; neither is root itself. Handing out a group membership = handing out a keycard, so treat it like an admin appointment.

## 3. The docker power chain (why "docker group ≈ root")

Three conditions must ALL hold:

| Condition | Check | You verified |
|---|---|---|
| ① socket exists | `ls -l /var/run/docker.sock` | `srw-rw---- 1 root docker` ✅ |
| ② you are in the group | `id` / `getent group docker` | `docker:x:1001:erp` ✅ |
| ③ engine responds | `docker info` / `curl --unix-socket ...` | JSON Version 29.6.2 ✅ |

When all three hold, every docker command runs with **root's hands** — your uid stays 1000, but dockerd (uid 0) executes for you. That is "indirect root power". Proof without trust:

```bash
id                                  # you: uid=1000
docker run --rm alpine id           # container: uid=0 (root!) — no sudo needed
docker run --rm -v /:/host:ro alpine ls /host/root   # read-only mount shows /root
```

Escape routes that make the chain scary (know them, don't run them on a server):
- `docker run -v /:/host ...` — mount the whole host
- `docker run --privileged ...` — almost all kernel capabilities
- `docker run -v /var/run/docker.sock:/var/run/docker.sock ...` — socket inside container = escape

Red lines:
- Never mount docker.sock into a container.
- Never expose the Docker API (port 2375) unauthenticated.
- Add users to the `docker` group as carefully as to `sudo`.

### How the power actually transfers — delegation, not elevation

You never "become" root — your uid stays 1000 throughout. The mechanism is **delegation**: a root process acts on your request.

```
erp: docker run -v /:/host -it ubuntu bash
   → ① docker CLI writes the request (JSON) into the socket
        (erp may — group permission bit rw- on group docker)
   → ② dockerd (uid=0) reads the request
   → ③ dockerd executes it AS ITSELF, with root privileges
        (creates the container, mounts /host = the whole host)
   → effect: a uid=1000 user got a root-level job done
```

The socket permission **is** the authentication: only root (owner) and docker members (group) can touch it, so dockerd trusts whoever reaches it — no extra password. Adding someone to the docker group hands them a *remote control for a root process*, not a root identity.

`sudo <cmd>` works the same way: a root child process runs the command for you. **Elevation** (your uid changes) and **delegation** (a root process acts on your request) are different mechanisms — Linux group-based tools mostly use delegation.

## 4. Docker troubleshooting — WSL vs Windows (two pairs of glasses)

Same machine, two views. If `docker` misbehaves in WSL, check the chain in order:

```
WSL:  ls -l /var/run/docker.sock        → socket missing? engine not running
Win:  Get-Process | ? Name -match Docker → no process? Docker Desktop not started
Win:  Get-Content "$env:APPDATA\Docker\settings-store.json"
      → EnableIntegrationWithDefaultWslDistro / IntegratedWslDistros
      (integration must list your distro, e.g. Ubuntu-24.04)
Start Docker Desktop → socket appears → docker version shows Server
```

Command translation table:

| Ask | WSL (Linux) | Windows (PowerShell) |
|---|---|---|
| docker version | `docker --version` | `docker.exe --version` |
| where is docker | `which docker` | `Get-Command docker` |
| engine running? | `systemctl is-active docker` / `pgrep dockerd` | `Get-Process \| ? Name -match 'Docker'` |
| socket | `ls -l /var/run/docker.sock` | (Windows uses named pipe `\\.\pipe\docker_engine`) |
| engine config | `/etc/docker/daemon.json` | `%APPDATA%\Docker\settings-store.json` |

docker subcommands (`ps` / `run` / `images`) are identical on both sides; only the system-level checks differ.

## 5. Related cards

- docs/sudoers-cheatsheet.md — the four tiers of sudo whitelisting + wrapper pattern
- docs/systemd-cheatsheet.md — systemctl three tenses (observe / today / tomorrow)
- pitfalls/004 — systemctl writes need sudo in WSL
