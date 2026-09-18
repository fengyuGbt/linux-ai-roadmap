# Linux × AI Learning Roadmap

A community-shared, AI-first Linux learning path, built by a Python developer heading toward **AI/LLM & data analysis** — and open for anyone who wants to learn Linux with the same mindset.

> 🐧 Learn Linux the way AI engineers actually use it.
> 📖 Every phase is tied to a real AI / data workflow, not abstract exercises.
> 🌍 Track progress in public: weekly journals, hands-on projects, and a live roadmap.

---

## Why this repo exists

Most Linux tutorials teach commands in a vacuum. This roadmap is different:

| Pillar | What it means in practice |
|---|---|
| **AI-first** | Every concept (files, permissions, processes, containers) is taught through an AI/ML or data-analysis workflow |
| **English-first** | Official docs, `man` pages, and journals are in English — double duty as technical-English training |
| **Learn in public** | Weekly journal entries are designed to be shared and to feed blog posts (dev.to / Zhihu) |

The plan is 12 weeks, 5 phases, plus an ongoing "habits" track. It assumes no prior Linux experience and works on a laptop, a WSL2 environment, or a cloud server.

---

## Repository structure

| Path | Description |
|---|---|
| [`ROADMAP.md`](ROADMAP.md) | The 12-week learning plan (English) |
| [`ROADMAP.zh-CN.md`](ROADMAP.zh-CN.md) | 中文学习路线（简体中文版） |
| [`docs/distro-recommendation.md`](docs/distro-recommendation.md) | Why Ubuntu 24.04 LTS, with an alternative-distro comparison |
| [`docs/environment-setup.md`](docs/environment-setup.md) | WSL2 / VM / dual-boot / cloud-VPS setup guides |
| [`docs/resources.md`](docs/resources.md) | Curated books, docs, interactive labs & communities |
| [`docs/roadmap-visualization.html`](docs/roadmap-visualization.html) | Visual timeline of the whole plan (open in any browser) |
| [`journal/`](journal/) | Weekly learning journal template & entries |

---

## The plan at a glance

| Phase | Weeks | Focus | AI / data tie-in |
|---|---|---|---|
| 0 | Day 0 | Environment setup | — |
| 1 | 1–2 | Linux fundamentals | file & permission hygiene for ML datasets |
| 2 | 3–4 | Shell & automation | automating data pipelines, cron pulls, SSH |
| 3 | 5–6 | Python data environment on Linux | venv/uv, Jupyter, pandas, Docker intro |
| 4 | 7–9 | Local AI/LLM stack | CUDA/GPU, Ollama, Hugging Face, RAG project |
| 5 | 10–12 | Deployment & operations | serving AI with FastAPI, cloud VPS, CI/CD |
| ∞ | ongoing | English reading, journaling, blogging | dev.to / Zhihu articles |

---

## How to use this repo

1. **Read [`ROADMAP.md`](ROADMAP.md)** and pick your starting phase (most beginners start at Phase 0).
2. **Set up your environment** with [`docs/environment-setup.md`](docs/environment-setup.md).
3. **Do the hands-on projects** — they are the real curriculum, not the reading lists.
4. **Journal weekly** using [`journal/template.md`](journal/template.md). Writing in English is encouraged; mistakes are welcome.
5. **Share and contribute**: fork it, open issues, submit PRs, or just star the repo.

---

## Learning principles

- **Concepts through workflows** — learn `chmod` while securing an ML dataset, learn systemd while keeping your LLM service alive.
- **Boring is beautiful** — stick to the LTS distro and the default tooling until you have a reason to leave it.
- **Read once, do twice** — for every hour of reading, spend two hours at the terminal.
- **Teaching is learning** — every phase ends with a short piece you can turn into a blog post.

---

## License

MIT — see [LICENSE](LICENSE). Learn in public, share freely.
