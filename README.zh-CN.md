# Linux × AI 学习路线图（中文版）

一个面向社区的、以 **AI 为核心导向** 的 Linux 系统学习路线，由一位正在走向 **AI/LLM 与数据分析** 方向的 Python 开发者发起——也欢迎所有想以同样方式学习 Linux 的人加入。

> 🐧 像 AI 工程师真正使用的那样学 Linux。
> 📖 每个阶段都绑定真实的 AI / 数据工作流，而不是空洞的练习题。
> 🌍 公开学习：每周学习日志、动手项目、实时更新的路线图。

---

## 这个仓库解决什么问题

大多数 Linux 教程在真空中教命令。本路线图不同：

| 支柱 | 落地方式 |
|---|---|
| **AI 优先** | 每个概念（文件、权限、进程、容器）都通过 AI/ML 或数据分析工作流来学 |
| **英语优先** | 官方文档、`man` 手册、日志都用英文——顺便完成技术英语训练 |
| **公开学习** | 每周日志天然适合分享，并可直接改写成博客文章（dev.to / 知乎） |

计划共 12 周、5 个阶段，外加一条长期"习惯线"。不要求任何 Linux 基础，在笔记本、WSL2 环境或云服务器上都可执行。

---

## 仓库结构

| 路径 | 说明 |
|---|---|
| [`ROADMAP.md`](ROADMAP.md) | 12 周学习计划（英文版） |
| [`ROADMAP.zh-CN.md`](ROADMAP.zh-CN.md) | 中文学习路线（本文件） |
| [`docs/distro-recommendation.md`](docs/distro-recommendation.md) | 为什么选 Ubuntu 24.04 LTS + 备选发行版对比 |
| [`docs/environment-setup.md`](docs/environment-setup.md) | WSL2 / 虚拟机 / 双系统 / 云服务器 环境搭建指南 |
| [`docs/resources.md`](docs/resources.md) | 精选书单、文档、交互式练习与社区 |
| [`docs/roadmap-visualization.html`](docs/roadmap-visualization.html) | 全计划可视化时间轴（浏览器打开即可） |
| [`journal/`](journal/) | 每周学习日志模板与条目 |

---

## 计划一览

| 阶段 | 周次 | 重点 | AI / 数据关联 |
|---|---|---|---|
| 0 | 第 0 天 | 环境搭建 | — |
| 1 | 第 1–2 周 | Linux 基础 | ML 数据集的文件与权限管理 |
| 2 | 第 3–4 周 | Shell 与自动化 | 自动化数据管线、定时拉取、SSH |
| 3 | 第 5–6 周 | Linux 上的 Python 数据环境 | venv/uv、Jupyter、pandas、Docker 入门 |
| 4 | 第 7–9 周 | 本地 AI/LLM 技术栈 | CUDA/GPU、Ollama、Hugging Face、RAG 项目 |
| 5 | 第 10–12 周 | 部署与运维 | FastAPI 提供 AI 服务、云服务器、CI/CD |
| ∞ | 持续 | 英文阅读、日志、写作 | dev.to / 知乎 文章 |

---

## 如何使用

1. **读 [`ROADMAP.md`](ROADMAP.md)**，确定自己的起点（多数新手从第 0 阶段开始）。
2. 按 [`docs/environment-setup.md`](docs/environment-setup.md) **搭建环境**。
3. **完成动手项目**——项目才是真正的课程，阅读清单只是辅助。
4. 用 [`journal/template.md`](journal/template.md) **每周记录日志**。鼓励用英文写，写错没关系。
5. **分享与贡献**：Fork、提 Issue、提交 PR，或点亮 Star。

---

## 学习原则

- **通过工作流学概念**——在给 ML 数据集设权限时学 `chmod`，在让 LLM 服务常驻时学 systemd。
- **稳定压倒一切**——在没有充分理由之前，坚持 LTS 发行版和默认工具链。
- **读一遍，做两遍**——每读一小时，就在终端里练两小时。
- **教会别人是最好的学习**——每个阶段结束，产出一篇可发博客的短文。

---

## License

MIT — 见 [LICENSE](LICENSE)。公开学习，自由分享。
