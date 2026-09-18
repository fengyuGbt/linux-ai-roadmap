# Environment Setup Guide

Get one working Ubuntu 24.04 environment before starting the roadmap. Pick the row that matches your hardware; the first one is the recommended default for Windows users.

---

## Pick your environment

| Scenario | Setup | Pros | Cons |
|---|---|---|---|
| **Windows + WSL2** (recommended to start) | Install Ubuntu 24.04 inside WSL2 | 5–10 minutes, zero disk partitioning risk, seamless VS Code/Jupyter integration, CUDA-on-WSL2 supported | It's a VM-ish layer, not bare metal; GPU passthrough needs Windows driver setup |
| **Virtual machine** (VirtualBox/VMware) | Install Ubuntu 24.04 in a VM | Full Linux experience, isolated, snapshots/rollback | Performance overhead, more setup work |
| **Dual boot** | Partition disk, install Ubuntu alongside Windows | Full native performance, real hardware | Riskier (backup first!), you reboot to switch OS |
| **Cloud VPS** (Phase 5 anyway) | Rent an Ubuntu 24.04 server | Real server practice, public IP, accessible anywhere | Costs money; no desktop GUI |

> **If you already have Ubuntu running (WSL2, VM, or a machine) — just verify it's 24.04 LTS and move on.** `cat /etc/os-release`

---

## Path A — WSL2 (Ubuntu 24.04) on Windows

### 1. Install

```powershell
# PowerShell as Administrator
wsl --install -d Ubuntu-24.04
# or if WSL is already installed: 
wsl --update
wsl --install -d Ubuntu-24.04
```

First launch sets your Linux username/password. Remember both.

### 2. Post-install checklist

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential git curl wget htop tmux unzip tree

# Git identity
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# SSH key (used for GitHub/GitLab later)
ssh-keygen -t ed25519 -C "you@example.com"
```

### 3. Key WSL facts

- Windows files are under `/mnt/c/...`; keep **project files inside Linux** (`~/...`) for speed.
- WSLg gives you Linux GUI apps on Windows 11 automatically.
- **GPU (CUDA) in WSL2:** install the NVIDIA Windows driver (supports WSL2 CUDA), then inside WSL follow the standard CUDA toolkit setup — `nvidia-smi` works inside WSL2.
- Docker Desktop integrates with WSL2 (enable "Use the WSL 2 based engine").

### 4. Terminal upgrade (optional but recommended)

- Windows Terminal + **Oh My Zsh** or just plain `tmux` — your call, don't burn time on theming.

---

## Path B — Virtual Machine

1. Install VirtualBox (free) or VMware Workstation Player.
2. Download Ubuntu 24.04 LTS desktop ISO from ubuntu.com/download.
3. Create a VM: **4 GB+ RAM, 2+ CPUs, 40 GB+ disk** (AI tooling is hungry).
4. Boot ISO → Install Ubuntu → done.
5. Enable Guest Additions for copy/paste & screen resize.

---

## Path C — Dual boot (only if you need native performance)

1. **Back up everything. Disable/check BitLocker.** (Seriously.)
2. Shrink a partition in Windows Disk Management to free ≥ 60 GB.
3. Write the Ubuntu ISO to a USB (Rufus / balenaEtcher).
4. Boot from USB → "Install Ubuntu alongside Windows Boot Manager".
5. After install: `sudo update-grub`, boot menu will show both OSes.

---

## Path D — Cloud VPS (start when you hit Phase 5)

1. Pick any provider with an **Ubuntu 24.04 LTS** image (they all have it).
2. Add your **SSH public key** (`~/.ssh/id_ed25519.pub`) at creation time.
3. First login: `ssh ubuntu@<server-ip>` → then run the post-install checklist above.

---

## Verify your setup (must pass before Phase 1)

```bash
lsb_release -a          # Ubuntu 24.04.x LTS
uname -m                # x86_64 or aarch64
git --version           # any recent version
df -h                   # check free disk
htop                    # launches = OK
```

---

## Common issues

| Symptom | Fix |
|---|---|
| `wsl: command not found` | WSL not installed — run `wsl --install` as Admin, reboot |
| WSL won't start / error 0x80370102 | Enable virtualization in BIOS; `wsl --update` |
| `apt update` slow | Switch mirrors (e.g. to a local mirror) — one-time fix |
| No permission on `/mnt/c` files | Files belong to Windows; edit inside `~/` instead |
| `nvidia-smi` not found in WSL2 | Update **Windows** NVIDIA driver to latest; then install CUDA toolkit inside WSL |
| Keyboard/mouse weird in VM | Install Guest Additions / open-vm-tools |

---

> Environment ready? Start **Phase 1** of the [ROADMAP](../ROADMAP.md).
