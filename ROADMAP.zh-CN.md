# Linux × AI — 12 周学习路线（中文版）

完整学习计划。从你当前的水平开始，按自己的节奏推进，每周坚持写日志。

---

## 计划一览

| 阶段 | 周次 | 重点 | AI / 数据关联 | 动手项目 |
|---|---|---|---|---|
| 0 | 第 0 天 | 环境搭建 | — | 一个可用的 Linux shell |
| 1 | 第 1–2 周 | Linux 基础 | 数据集文件与权限管理 | `~/lab` 工作区 + 第一个脚本 |
| 2 | 第 3–4 周 | Shell 与自动化 | 自动化数据管线 | 数据集处理脚本 |
| 3 | 第 5–6 周 | Python 数据环境 | venv/uv、Jupyter、Linux 上的 pandas | 本地数据分析项目 |
| 4 | 第 7–9 周 | 本地 AI/LLM 技术栈 | GPU/CUDA、Ollama、Hugging Face | 本地 LLM + RAG 演示 |
| 5 | 第 10–12 周 | 部署与运维 | 服务化 AI、云服务器、CI/CD | 公开部署 RAG 服务 |
| ∞ | 持续 | 习惯：阅读、日志、写作 | dev.to / 知乎 文章 | — |

---

## 使用方法

- **读一小时，练两小时。** 动手永远比看书有用。
- **项目就是课程。** 时间有限时先做项目，缺什么再补什么。
- **每周写日志**（`journal/template.md`）。尽量用英文写——这也是训练的一部分。
- **不要跳过第 0 阶段。** 一个坏掉的环境，之后补的时间远超现在花的一两个小时。

---

## 每周节奏（推荐）

| 日期 | 活动 |
|---|---|
| 周一 / 三 / 五 | 1–2 小时专注学习 + 终端练习 |
| 周六 | 项目时间（本周动手任务） |
| 周日 | 日志 + 规划下周 + （可选）文章提纲 |

---

## 第 0 阶段 — 环境搭建（第 0 天）

**目标：** 有一个可以从日常机器访问的可用 Ubuntu 24.04 shell，并配置好 Git。

步骤：

1. 按 [`docs/environment-setup.md`](docs/environment-setup.md) 搭好一个环境：
   - Windows 上推荐 **WSL2**（适合日常 AI 开发），或 **虚拟机**、**云服务器**——根据你的硬件任选其一。
2. 完成后置检查清单：`sudo apt update && sudo apt upgrade`、Git 身份、SSH 密钥、`htop`、`tmux`。
3. 真正学会前三个命令：`pwd`、`ls`、`cd`——并阅读它们的 `man` 手册（英文）。

**英语任务：** 读 `man ls`，记下 5 个你之前不知道的选项。

✅ **验收：** 能打开终端、显示当前目录、用 `ls -lh` 列出文件、创建和删除测试目录。

---

## 第 1 阶段 — Linux 基础（第 1–2 周）

**目标：** 掌握文件系统、文件操作、权限、用户和包管理——每位 AI 工程师的日常工具箱。

**为什么对 AI 重要：** 模型权重、数据集、日志都是文件。知道它们放在哪里（`/data`、`~/models`、`/var/log`）、如何保护（`chmod`/`chown`）、怎么装 GPU/驱动包（`apt`），是"5 分钟解决"和"折腾一下午"的区别。

### 第 1 周 — 文件、路径与导航

- **学习**
  - 文件系统层级：`/`、`/home`、`/etc`、`/var`、`/tmp`、`/usr`、`/opt`、`/mnt`、`/data`
  - 导航与查看：`pwd`、`ls -la`、`cd`、`find`、`locate`、`du`、`df`、`file`
  - 读取与编辑：`cat`、`less`、`head`、`tail`、`grep`、`nano` / `vim` 基础
  - 绝对路径 vs 相对路径、`~`、`.`、`..`、通配符 `*` `?` `[]`
- **练习**
  - 探索 `/etc`——挑 3 个配置文件用 `less` 阅读
  - 创建 `~/lab/{data,scripts,models,logs}`——这是你以后常驻的工作区
  - `find ~/lab -type f -name "*.txt"` 练习；用 `wc -l` 数行数
- **项目（上）：** 把一份真实数据集放进 `~/lab/data`（比如 Kaggle 的 CSV，或通过 `huggingface-cli` 下载 Hugging Face 数据集），用 `head`/`wc`/`grep`/`cut` 检查，手写记录它的形状。
- **英语任务：** 读 `man find`；在日志里用英文写 3 句话说明 `-name` 和 `-iname` 的区别。

### 第 2 周 — 权限、用户、包管理

- **学习**
  - 权限模型：`r w x`、属主/属组/其他、`chmod`（八进制与符号）、`chown`、`umask`
  - 用户与组：`whoami`、`id`、`sudo`、`/etc/passwd` 基础、`su`
  - 包管理：`apt update/upgrade/search/install/remove`、`dpkg -l`、`snap`
  - 编辑器：`vim` 求生模式（打开、插入、保存、退出）或 `nano`
  - 帮助系统：`man`、`info`、`tldr`、`--help`
- **练习**
  - 锁定 `~/lab`，只允许你的用户写入：`chmod -R u=rwX,go=rX ~/lab`——并理解为什么
  - 用 `apt` 装一个工具（如 `htop`）、搜索、卸载、重装
  - 跑一遍 `vimtutor`，达到"能活下来"的水平
- **项目（下）：** 给第 1 周的数据集设置正确权限（他人只读），创建属于你的 `scripts/` 目录，在 `~/lab` 里写一行 README。
- **英语任务：** 在日志里用 2–3 句英文解释 `chmod 750`。

✅ **验收（第 1 阶段结束）：** 能完整解释 `ls -l` 输出的每一部分、有目的地设置文件权限、安装/卸载软件包、能查任何命令的英文手册。

---

## 第 2 阶段 — Shell 与自动化（第 3–4 周）

**目标：** 写出能自动化数据工作的 shell 命令与脚本，理解进程与服务，熟练使用 SSH 和 Git 命令行。

**为什么对 AI 重要：** 微调、数据拉取、模型服务都是长时运行或定时任务。管道、`cron`、`systemd` 就是真实 ML 系统保持存续和数据新鲜的方式。

### 第 3 周 — 管道、进程与 bash 脚本

- **学习**
  - 管道与重定向：`|`、`>`、`>>`、`<`、`2>`、`tee`
  - 文本工具：`grep`、`sed`、`awk`、`sort`、`uniq`、`cut`、`xargs`（各一个实用例子）
  - 进程：`ps aux`、`top`/`htop`、`kill`、`killall`、后台任务 `&`、`nohup`、`jobs`、`fg`/`bg`
  - Bash 脚本：shebang、变量、`$@`/`$?`、`if`/`for`、函数、退出码
- **练习**
  - 写一条命令：按大小找出 `~/lab` 里最大的 10 个文件
  - 写第一个脚本 `~/lab/scripts/backup.sh`：把 `data/` 带时间戳复制到 `backup/`，并 `chmod +x`
  - 用 `nohup` 启动一个长任务，用 `htop` 观察
- **项目：** 写 `process_dataset.sh`：读 CSV、数行数、去表头、输出清洗版本——在真实文件上用 `sed`/`awk`/`sort` 管道跑通。
- **英语任务：** 读 Bash 手册的 "Redirections" 章节；记下 3 个你开始使用的重定向技巧。

### 第 4 周 — systemd、定时任务、SSH、Git

- **学习**
  - 服务：`systemctl status/start/enable`、`journalctl -u`、写一个简单的 `.service` 单元
  - 定时：`cron`（`crontab -e`）vs systemd timer（现代系统为什么更推荐 timer）
  - SSH：生成密钥（`ssh-keygen`）、`ssh-copy-id`、`~/.ssh/config`、`scp`/`rsync`
  - 命令行 Git：`status`、`add`、`commit`、`log --oneline`、`branch`、`remote`、`push`/`pull`
- **练习**
  - 创建一个 systemd 服务每天运行备份脚本（或用 timer），用 `journalctl` 验证
  - 配置 SSH 密钥并连接第二台机器（云服务器或其他主机）
  - 把 `~/lab` 初始化为 Git 仓库，做 3 次有意义的提交
- **项目：** 让 `process_dataset.sh` 每周一早上自动运行；把 `~/lab` 推到私有 GitHub 仓库作备份。
- **英语任务：** 在日志里用英文写 5 行提交历史总结，说明每次提交做了什么。

✅ **验收（第 2 阶段结束）：** 能不查 Google 拼出 shell 管道、解释 systemd 单元的工作原理、运行定时任务、从终端把仓库推到 GitHub。

---

## 第 3 阶段 — Linux 上的 Python 数据环境（第 5–6 周）

**目标：** 在 Linux 上搭建干净、可复现的 Python 数据栈：环境管理、Jupyter、日常 pandas 工作流。

**为什么对 AI 重要：** "在我笔记本上能跑"是 ML 头号失败模式。环境隔离（`venv`/`uv`）和知道 Python 在 Linux 上的位置，能避免大部分依赖地狱。

### 第 5 周 — Python 环境与数据栈

- **学习**
  - Linux 上的 Python：`which python3`、系统环境 vs venv、为什么永远不要全局 `pip install`
  - `venv`、`uv`（现代、快）或 `conda`/`micromamba`——选一个并坚持用
  - 环境变量：`PATH`、`export`、`.env` 文件、`env` 命令
  - Linux 上的 Jupyter：装进 venv、无头运行、从浏览器/VS Code 连接
  - Python 中的文件与路径：`pathlib`、`os`、高效读写 CSV
- **练习**
  - 用 `uv` 创建 `~/lab/.venv`，安装 `numpy pandas jupyter`
  - 无 GUI 运行 Jupyter → 从宿主机器连接
  - 写一个 Python 脚本用 `pathlib` 遍历 `~/lab/data` 并打印文件大小
- **项目：** 一个迷你数据分析脚本：加载第 1 周的 CSV、清洗（dropna/重命名）、计算汇总统计、导出小报告——全部在 Linux 终端完成。
- **英语任务：** 读 `uv` 的 README（英文）；写 3 条笔记说明 `uv` 和 `pip` 的区别。

### 第 6 周 — 数据工作流与 Docker（初接触）

- **学习**
  - 磁盘上的数据卫生：命名规范、`parquet` vs `csv`、数据集版本化基础
  - 大文件的 `git-lfs`
  - Docker 概念只学够用：镜像 vs 容器、`docker run`、`docker ps`、卷、端口——先不要深入
  - 为什么容器是 AI 应用的标准交付形态
- **练习**
  - 跑一个一次性容器：`docker run --rm -it ubuntu:24.04 bash`——四处看看，退出
  - 用 pandas 把一个数据集转成 parquet，对比文件大小
  - 把 venv 的 `requirements.txt`（或 `uv.lock`）提交进仓库
- **项目：** 把你的数据分析脚本写进一个微型 Dockerfile，并在容器里运行。
- **英语任务：** 在日志里写一段英文："Why I will use Docker for AI projects"（用自己的话）。

✅ **验收（第 3 阶段结束）：** 能从零在 Linux 上创建可复现的 Python 环境、运行 Jupyter、处理真实数据集、用简单的英文解释容器为什么重要。

---

## 第 4 阶段 — 本地 AI/LLM 技术栈（第 7–9 周）

**目标：** 在 Linux 上真正运行和服务 AI 模型：GPU 加速（或合理的 CPU 方案）、本地 LLM、一个小的检索增强生成（RAG）系统。

**为什么对 AI 重要：** 从这里开始，Linux 不再是"要学的系统"，而是你的 AI 工作站。GPU 驱动、模型服务、向量数据库都是 Linux 优先的。

### 第 7 周 — GPU 加速（或 CPU 兜底）

- **学习**
  - 检查硬件：`lspci | grep -i nvidia`、`nvidia-smi`（AMD 用 `rocminfo`）
  - Ubuntu 上 NVIDIA 驱动 + CUDA 工具链基础：官方仓库、`nvidia-smi`、`nvtop`
  - 没有 GPU（或 WSL 无 GPU 透传）：明确 CPU 方案——哪些在 CPU 上跑得好（小模型、embedding、pandas）
  - PyTorch GPU 检查：`python -c "import torch; print(torch.cuda.is_available())"`
- **练习**
  - 让 `nvidia-smi` 显示你的 GPU；如果没有，诚实记录你的 CPU-only 方案
  - 在 venv 里装 PyTorch（CUDA 或 CPU wheel），用上面一行命令验证
- **英语任务：** 读 NVIDIA 的 "CUDA Installation Guide for Linux" 导言（或 PyTorch 安装页），写 3 行总结。

### 第 8 周 — 运行本地 LLM

- **学习**
  - Ollama：`ollama pull`、`ollama run`、模型大小与内存的权衡
  - 量化是什么（`q4`、`q8`、GGUF），为什么本地模型便宜
  - 服务基础：Ollama 的 REST API、用 `curl` 调一次补全
  - 备选菜单：`llama.cpp`/`llama-server`、`vLLM`（需要吞吐量时）
- **练习**
  - 拉一个小模型（如 `qwen2.5:7b` 或 `llama3.2:3b`），在终端里聊天
  - 用 `curl` 从脚本调用同一个模型
- **项目（上）：** 一个 Python 脚本：向本地模型提问并打印答案。

### 第 9 周 — Hugging Face 生态 + 真实 RAG 项目

- **学习**
  - Linux 上的 Hugging Face `transformers`/`datasets`：下载模型、跑推理
  - Embedding：sentence-transformers（或 Ollama embeddings）
  - 向量库基础：FAISS 或 Chroma——索引是什么、为什么做相似度检索
  - RAG 高层理解：检索 → 增强 → 生成
- **项目（收官项目）：** 搭建 **`~/lab/rag-demo`**：
  1. 把 5–10 份文档（或一个 wiki 子集）灌进向量索引
  2. 提问，得到带出处的答案
  3. 用 Ollama + embeddings 在本地跑通
- **英语任务：** 给 `rag-demo` 写一份英文 `README.md` 解释架构——这就是你第一篇博客的草稿。

✅ **验收（第 4 阶段结束）：** 能运行本地 LLM、通过 HTTP 提供推理服务、用 RAG 管线回答自己文档的问题。

---

## 第 5 阶段 — 部署与运维（第 10–12 周）

**目标：** 交付一个持续在线的 AI 服务：FastAPI、systemd、云 Linux 服务器、安全基础、CI/CD。

**为什么对 AI 重要：** 本地演示人人都会；能在重启、用户、烂代码中活下来的服务才是工作。这个阶段让你从"会跑模型"变成"能跑模型产品"。

### 第 10 周 — 用 FastAPI + systemd 提供 AI 服务

- **学习**
  - FastAPI 基础：路由、请求/响应模型、`uvicorn`
  - 把第 9 周的 RAG 暴露成一个小 HTTP API
  - 作为 systemd 服务运行：工作目录、环境、崩溃自动重启
  - 用 `journalctl -u my-rag -f` 看日志
- **练习**
  - 从另一台机器 `curl` 你的 API
  - 杀掉服务，观察 systemd 自动重启
- **英语任务：** 写 systemd 单元文件，用英文注释每一行。

### 第 11 周 — 云 Linux 服务器与加固

- **学习**
  - 租一台最便宜的云 Linux 机器（或复用家用服务器）：Ubuntu 24.04 LTS
  - SSH 加固：仅密钥登录、禁用 root 登录、`fail2ban`
  - 防火墙：`ufw` 只放行 ssh 和应用端口
  - 反向代理基础：Nginx → uvicorn（为什么需要：TLS、负载均衡）
- **练习**
  - 把 RAG API 部署到云服务器，放在 Nginx 后面
  - 测试重启、查看 `ufw status`、读 `fail2ban` 日志
- **英语任务：** 用英文写一份 10 行的 `DEPLOY.md` 描述部署步骤。

### 第 12 周 — 监控、备份、CI/CD

- **学习**
  - 监控：`htop`、`glances`、`journalctl`、`df -h`
  - 备份：`rsync` + cron/timer、重要仓库的异地副本
  - CI/CD 初接触：GitHub Actions——每次推送跑一个 lint/test 任务
  - （可选）自动部署：合并到 `main` 后自动重新部署
- **练习**
  - 给 `rag-demo` 加一个跑 Python lint + test 的 GitHub Action
  - 定时 `rsync` 备份 `~/lab`（或把日志推到本仓库）
- **收官项目：** 你的 RAG 服务在公开 URL 上在线运行，有监控、有 CI 管线。写"经验教训"文章。
- **英语任务：** 把 RAG 的 `README` + 心得发到 **dev.to**（英文），中文版发到 **知乎**。

✅ **验收（第 5 阶段结束）：** 能把 AI 服务部署到真实 Linux 服务器、保持运行与安全、做好备份、用英文描述整个技术栈。

---

## 长期习惯（第 12 周之后）

- **每日：** 保留一个使用 Linux 的终端工作流（哪怕只是 Git + Jupyter）。
- **每周：** 写日志；读一篇英文 man 手册或官方文档；试一个新命令。
- **每月：** 发一篇文章（英文 → dev.to，中文 → 知乎），素材来自日志。
- **每季度：** 重看这份路线图，选一个深入方向（内核、网络、k8s、MLOps）。

---

## 能力自评清单

把这份清单复制到日志里，边学边打勾。

### 第 1 阶段
- [ ] 我能完整解读 `ls -l` 输出
- [ ] 我能有目的地 `chmod`/`chown`
- [ ] 我能用 `apt` 安装和卸载软件包
- [ ] 我能坚持读完英文 `man` 手册

### 第 2 阶段
- [ ] 我能用 `grep`/`sed`/`awk`/`sort` 串起管道
- [ ] 我能写带参数运行 bash 脚本
- [ ] 我能创建 systemd 服务并查看日志
- [ ] 我能用 cron 或 timer 定时任务
- [ ] 我能用命令行 Git 推拉仓库

### 第 3 阶段
- [ ] 我能凭记忆创建干净 venv/uv 环境
- [ ] 我能无头运行 Jupyter
- [ ] 我能用 pandas 加载/清洗/分析真实数据集
- [ ] 我能跑一次性 Docker 容器

### 第 4 阶段
- [ ] 我能验证 GPU/CUDA 状态（或说明用 CPU 的理由）
- [ ] 我能通过 Ollama 和 API 运行本地 LLM
- [ ] 我建成了基于自己文档的可用 RAG 演示

### 第 5 阶段
- [ ] 我能用 FastAPI + systemd 提供服务化模型
- [ ] 我加固了 SSH 服务并配置了 `ufw`
- [ ] 我部署到了真实 Linux 机器 + Nginx
- [ ] 我有 CI 任务和备份习惯

---

## 参与贡献

发现失效链接？更好的练习？优质资源？开 Issue 或提交 PR——这份路线图靠社区共同改进。
