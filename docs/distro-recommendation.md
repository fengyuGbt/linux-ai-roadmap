# Linux Distribution Recommendation for AI Development

**Bottom line: use Ubuntu 24.04 LTS (Noble Numbat).** It is the safest, best-supported choice for AI/LLM and data work, and it matches the environment this roadmap was built and tested on (a WSL2 Ubuntu 24.04 workstation).

---

## Why Ubuntu for AI/LLM work

1. **Ecosystem gravity.** Vendor docs, NVIDIA's official CUDA repository, PyTorch/TensorFlow install pages, Hugging Face guides, cloud images, and nearly every AI tutorial assume Ubuntu first. When you hit an error, the answer you find online will match your system.
2. **Stability + long support.** Ubuntu 24.04 LTS gets standard security maintenance until **April 2029** (expanded until 2034). You install once and forget about upgrades for years — ideal while learning.
3. **The AI stack is Ubuntu-tuned.** NVIDIA ships a dedicated `apt` repo for Ubuntu; PyTorch's CUDA wheels are built and tested on Ubuntu; Docker images used in AI are predominantly Ubuntu-based.
4. **Beginner-friendly but professional.** The same distro runs your laptop and your cloud server, so skills transfer 1:1 from local learning to production deployment.

### What about the newer Ubuntu 26.04 LTS?

Ubuntu 26.04 LTS ("Resolute Raccoon") was released **April 2026** with support until May 2031. It is a fine choice for new machines, and this roadmap works identically on it. However:

- 24.04 has the most battle-tested driver/tooling combinations (NVIDIA + CUDA + PyTorch) right now.
- AI documentation and community answers overwhelmingly target 24.04 LTS today.
- Nothing you learn is wasted: upgrading 24.04 → 26.04 later is routine.

**If you already have Ubuntu 24.04 running — keep it. If you're installing fresh and want the longest runway, 26.04 LTS is also a perfectly good choice.** This roadmap is version-agnostic within the LTS family.

---

## Comparison of top candidates

| Distro | AI/ML ecosystem | Difficulty | Stability | Docs & community | NVIDIA experience | Best for |
|---|---|---|---|---|---|---|
| **Ubuntu 24.04 LTS** | ★★★★★ | Easy | Very high (LTS) | Excellent | Excellent (official repo) | **Default choice for AI dev & servers** |
| Ubuntu 26.04 LTS | ★★★★☆ | Easy | High (new LTS) | Good | Good (newer kernel, ecosystem settling) | Fresh installs wanting longest support |
| Fedora Workstation | ★★★★☆ | Medium | Medium (fast-moving) | Good | Good (needs RPM Fusion for some drivers) | Developers who want newest kernels/toolchains |
| Pop!_OS | ★★★★☆ | Easy | High (Ubuntu base) | Medium | Excellent (NVIDIA ISO preinstalled) | NVIDIA-only desktop users who hate driver setup |
| Debian stable | ★★★★☆ | Medium | Very high | Very good | Medium (older driver packages) | Maximum-stability servers, minimal installs |
| Arch / CachyOS | ★★★★★ | Hard | Rolling (self-maintained) | Good (wiki is superb) | Good (AUR, newest CUDA/ROCm) | Learning Linux internals deeply, tinkerers |

---

## Picks by scenario

| Your situation | Pick |
|---|---|
| Windows machine, want to learn AI/LLM on Linux fast | **WSL2 + Ubuntu 24.04 LTS** (see environment-setup) |
| Dedicated Linux workstation/laptop, want zero friction | **Ubuntu 24.04 LTS** (or Pop!_OS if NVIDIA-only and driver-averse) |
| Cloud server for deployment (Phase 5) | **Ubuntu 24.04 LTS server image** (cheapest, best-supported) |
| Want to understand Linux internals by living in them | Try Arch in a VM after finishing Phase 4 |
| Old hardware / minimal footprint | Debian stable |

---

## How this recommendation was made

- Published distro guides for AI/ML in 2026 consistently rank Ubuntu first for ecosystem fit and documentation density, with Fedora as the "cutting edge" alternative and Pop!_OS as the NVIDIA-friendly option.
- Ubuntu's official release-cycle page confirms 24.04 LTS support through April 2029 and 26.04 LTS through May 2031 (standard security maintenance).
- The environment this roadmap is built and tested on: **Ubuntu 24.04 LTS under WSL2**, with Docker Desktop available — the exact stack recommended here.

> Sources: Ubuntu release cycle (ubuntu.com/about/release-cycle), Canonical 26.04 LTS announcement, and 2026 community distro-for-AI guides. Links live in [resources.md](resources.md).
