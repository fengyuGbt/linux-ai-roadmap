# Linux × AI — 12-Week Learning Roadmap

The complete plan. Start at your current level, move at your own pace, and journal every week.

---

## At a glance

| Phase | Weeks | Focus | AI / data tie-in | Hands-on project |
|---|---|---|---|---|
| 0 | Day 0 | Environment setup | — | A working Linux shell |
| 1 | 1–2 | Linux fundamentals | Dataset file & permission hygiene | `~/lab` workspace + first script |
| 2 | 3–4 | Shell & automation | Automating data pipelines | Dataset processing script |
| 3 | 5–6 | Python data environment | venv/uv, Jupyter, pandas on Linux | Local data-analysis project |
| 4 | 7–9 | Local AI/LLM stack | GPU/CUDA, Ollama, Hugging Face | Local LLM + RAG demo |
| 5 | 10–12 | Deployment & operations | Serving AI, cloud VPS, CI/CD | Deploy the RAG service publicly |
| ∞ | ongoing | Habits: reading, journaling, blogging | dev.to / Zhihu articles | — |

---

## How to use this plan

- **One hour of reading → two hours at the terminal.** Hands-on beats theory every time.
- **The projects are the curriculum.** If you have limited time, do the projects first and read what you need along the way.
- **Journal every week** (`journal/template.md`). Write in English when you can — it is part of the training.
- **Don't skip Phase 0.** A broken environment will cost more time later than the 1–2 hours it takes to set it up properly.

---

## Weekly rhythm (recommended)

| Day | Activity |
|---|---|
| Mon / Wed / Fri | 1–2h focused study + terminal practice |
| Sat | Project work (the week's hands-on task) |
| Sun | Journal + plan next week + (optional) blog outline |

---

## Phase 0 — Environment Setup (Day 0)

**Goal:** have a working Ubuntu 24.04 shell you can reach from your daily machine, plus Git configured.

Actions:

1. Follow [`docs/environment-setup.md`](docs/environment-setup.md) and get one environment ready:
   - **WSL2** on Windows (recommended for daily AI development), **VM**, or **cloud VPS** — pick one that matches your hardware.
2. Run the post-install checklist: `sudo apt update && sudo apt upgrade`, Git identity, SSH key, `htop`, `tmux`.
3. Learn your first three commands for real: `pwd`, `ls`, `cd` — and read their `man` pages (English!).

**English task:** read the `man ls` page and note down 5 options you didn't know.

✅ **Acceptance:** you can open a terminal, print your working directory, list files with human-readable sizes (`ls -lh`), and create/delete a test directory.

---

## Phase 1 — Linux Fundamentals (Weeks 1–2)

**Goal:** own the file system, file operations, permissions, users, and package management — the daily toolbox of every AI engineer.

**Why it matters for AI:** model weights, datasets, and logs are files. Knowing where they live (`/data`, `~/models`, `/var/log`), how to protect them (`chmod`/`chown`), and how to install GPU/driver packages (`apt`) is the difference between a 5-minute fix and an afternoon of pain.

### Week 1 — Files, paths, and navigation

- **Learn**
  - File system hierarchy: `/`, `/home`, `/etc`, `/var`, `/tmp`, `/usr`, `/opt`, `/mnt`, `/data`
  - Navigation & inspection: `pwd`, `ls -la`, `cd`, `find`, `locate`, `du`, `df`, `file`
  - Reading & editing: `cat`, `less`, `head`, `tail`, `grep`, `nano` / `vim` basics
  - Absolute vs relative paths, `~`, `.`, `..`, wildcards `*` `?` `[]`
- **Practice**
  - Explore `/etc` — pick 3 config files and read them with `less`
  - Create `~/lab/{data,scripts,models,logs}` — this will be your permanent workspace
  - `find ~/lab -type f -name "*.txt"` drills; count lines with `wc -l`
- **Project (part 1):** copy a real dataset into `~/lab/data` (e.g. a CSV from Kaggle or a Hugging Face dataset via `huggingface-cli`), inspect it with `head`/`wc`/`grep`/`cut`, and write down its shape by hand.
- **English task:** read `man find`; write 3 sentences in your journal about what `-name` vs `-iname` do.

### Week 2 — Permissions, users, packages

- **Learn**
  - Permission model: `r w x`, owner/group/others, `chmod` (octal & symbolic), `chown`, `umask`
  - Users & groups: `whoami`, `id`, `sudo`, `/etc/passwd` basics, `su`
  - Package management: `apt update/upgrade/search/install/remove`, `dpkg -l`, `snap`
  - Editors: `vim` survival mode (open, insert, save, quit) or `nano`
  - Help system: `man`, `info`, `tldr`, `--help`
- **Practice**
  - Lock down `~/lab` so only your user can write: `chmod -R u=rwX,go=rX ~/lab` — understand why
  - Install a tool with `apt` (e.g. `htop`), search for a package, remove it, reinstall
  - Use `vimtutor` once (built into vim installs) to reach "survivable" level
- **Project (part 2):** give the dataset from Week 1 the right permissions (read-only for others), create a `scripts/` dir owned by you, and write a one-line README inside `~/lab`.
- **English task:** explain `chmod 750` in your journal in 2–3 English sentences.

✅ **Acceptance (end of Phase 1):** you can describe what every part of `ls -l` output means, set file permissions on purpose, install/uninstall packages, and look up any command's manual in English.

---

## Phase 2 — Shell & Automation (Weeks 3–4)

**Goal:** write shell one-liners and scripts that automate data work, understand processes and services, and use SSH/Git from the command line confidently.

**Why it matters for AI:** fine-tuning, data pulls, and model serving are all long-running or scheduled jobs. Pipes, `cron`, and `systemd` are how real ML systems stay alive and keep data fresh.

### Week 3 — Pipes, processes, and bash scripting

- **Learn**
  - Pipes & redirection: `|`, `>`, `>>`, `<`, `2>`, `tee`
  - Text tools: `grep`, `sed`, `awk`, `sort`, `uniq`, `cut`, `xargs` (one practical example each)
  - Processes: `ps aux`, `top`/`htop`, `kill`, `killall`, background jobs `&`, `nohup`, `jobs`, `fg`/`bg`
  - Bash scripting: shebang, variables, `$@`/`$?`, `if`/`for`, functions, exit codes
- **Practice**
  - Build a one-liner: find the 10 largest files in `~/lab` sorted by size
  - Write your first script `~/lab/scripts/backup.sh` that copies `data/` to `backup/` with a timestamp, and make it executable (`chmod +x`)
  - Start a long-running job with `nohup` and check it with `htop`
- **Project:** write `process_dataset.sh` that takes a CSV, counts rows, drops the header, and outputs a cleaned version — then run it through a pipeline of `sed`/`awk`/`sort` on a real file.
- **English task:** read the Bash manual's "Redirections" section; write down 3 redirection tricks you now use.

### Week 4 — systemd, scheduled jobs, SSH, Git

- **Learn**
  - Services: `systemctl status/start/enable`, `journalctl -u`, writing a simple `.service` unit
  - Scheduling: `cron` (`crontab -e`) vs systemd timers (why timers are preferred in modern systems)
  - SSH: key generation (`ssh-keygen`), `ssh-copy-id`, `~/.ssh/config`, `scp`/`rsync`
  - Git from the shell: `status`, `add`, `commit`, `log --oneline`, `branch`, `remote`, `push`/`pull`
- **Practice**
  - Create a systemd service that runs your backup script daily (or a timer), verify with `journalctl`
  - Set up an SSH key and connect to a second machine (e.g. your cloud VPS or another box)
  - Initialize `~/lab` as a Git repo, make 3 commits with meaningful messages
- **Project:** schedule `process_dataset.sh` to run every Monday morning; push `~/lab` to a private GitHub repo as a backup.
- **English task:** write a 5-line commit history summary in your journal, describing what each commit did — in English.

✅ **Acceptance (end of Phase 2):** you can build a shell pipeline without Googling every flag, explain how a systemd unit works, run a scheduled job, and push a repo to GitHub from the terminal.

---

## Phase 3 — Python Data Environment on Linux (Weeks 5–6)

**Goal:** a clean, reproducible Python data stack on Linux: environment managers, Jupyter, and everyday pandas workflows.

**Why it matters for AI:** "it works on my laptop" is the #1 ML failure mode. Environment isolation (`venv`/`uv`) and knowing where Python lives on Linux prevent most dependency hell.

### Week 5 — Python environments and the data stack

- **Learn**
  - Python on Linux: `which python3`, system vs venv, why you never `pip install` globally
  - `venv`, `uv` (modern, fast) or `conda`/`micromamba` — pick one and use it consistently
  - Environment variables: `PATH`, `export`, `.env` files, `env` command
  - Jupyter on Linux: install in a venv, run headless, connect from browser/VS Code
  - Files & paths in Python: `pathlib`, `os`, reading/writing CSV efficiently
- **Practice**
  - Create `~/lab/.venv` with `uv`, install `numpy pandas jupyter`
  - Run Jupyter without a GUI browser tab → connect from your host machine
  - Write a Python script that walks `~/lab/data` with `pathlib` and prints file sizes
- **Project:** a mini data-analysis script: load the Week 1 CSV, clean it (dropna/rename), compute summary stats, and export a small report — all from the Linux terminal.
- **English task:** read the `uv` README (English); write 3 notes about what `uv` does differently from `pip`.

### Week 6 — Data workflows and Docker (first contact)

- **Learn**
  - Data hygiene on disk: naming conventions, `parquet` vs `csv`, dataset versioning basics
  - `git-lfs` for large files
  - Docker concepts ONLY at the level you need: image vs container, `docker run`, `docker ps`, volumes, ports — don't go deeper yet
  - Why containers are the standard delivery format for AI apps
- **Practice**
  - Run a throwaway container: `docker run --rm -it ubuntu:24.04 bash` — look around, exit
  - Convert one dataset to parquet with pandas and compare file sizes
  - Commit your venv's `requirements.txt` (or `uv.lock`) to the repo
- **Project:** wrap your data-analysis script in a tiny Dockerfile and run it in a container.
- **English task:** write a paragraph in your journal: "Why I will use Docker for AI projects" (your own words, English).

✅ **Acceptance (end of Phase 3):** you can create a reproducible Python environment on Linux from scratch, run Jupyter, process a real dataset, and explain in plain English why containers matter.

---

## Phase 4 — Local AI/LLM Stack (Weeks 7–9)

**Goal:** run and serve real AI models on Linux: GPU acceleration (or a sensible CPU path), local LLMs, and a small retrieval-augmented generation (RAG) system.

**Why it matters for AI:** this is where Linux stops being a "system to learn" and becomes your AI workstation. GPU drivers, model servers, and vector databases are all Linux-first.

### Week 7 — GPU acceleration (or the CPU fallback)

- **Learn**
  - Check your hardware: `lspci | grep -i nvidia`, `nvidia-smi` (or `rocminfo` for AMD)
  - NVIDIA driver + CUDA toolkit basics on Ubuntu: official repo, `nvidia-smi`, `nvtop`
  - If no GPU (or WSL without GPU passthrough): make the CPU path explicit — what runs fine on CPU (most small LLMs, embeddings, pandas)
  - PyTorch GPU check: `python -c "import torch; print(torch.cuda.is_available())"`
- **Practice**
  - Get `nvidia-smi` showing your GPU, or document your CPU-only plan honestly
  - Install PyTorch (CUDA or CPU wheel) in your venv and verify with the one-liner above
- **English task:** read NVIDIA's "CUDA Installation Guide for Linux" intro (or PyTorch install page) and write a 3-line summary.

### Week 8 — Running local LLMs

- **Learn**
  - Ollama: `ollama pull`, `ollama run`, model size vs memory tradeoffs
  - What quantization means (`q4`, `q8`, GGUF) and why local models are cheap
  - Serving basics: Ollama's REST API, `curl` a completion
  - Alternatives on the menu: `llama.cpp`/`llama-server`, `vLLM` (when you need throughput)
- **Practice**
  - Pull a small model (e.g. `qwen2.5:7b` or `llama3.2:3b`), chat with it in the terminal
  - Call the same model via `curl` from a script
- **Project (part 1):** a Python script that asks the local model a question and prints the answer.

### Week 9 — Hugging Face ecosystem + a real RAG project

- **Learn**
  - Hugging Face `transformers`/`datasets` on Linux: download a model, run inference
  - Embeddings: sentence-transformers (or Ollama embeddings)
  - Vector store basics: FAISS or Chroma — what an index is, why similarity search
  - RAG at a high level: retrieve → augment → generate
- **Project (the capstone):** build **`~/lab/rag-demo`**:
  1. Ingest 5–10 documents (or a wiki dump) into a vector index
  2. Ask questions, get answers with sources
  3. Run it locally with Ollama + embeddings
- **English task:** write a short `README.md` for `rag-demo` explaining the architecture in English — this becomes your first blog post draft.

✅ **Acceptance (end of Phase 4):** you can run a local LLM, serve it over HTTP, and answer questions from your own documents with a working RAG pipeline.

---

## Phase 5 — Deployment & Operations (Weeks 10–12)

**Goal:** ship an AI service that stays up: FastAPI, systemd, a cloud Linux server, security basics, and CI/CD.

**Why it matters for AI:** local demos are nice; services that survive restarts, users, and bad code are the job. This phase turns you from "can run models" into "can run a model-backed product."

### Week 10 — Serving AI with FastAPI + systemd

- **Learn**
  - FastAPI basics: route, request/response models, `uvicorn`
  - Expose your Week 9 RAG as a small HTTP API
  - Run it as a systemd service: working dir, environment, restart on failure
  - `journalctl -u my-rag -f` for logs
- **Practice**
  - `curl` your API from another machine
  - Kill the service and watch systemd restart it
- **English task:** write the systemd unit file and comment every line in English.

### Week 11 — Cloud Linux server & hardening

- **Learn**
  - Rent the cheapest cloud Linux box (or reuse a home server): Ubuntu 24.04 LTS
  - SSH hardening: key-only auth, disable root login, change port (or not), `fail2ban`
  - Firewall: `ufw` allow ssh + your app port only
  - Reverse proxy basics: Nginx → uvicorn (why you need it: TLS, load balancing)
- **Practice**
  - Deploy your RAG API to the cloud server behind Nginx
  - Test a restart, check `ufw status`, read `fail2ban` logs
- **English task:** write a 10-line `DEPLOY.md` in English describing the deployment steps.

### Week 12 — Monitoring, backup, CI/CD

- **Learn**
  - Monitoring: `htop`, `glances`, `journalctl`, disk with `df -h`
  - Backups: `rsync` + cron/timer, off-site copy of important repos
  - CI/CD first contact: GitHub Actions — run a lint/test job on every push
  - (Optional) auto-deploy: webhook or Action that redeploys on merge to `main`
- **Practice**
  - Add a GitHub Action that runs a Python lint + test on your `rag-demo`
  - Schedule a nightly `rsync` backup of `~/lab` (or push journal to this repo)
- **Project finale:** your RAG service is live on a public URL, with monitoring and a CI pipeline. Write the "lessons learned" post.
- **English task:** publish the RAG `README` + lessons to **dev.to** (English), and a Chinese version to **Zhihu**.

✅ **Acceptance (end of Phase 5):** you can deploy an AI service to a real Linux server, keep it running and secure, back it up, and describe the whole stack in English.

---

## Ongoing habits (after Week 12)

- **Daily:** keep one terminal workflow that uses Linux daily (even if only for Git + Jupyter).
- **Weekly:** journal; read one English man page or official doc; try one new command.
- **Monthly:** publish one article (English → dev.to, Chinese → Zhihu) based on your journal.
- **Quarterly:** revisit this roadmap — pick a deeper track (kernel, networking, k8s, MLOps).

---

## Skill self-assessment checklist

Copy this into your journal and tick boxes as you go.

### Phase 1
- [ ] I can read `ls -l` output completely
- [ ] I can `chmod`/`chown` files for a purpose
- [ ] I can install and remove packages with `apt`
- [ ] I read `man` pages in English without giving up

### Phase 2
- [ ] I can chain `grep`/`sed`/`awk`/`sort` in a pipeline
- [ ] I can write and run a bash script with arguments
- [ ] I can create a systemd service and check logs
- [ ] I can schedule jobs with cron or a timer
- [ ] I can push/pull with Git from the shell

### Phase 3
- [ ] I can create a clean venv/uv environment from memory
- [ ] I can run Jupyter headless on Linux
- [ ] I can load/clean/analyze a real dataset in pandas
- [ ] I can run a throwaway Docker container

### Phase 4
- [ ] I can verify GPU/CUDA status (or justify CPU)
- [ ] I can run a local LLM via Ollama and via API
- [ ] I built a working RAG demo over my own documents

### Phase 5
- [ ] I can serve a model with FastAPI under systemd
- [ ] I hardened an SSH server and configured `ufw`
- [ ] I deployed behind Nginx on a real Linux box
- [ ] I have a CI job and a backup routine

---

## Contributing

Found a broken link? A better exercise? A great resource? Open an issue or submit a PR — this roadmap is meant to be improved by the community.
