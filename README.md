# Hsieh-Ting Lin (林協霆) — Knowledge Compendium

> Curated from his Facebook posts, GitHub (118 repos), blog at [htl.physician.tw](http://htl.physician.tw), and book [*The Terminal Way*](https://the-terminal-way.netlify.app/).
> Dr. Lin is an oncology/hematology fellow at Koo Foundation Sun Yat-Sen Cancer Center, Taipei, and a prolific physician-coder.
> **Core philosophy:** "工具塑造思維，思維決定效率" — Tools shape thinking, thinking determines efficiency. "我們要的不是那台電鑽，而是牆上那一個洞" — We don't want the drill; we want the hole in the wall.

---

## Table of Contents

1. [Terminal Mastery & Development Environment](#1-terminal-mastery--development-environment)
2. [Git & Version Control](#2-git--version-control)
3. [Modern CLI Toolchain](#3-modern-cli-toolchain)
4. [AI/Agentic Workflow & MCP](#4-aiagentic-workflow--mcp)
5. [Statistics & Research Methods](#5-statistics--research-methods)
6. [Causal Inference for Clinicians](#6-causal-inference-for-clinicians)
7. [Deploying Agent Outputs](#7-deploying-agent-outputs)
8. [Anti-Bot & Web Scraping Patterns](#8-anti-bot--web-scraping-patterns)
9. [Medical AI Applications](#9-medical-ai-applications)
10. [AI-Era Medical Education](#10-ai-era-medical-education)
11. [Personal Knowledge Management](#11-personal-knowledge-management)
12. [Automation & DevOps](#12-automation--devops)
13. [Prompt Engineering & AI Pedagogy](#13-prompt-engineering--ai-pedagogy)
14. [Programming Languages & Stack](#14-programming-languages--stack)
15. [His Philosophy & Mental Models](#15-his-philosophy--mental-models)
16. [Vibe Learning & Programming Pedagogy](#16-vibe-learning--programming-pedagogy)
17. [Git Safety in the Age of AI Agents](#17-git-safety-in-the-age-of-ai-agents)
18. [LLM Diagnosis & Clinical Reasoning](#18-llm-diagnosis--clinical-reasoning)
19. [The 你快勒馬 Principle (Anti-Overengineering)](#19-the-你快勒馬-principle-anti-overengineering)
20. [Advanced Anti-Scraping Countermeasures](#20-advanced-anti-scraping-countermeasures)
21. [Data Analysis Pipelines](#21-data-analysis-pipelines-beyond-meta-analysis)
22. [The Deeper Philosophy](#22-the-deeper-philosophy)
15. [His Philosophy & Mental Models](#15-his-philosophy--mental-models)

---

## 1. Terminal Mastery & Development Environment

### The Stack (from his dotfiles, 677 commits)

| Tool | Purpose | Key Config |
|------|---------|------------|
| **Zsh + Oh-My-Zsh** | Shell | Powerlevel10k theme, vi mode, autosuggestions, syntax highlighting, fzf-tab completion |
| **Neovim** | Editor | NvChad-based, Python integration via pyenv neovim3 venv |
| **Tmux** | Terminal multiplexer | Persistent sessions, pane management |
| **WezTerm** | Terminal emulator | GPU-accelerated, cross-platform |
| **Hammerspoon** | macOS automation | Window management, custom hotkeys |
| **fzf** | Fuzzy finder | File search, command history, tab completion |
| **ripgrep** | Code search | Faster grep alternative |
| **fd** | File find | Faster find alternative |
| **lsd** | File listing | Modern ls with icons and colors |
| **lazygit** | Git TUI | Terminal-based git interface |

### Zsh Plugin Stack (7 plugins, lazy-loaded)

```
zsh-autosuggestions   → fish-style autosuggestions
zsh-syntax-highlighting → real-time syntax coloring
zsh-vi-mode           → vi keybindings in shell
zsh-lazyload          → deferred loading for startup speed
zsh-you-should-use    → reminds you of existing aliases
fzf-tab               → fuzzy tab completion via fzf
```

### Bootstrap System (symlink-based)

1. Clone to `~/.dotfiles`
2. `brew bundle --file="~/.dotfiles/Brewfile"` installs all packages
3. `~/.dotfiles/start/link_dotfiles` symlinks everything to `~`
4. `sh ~/.dotfiles/macos.sh` applies macOS defaults

### Key Terminal Skills (from *The Terminal Way* book, 22 chapters)

- **Shell mastery**: piping, redirection, job control, aliases, functions
- **Tmux**: session management, pane splitting, copy mode, scripting
- **Neovim**: modal editing, LSP, treesitter, custom snippets, Python integration
- **Text processing trio**: sed, awk, grep (and their modern replacements)
- **Docker**: containerized development environments
- **SSH**: remote development, tunnels, agent forwarding
- **Debugging**: strace/dtrace, profiling, log analysis

---

## 2. Git & Version Control

### Daily Git Workflow (from *The Terminal Way* Ch.9)

- Commit early, commit often
- Meaningful commit messages in conventional format
- Branch-based workflow: feature branches → PR → merge
- Interactive rebase for clean history

### Advanced Git Practices (from his repos)

- **Git hooks**: `.githooks/` directory for reproducibility enforcement in meta-pipe
- **Secret scanning**: `.gitleaks.toml` and `.gitguardian.yaml` in dotfiles
- **Git worktrees**: Used in claude-telegram-bot for isolated risky changes (`/worktree` command)
- **BFG**: `bfg-1.14.0.jar` included in dotfiles for git history cleaning
- **Semantic versioning**: Tagged releases (`v*.*.*`) with CI-triggered builds

### Automated Release Pipeline Pattern

From his Anki addon workflow:
```
GitHub Release → GitHub Action triggered → Auto-update AnkiWeb version
```
He reverse-engineered AnkiWeb's internal API since they don't offer npm/pip-style GitHub integration, then wired it to CI/CD. This pattern — **reverse-engineer internal API → wrap as CLI → wire to CI/CD** — is one he repeats across projects.

### Git Tools

- **lazygit**: Terminal UI for staging, committing, branching, rebasing
- **GitHub CLI (gh)**: PR management, issue tracking, repo operations
- **claude-with-webhook**: Self-hosted Go webhook server that triggers Claude Code on git events

---

## 3. Modern CLI Toolchain

### Core Replacements for Classics

| Classic | Modern Replacement | Why |
|---------|-------------------|-----|
| `grep` | `ripgrep` (rg) | Faster, respects .gitignore, better defaults |
| `find` | `fd` | Faster, smarter defaults, regex-friendly |
| `ls` | `lsd` | Icons, colors, tree view |
| `cat` | `bat` | Syntax highlighting, line numbers, paging |
| `cd` | `zoxide` | Smart directory jumping based on frequency |

### fzf — The Universal Fuzzy Finder

**Shell integration:**
- `Ctrl-R` → fuzzy command history search
- `Ctrl-T` → fuzzy file search
- `fzf-tab` → replaces tab completion with interactive fuzzy menu

**In scripts:**
```bash
vim $(fzf)
git checkout $(git branch | fzf)
kill $(ps aux | fzf | awk '{print $2}')
```

### Package & Version Management

- **Homebrew**: macOS package management via Brewfile (+ Brewfont for fonts)
- **pyenv**: Python version management (multiple versions coexisting)
- **uv**: Fast Python package manager (used in meta-pipe, end-to-end)
- **pnpm**: Node.js package manager (used in mini-claw, lizard-linebot)
- **Bun**: Fast JS runtime (used in claude-telegram-bot)
- **renv**: R package reproducibility (used in meta-pipe)

---

## 4. AI/Agentic Workflow & MCP

### The Core Loop: Propose → Get Critiqued → Defend or Retreat

Dr. Lin describes his agentic workflow as a dialogue, not a command:

> "我常常是對著 Agent 提案，然後被它批鬥一輪：「我覺得這樣不行，你為什麼要上那麼多層抽象？你為什麼不這樣那樣…」，來回交手個幾輪，我很堅持要的功能要說服它。這樣跟 Agent 的互動會讓我把要解決的問題想細一點，有時候甚至會戰略性撤退，趕快轉進把認知資源投注在真正值得解決的問題上。"

**Translation**: He proposes solutions to his agent, the agent criticizes them ("too many abstraction layers," "why not do it this way..."), they go back and forth, he defends what he really needs. This makes him think more carefully about each problem. Sometimes he strategically retreats — realizing the real cognitive resources should go somewhere else.

### The Agentic Workflow

1. **Define the spec** — write a clear, detailed prompt describing the task
2. **Let agents execute** — Claude Code runs multi-step pipelines autonomously
3. **Agent critiques your proposal** — not just executing; the agent pushes back on design
4. **Defend or retreat** — human makes judgment calls on what's worth building
5. **Review outputs** — human reviews at quality gates
6. **Iterate** — refine prompts based on results, archive proven patterns

### Nightly Research Ritual

He described his bedtime routine:
> "最近睡前的儀式都是請 Claude 寫一篇 paper，當作隔天早上的晨間讀物。"

Full prompt template:
> "我想做個跟 ___ 有關的題目，請幫我看一下最近發表過的類似主題，以 High IF 期刊為偏好及審美。開始幫我想這個題目，設計方法、研究，再依據結果，以它的 novelty、robustness 的程度來看適合哪間期刊、去參考他們的寫作規範，latex + csl + bib ，寫完初稿後，需要請 4 位 subagents 提出 revision 意見，然後我們修改，這樣要至少四輪以上，直到所有 reviewer 都 "ACCEPT"，記得加上 AI usage disclaimer 。最終要交付的東西是一個 github private repo 、所有的 preprint 的素材都要提交要在 release 中"

**This is the end-to-end agentic research pipeline** — produce a full paper while you sleep, with multi-round peer review by subagents. Published in Nature on the concept.

### MCP (Model Context Protocol) — His Implementations

#### The Chrome Extension MCP Pattern (Hard-Won Lesson)

This is one of his most important architectural patterns. When OpenEvidence added DataDome anti-bot protection, all browser-simulation and API-reverse-engineering approaches broke:

> "自從上星期 OE 加入 DataDome 後，用 POST 方法都會撞到 403，內心大概有底所有模擬 browser 、反向解析 API 的方法的鼠貓遊戲都不持久。"

**The insight**: Chrome extensions have enormous privileges — they can read cookies, open tabs, refresh pages. If you build a Chrome extension that runs a local server and keeps an MCP connection alive, when the agent needs to query a protected site, it tells the extension to silently open a background tab. That tab already has all the user's cookies and browser fingerprint, so it passes every anti-bot check.

**Fire-and-forget + polling pattern**:
1. Agent asks question via MCP → Extension opens background tab with user's real browser session
2. Returns an `id` immediately
3. Agent polls for results a few seconds later
4. All cookies, browser fingerprints, and session data are real — indistinguishable from human use

**Generalizable pattern**: This same architecture works against any anti-bot-protected site. "之後對付反爬蟲的網站都可以採取這套策略 (將目光望向衛服部網站)."

#### HAR File Pattern (Alternative)

Before the Chrome extension approach, he tried the `har` file method:
1. Export HAR from browser dev tools
2. Extract cookies + browser fingerprint from the HAR
3. Use both to construct requests that pass DataDome's behavioral ML detection

> "Claude Code 死活不肯動手，各種繞還是被它看穿了我的小心思...那就換 Codex 來用，但 Codex 分析了一輪後，給我一個更務實的建議"

**Lesson**: Different AI models have different ethical boundaries. If one refuses, another may suggest a more practical approach. But also: the model that refuses might be protecting you from a fragile solution.

#### openevidence-mcp
- Wraps OpenEvidence (medical literature AI) as an MCP server
- Evolved through multiple architectures: direct API → HAR-based → Chrome Extension relay
- Integrates PubMed and CrossRef for citation validation
- TypeScript, browser extension relay
- Enables Claude Code to query medical evidence directly

#### openevidence-skill
- Python-stdlib-only portable version of the MCP server
- Uses "progressive loading" to avoid bloating the system prompt
- 1:1 tool surface mirror of openevidence-mcp

#### ask_user_mcp (in claude-telegram-bot)
- Enables inline Telegram action buttons during Claude interactions
- Claude can prompt the user for decisions mid-workflow

#### Other MCP / Claude Skills (15+)
- **audit-oe-skill**: Parallel PubMed verification of OpenEvidence citations
- **research-guardian-skill**: Multi-gate automated research quality verification
- **drug-drug-skill**: Evidence-based drug-drug interaction assessment (modeled after Micromedex)
- **ebmt-handbook-skill**: Clinical guideline skill from EBMT Handbook 8th Edition
- **toefl-skill**: TOEFL iBT preparation coach
- **zh-article-analyzer-skill**: Traditional Chinese article deep analysis
- **zh-ebn-report-skill**: Taiwan nursing evidence-based report (N1-N4 promotion) coach
- **critique-defense-copilot-skill**: Calm-response coach for online attacks/defamation
- **gh-repo-father-skill**: GitHub repo bootstrapper — raw idea to scaffolded repo
- **line-inbox skill**: Queries Turso DB from Claude Code for natural-language inbox search

### Multi-Agent Architecture (from end-to-end project)

**Three-layer research system:**
```
Layer 1: Autonomous pipeline → produces draft manuscript
Layer 2: Audit subagent → examines Layer 1 outputs, generates findings
Layer 3: External validation → preregistered validation
```

- **4 distinct reviewer persona agents** with separate prompt files
- Multiple review rounds with verbatim transcripts captured
- "Single-operator" model: one human orchestrates multiple AI agents
- **Disclosure standard**: prompt + commit hash + tagged release (not traditional methods-section narrative)
- Submitted to *The Lancet Digital Health* as a Viewpoint and also published in *Nature*

### The LLM Reference Hallucination Debate

From a Facebook thread with significant engagement:
> "2026 年如果一個人還會說 LLM / Agents 會引用虛假文獻的，那他對 LLM / Agents 的理解恐怕還停留在 2023 年"

**The "harness" concept**: LLMs without guardrails hallucinate references. With proper harness (systematic review methodology, PubMed verification, audit agents), they don't. The people getting caught with hallucinated references are those who didn't build harnesses.

**Counterpoint from a colleague**: "If you need expert-level harness to be reliable, then from a product perspective it's unreliable for the general population — like a knife that requires surgical training."

**His solution**: Systematic review pipeline → AI accelerates the process → every reference verified against PubMed. The meta-pipe and robust-lit-review projects are this harness, productized.

---

## 5. Statistics & Research Methods

### Meta-Analysis Pipeline (meta-pipe, 91 stars)

A **9-stage reproducible pipeline** — AI-assisted but statistically rigorous:

| Stage | What It Does | Output |
|-------|-------------|--------|
| 01 Protocol | Define PICO, eligibility | `pico.yaml`, `eligibility.md` |
| 02 Search | Multi-database search | `dedupe.bib` |
| 03 Screening | Title/abstract review | `decisions.csv` |
| 04 Fulltext | Full-text retrieval | `manifest.csv` |
| 05 Extraction | Data extraction | `extraction.csv` |
| 06 Analysis | Statistical computation | `figures/`, `tables/` |
| 07 Manuscript | Write-up | `manuscript.pdf` (Quarto) |
| 08 Reviews | GRADE assessment | `grade_summary.md` |
| 09 QA | Final verification | `final_qa_report.md` |

**Example project**: ICI for triple-negative breast cancer — 5 RCTs, N=2,402. **14 hours vs 100+ hours manual.** Risk ratio 1.26 (95% CI 1.16–1.37, p=0.0015), GRADE HIGH quality.

### Full-Text Journal Download via EZproxy

Most institutional library access uses EZproxy — the library acts as a proxy server. The pattern:

1. Log into library → get cookies
2. URL transforms: `www.nejm.org` → `www-nejm-org.institution-dns:PORT/doi/full/{DOI}`
3. Parse `<meta name="citation_pdf_url">` from HTML
4. Wrap as CLI for one-command full-text download
5. For OA articles: use Unpaywall API

His implementation uses the institution's specific URL/DOI handling rules and port. Not open-sourced (each library has different internal rules), but the pattern is documented: "相信大家聰明的 Opus 4.7 跟 GPT 5.5 應該有辦法看得懂."

### Statistical Methods Across His Projects

- **Binary outcome meta-analysis**: RR, OR, RD with fixed/random effects models
- **Network meta-analysis**: dedicated `ma-network-meta-analysis` module
- **Publication bias**: Funnel plots, Egger's test, trim-and-fill
- **Forest plots**: standard + cumulative
- **GRADE framework**: Evidence quality assessment (⊕⊕⊕⊕)
- **PRISMA 2020 compliance**: Full reporting standards
- **Survival analysis**: Kaplan-Meier curves, Cox regression, log-rank tests
- **MMRM** (Mixed Models for Repeated Measures): From roche-vabysmo-rwe-workshop
- **CMH** (Cochran-Mantel-Haenszel): Stratified analysis
- **Gene signature validation**: Venet 2011 paradigm — random gene sets as null benchmark
- **Topological data analysis**: scRNA-seq cell-state plasticity (sctda-cancer-plasticity)
- **Graph theory**: Evidence-free decision points in biomarker-driven guidelines (mbc-evidence-dag-paper)
- **NGS tertiary analysis**: BAM → ESMO clinical report using OncoKB/ESCAT classification

### R for Clinical Statistics (from learn-r-with-ai)

**30-task curriculum, 6 parts:**
1. **Quick Start** (Tasks 1-5): R environment, basic operations
2. **Reading Data** (Tasks 6-8): Import, inspect, clean
3. **Table 1** (Tasks 9-14): Descriptive statistics, group comparisons
4. **Publication Figures** (Tasks 15-19): Box plots, multi-panel, forest plots, funnel plots
5. **Statistical Tests** (Tasks 20-24): t-tests, ANOVA, chi-square, log-rank
6. **Integration** (Tasks 25-30): End-to-end analysis, reporting

### Research Tools (zero-dependency where possible)

- **flowdoc**: Zero-dependency TypeScript tool generating PRISMA/CONSORT/STROBE flow diagrams as SVG
- **robust-lit-review**: Automated systematic review pipeline (Scopus, PubMed, Embase, DOI validation)
- **research-publishing-pipeline**: Semi-automated research writing with Claude Code

---

## 6. Causal Inference for Clinicians

> From his teaching post at ASH Asia Trainee Day. Posted because "如果一個問題被問三次就發成文好了" (if a question gets asked three times, write a post).

### The Problem

Observational studies show A correlates with B. Media and public intuition: "A caused B." Observational studies get challenged → someone invokes the "RCT is the gold standard" card → but RCTs are often impractical (cost, ethics, time).

### The Solution: Counterfactual Reasoning (ELI5)

He uses **JJ Lin's song "可惜沒有如果" (If Only)** as a teaching example:

> "倘若那天，把該說的話好好說，該體諒的不執著…"

In the MV, JJ believes his poor communication caused the breakup. To prove this, you'd need a **parallel universe** — same person, same scene, but this time JJ communicated well, then follow both timelines to see if they end up together.

This is **counterfactual reasoning**: what would have happened to the same person under a different intervention?

### Core Concepts (in order of introduction)

| Concept | ELI5 |
|---------|------|
| **Treatment Effect** | The difference in outcome (Y) caused by the intervention (treatment) |
| **ATE** (Average Treatment Effect) | Average the treatment effect across a whole population |
| **Counterfactual** | Parallel universe where everything is the same except the treatment |
| **Propensity Score** | A composite score estimating how likely someone is to receive the treatment given their covariates |
| **Matching** | Find people with similar propensity scores — one got treatment, one didn't — and compare |
| **Weighting** | Instead of matching 1:1, weight observations by their propensity score |
| **TMLE** (Targeted Maximum Likelihood Estimation) | "選他就對了" (just pick this one) — the modern go-to method for getting a balanced cohort that convincingly shows causal effects |

### Key Intuitions

- **Parallel universes don't exist** → instead, find people with similar characteristics (same attractiveness, same singing ability, similar dating history) where one "communicated well" (treatment) and one didn't (control)
- **Propensity score** = one number that summarizes all confounding variables into a probability of receiving treatment
- After matching/weighting on propensity scores, you get **covariate balance** — the two groups look comparable, which makes your causal claim more convincing
- **Unknown unknowns** and **robustness checks** are a whole separate discussion (which he offered to write about)

### Target Trial Emulation

Mentioned as the framework for designing observational studies that mimic RCTs. Instead of "we observed these people," you design the study as if you were running a trial, then find observational data that matches your trial design.

---

## 7. Deploying Agent Outputs

> From his Facebook post on how to publish products from Claude/any agent.

### Hosting Platforms Compared

| Platform | Best For | Key Advantage | Key Limitation |
|----------|----------|---------------|----------------|
| **GitHub Pages** | Beginners | Repo → GitHub Actions → one-click deploy | Repo must be public |
| **Netlify** | Astro, general static sites | Upload folder directly, drag-and-drop HTML | — |
| **Vercel** | React ecosystem | Optimized for Next.js, serverless functions | Less ideal for non-React |
| **Cloudflare Pages** | Everything | Access to entire CF universe (Workers, D1, R2, Zero Trust) | Requires a domain |

### His Recommendation

**"Cloudflare 是賽博菩薩"** (Cloudflare is the Cyber Bodhisattva) — this phrase appears in at least 3 separate posts. The free tier is extremely generous and the ecosystem is deep.

**For what agent-generated output:**
- If it's just HTML/CSS → Netlify's drag-and-drop (no CLI needed)
- If it needs a backend → Cloudflare Workers + D1 + R2
- If it's a React project → Vercel
- If it's open source and simple → GitHub Pages

### The "No Agent Power Required" Path
Netlify specifically: create account → see upload area → drop HTML folder → done. Main file must be `index.html`.

---

## 8. Anti-Bot & Web Scraping Patterns

### The Cat-and-Mouse Problem

DataDome (and similar services) don't just check IPs. They use ML to analyze:
- Behavioral patterns (click speed, scroll behavior, "thinking speed")
- Browser fingerprints (canvas, WebGL, font rendering)
- Session characteristics

Traditional approaches fail because:
- Simulated browsers are detectable at the ML level
- Reverse-engineered APIs break when the target adds bot protection
- IP-based blocking follows you across computers

### The Chrome Extension Architecture (Recommended)

```
Agent (MCP Client) ←→ Local MCP Server ←→ Chrome Extension ←→ Target Website
                                              (real cookies,    (sees real browser)
                                               real fingerprint)
```

**Why it works**: The requests originate from a real Chrome browser with the user's authenticated session. DataDome sees a human, not a bot. The extension opens a background tab, fires the query, returns immediately with an ID, then the agent polls for results.

**Ethical consideration**: He uses this for OpenEvidence (a service he's authenticated to use; he's bypassing bot detection, not paywalls). "OE 本身有 100 queries/hour 的限制，不要碰到就好" — he respects the rate limits.

### The HAR File Approach (Fallback)

1. Open browser dev tools → Network tab
2. Perform the action manually
3. Export as HAR file
4. Extract cookies + browser fingerprint from HAR
5. Use these to construct requests programmatically

**Limitation**: IP changes can invalidate this approach. One computer change broke his previous method: "顯然，我前幾天 Po 的方法一換電腦就失效，大概是 IP 被標記了."

### EZproxy Pattern (Legal Full-Text Access)

For institutional journal access:
1. Log into library → get cookies
2. Transform URL: `www.journal.org` → `www-journal-org.proxy.institution.edu:PORT/doi/full/{DOI}`
3. Extract `<meta name="citation_pdf_url">` from HTML
4. Wrap as CLI: one command → full PDF
5. For open-access: Unpaywall API

---

## 9. Medical AI Applications

### Evidence-Based Medicine AI

- **OpenEvidence MCP**: Query medical literature from within AI coding sessions
- **PubMed/CrossRef validation**: Parallel citation verification with audit agents
- **PICO rewriting**: Groq-powered clinical question formulation (in Anki addon)
- **Evidence grading**: Automated GRADE assessment in meta-pipe
- **breast-cancer-uptodate**: Weekly auto-generated treatment trend reports

### Clinical Genomics

- **NGS tertiary analysis**: R pipeline from BAM files to ESMO 2024 clinical reports
- **AMP/ASCO/CAP classification**: Agentic AI vs. rule-based variant classification benchmarking
- **OncoKB integration**: Clinical actionability annotation

### Medical Education

- **hematok**: TikTok-style endless scroll of 6,973 ASH Image Bank hematology images (React PWA, Cloudflare)
- **hematology-board-review**: 72 topic notes + 216 self-authored ABIM-style questions (Python + Obsidian + spaced repetition + AI coach)
- **hemonc-daily-case**: Automated daily case summaries via Claude Code Routine
- **learn-r-with-ai**: AI-mediated R learning — "your job is to ask the right questions, not write code"
- **mcq-bank**: Collaborative MCQ study system with wiki explanations, threaded discussion, timed mock exams (React, Cloudflare Workers, Hono, Zero Trust)
- **kahoot-cf**: Self-hostable Kahoot clone on Cloudflare Workers + Durable Objects + Zero Trust (1,000 concurrent users — Kahoot charges $49/month for this)
- **CompTIA-security-plus-notes**: 83 topics, 563 concepts, 332 practice questions — digital garden built with Quartz

### Clinical Workflow

- **vghtpe-uro**: Hospital scheduling system with Google Sheets + clasp (Apps Script)
- **lizard-gslide-module**: Automated Google Slides formatting for presentations — batch processing, theme application, watermark toggle (33 stars)
- **society-calendar**: AI auto-scrapes Taiwan medical society calendars → Google Calendar sync

---

## 10. AI-Era Medical Education

> From his viral Facebook post questioning the foundations of medical education.

### The Diagnosis

Medical education today operates on the "共筆" (collaborative note-taking) model — students compile lecture notes, memorize, and reproduce on exams. With AI:
> "現今所有醫學系做共筆都是把老師的講義丟給 NotebookLLM 然後噴一堆重點摘要搭配考古題就可以結案"

The problem isn't AI use — it's that students lack **prior knowledge** to evaluate AI output. Without prior knowledge, there's no **taste** — no ability to distinguish good from bad. Students become "應聲蟲" (echo chambers) for whatever AI outputs.

### Why "Just Add Critical Thinking" Fails

The common response is "teach critical thinking instead of memorization." But:
> "對於醫學生來說，可能就是只在 Prompt 中多加一句『用批判性思考的方式來回答這一題』照樣用 AI 回答，也是一樣照單全收 AI 吐出來的東西"

Adding "use critical thinking" to a prompt doesn't create critical thinking if the student lacks the prior knowledge to evaluate the response.

### His Proposal: Textbook-to-Exam Model

1. **Eliminate lectures** — "反正每年都講差不多，學生如我當年也是翹課" (students skip them anyway; same content every year)
2. **Exam directly from textbooks** — questions based on specific textbook chapters
3. **USMLE-style questions requiring reasoning** — "請依這段內容，以 USMLE 風格，需要推理，才能想出答案的 5 題"
4. **Frequent small exams** — every class session is a quiz
5. **Let students figure out their own AI methods** — "八仙過海，各顯神通" — they'll develop their own ways to digest textbook content with AI

**The goal**: When students are forced to engage deeply with primary sources, and given the freedom to use AI however they want to master that content, **foundational knowledge + critical thinking will emerge naturally** — they don't need to be taught as separate subjects.

### Essential Physician AI Agents (His List)

> "我覺得作為一個醫師個體，有以下幾樣必備的AI Agent工作系統流程會讓生活快樂很多"

1. **Auto-fill teaching evaluation forms** agent
2. **Auto-complete online course requirements** agent (login, watch, track hours)
3. **Inbox triage bot** — collect email, LINE, Facebook messages → auto-categorize → add to calendar and reminders based on habits
4. **Journal surveillance system** — regularly digest top journal content into summaries
5. **Information-to-output pipeline** — organize information for presentations, blog posts, social media
6. **Automated research pipeline** — feasibility assessment, figure/table generation, literature collection, formatting
7. **Word document handler** — process various Word files automatically

---

## 11. Personal Knowledge Management

### LINE as Data Capture

**lizard-the-linebot architecture:**
```
LINE forward → Cloudflare Worker (HMAC verify) → Turso DB (idempotent insert) → Claude Code agent → action
```

- **Capture**: Forward any LINE message to the bot
- **Store**: Turso (libSQL) edge database, with type-specific columns (text, sticker, file, location) + raw_payload catch-all
- **Binary content**: Downloaded from LINE's content API, stored in Cloudflare R2 under `YYYY-MM/<messageId>` (LINE only keeps content ~7 days)
- **Reply gating**: Only responds to explicit @-mentions, not `@all` or 1:1 DMs
- **Cost**: All three services (Cloudflare Workers, Turso, LINE) operate on free tiers at personal volume

**Philosophy**: "LINE is a black box" — useful information arrives constantly, then disappears into poor search and no export. The bot reframes LINE as **capture only**; "the DB is the integration surface."

### Claude Code Session Management

- **session-collection**: Aggregates conversation history — extracts thinking processes, reusable prompt patterns, and final solutions
- Creates a searchable knowledge base from all AI interactions
- "思考過程、可重用的 prompt 模式與最終解法"

### Tools for Knowledge Management

| Tool | Purpose |
|------|---------|
| **Obsidian** | Clinical notes, board review, linked thinking, spaced repetition |
| **Anki** | Spaced repetition (custom addon for OpenEvidence/UpToDate/PICO integration) |
| **LINE bot** | Capture inbox messages, meeting links, research snippets |
| **Newsboat** | RSS reader (configured in dotfiles) |
| **Todo.txt** | Task tracking (configured in dotfiles) |
| **Email tool** | Personalized TypeScript mail processing to reduce inbox noise |

### Anki Ecosystem (his contributions)

- **anki-openevidence-addon**: Send highlighted card text to OpenEvidence/UpToDate/Google, with Groq PICO rewriting
- **ankiweb-add-card**: CLI to create decks/add cards via reverse-engineered AnkiWeb protobuf API — "目前體感上最流暢的做法" (currently the smoothest experience)
  - Stores credentials in `.env`
  - Auto-authenticates via httpx to get cookies
  - Packaged as a Claude Code skill via Makefile
  - Can call from claude.ai web interface directly
- **AnkiWeb CI/CD**: Since AnkiWeb has no official GitHub integration, he reverse-engineered their publishing API and wired GitHub Releases → automatic AnkiWeb version updates

---

## 12. Automation & DevOps

### Cloudflare Ecosystem (his go-to platform)

He calls Cloudflare "賽博菩薩" (Cyber Bodhisattva) repeatedly. The free tier covers nearly everything at personal scale:

- **Workers**: Serverless functions (LINE bot, Kahoot clone, Threads CLI OAuth, image hosting, TLDR extension backend)
- **Durable Objects**: Stateful serverless — real-time multi-player sync (Kahoot clone, 1,000 concurrent users)
- **D1**: SQLite at the edge (image hosting, Kahoot)
- **R2**: Object storage (LINE attachments, image hosting)
- **Zero Trust**: Access control (mcq-bank, ttyd-tmux-cf, Kahoot — only authorized users can access)
- **Pages**: Static hosting (personal site, documentation)
- **Tunnels**: Secure access to local services without opening ports

### Automation Patterns (Daily/Weekly/Monthly)

| Project | Frequency | What It Does |
|---------|-----------|-------------|
| **hemonc-daily-case** | Daily | Generates hematology/oncology case summaries |
| **polish-prompt** | Daily | LLM reviews daily English writing |
| **breast-cancer-uptodate** | Weekly | Breast cancer treatment trend reports via OpenEvidence |
| **society-calendar** | On-demand | Scrapes medical society calendars → Google Calendar |
| **tma-edu-exam** | Monthly | Auto-completes TMA continuing education exams |
| **lizard-gslide-module** | On-demand | Batch formats Google Slides |
| **owa-sync** | On-demand | Outlook Web Access personal sync |

### Self-Hosted Infrastructure

- **ttyd-tmux-cf**: Persistent web terminal accessible from any browser
  - ttyd + tmux running on Mac mini at home
  - Cloudflare Tunnel + Zero Trust authentication
  - Nerd Font uploaded to R2 for proper rendering
  - Access from any computer (even public ones) via `term.your-domain.com` in incognito
  - **Use case**: environments where WiFi blocks SSH but allows web browsing
- **img-hosting**: Self-hosted Imgur alternative (R2 + D1)
- **pdf-presenter**: Lightweight CLI PDF presenter with browser-based presenter mode
- **claude-with-webhook**: GitHub webhook → triggers Claude Code planning (Go, Tailscale)

### macOS Automation

- **Hammerspoon**: Window management, custom keybindings
- **LizardType**: Native macOS push-to-talk dictation (Swift) — hold key, speak, release, text pasted at cursor
- **macos.sh**: System defaults script in dotfiles
- **launchd**: Service management (ttyd-tmux persistent service)

---

## 13. Prompt Engineering & AI Pedagogy

### Prompt as Product

From his `polish-prompt` project:
> Treat prompts as "文本產品" (text products) — version-managed, evaluated, iterated.

**Workflow:**
1. Write prompt as structured text
2. Version control the prompt
3. Evaluate AI output against criteria
4. Iterate prompt based on results
5. Archive proven patterns for reuse

### The AI-Mediated Learning Method (from learn-r-with-ai)

**The 5-Step Loop:**
1. Instructor gives a **task** (in natural language)
2. Learner pastes the task description to AI (ChatGPT/Claude)
3. AI generates the code
4. Learner runs code in Positron/Posit.cloud
5. Group reviews results and interprets together

**Core principle**: "你的工作是「問對問題」，不是「寫對程式」"
(Your job is to ask the right questions, not write the right code.)

**Five learning objectives:**
1. Describe problems clearly enough that AI writes the code
2. Understand roughly what AI-generated code does
3. Know how to ask AI for corrections when errors occur
4. Produce publication-quality tables and figures
5. Have a reusable analysis template

### Disclosure Standard for AI-Assisted Research

From his *Lancet Digital Health* submission:
> Minimum disclosure = prompt + commit hash + tagged release
> (Not traditional methods-section narrative)

Every AI interaction is recorded, versioned, and published with no post-hoc curation. The entire artifact chain is auditable.

### The Claude Code Skill Ecosystem

He treats skills as portable, versioned, installable packages. Pattern:
```bash
npx skills add <skill-name>
```

Skills he maintains across domains:
- Clinical: drug-drug, ebmt-handbook, zh-ebn-report, audit-oe
- Research: research-guardian, robust-lit-review
- Education: toefl, zh-article-analyzer
- Development: gh-repo-father, agent-skills
- Personal: critique-defense-copilot, line-inbox

---

## 14. Programming Languages & Stack

### Language Distribution (across 118 repos)

| Language | Usage | Primary Domain |
|----------|-------|----------------|
| **Python** | ~40% of repos | Research pipelines, automation, skills, CLI tools, reverse engineering |
| **TypeScript** | ~25% of repos | Bots (Telegram, LINE), web apps, MCP servers, Cloudflare Workers |
| **R** | ~15% of repos | Statistics, meta-analysis, bioinformatics, tutorials |
| **Lua** | ~5% | Neovim config, Hammerspoon |
| **Shell** | ~5% | Dotfiles, automation scripts |
| **JavaScript** | ~5% | Google Apps Script (clasp), Chrome extensions |
| **Go** | ~3% | Webhook servers, CLI tools |
| **Swift** | ~2% | macOS native apps (LizardType) |
| **TeX/LaTeX** | Tooling | Manuscripts, books, presentations (Quarto + Typst) |

### Tech Stack Summary

```
Frontend: React, Astro, Vue, Vite, Slidev, Quarto, Typst
Backend:  Cloudflare Workers, Hono, Python/FastAPI, Go
Database: Turso (libSQL), D1, R2, SQLite
AI:       Claude Code, Claude Agent SDK, MCP, Groq, OpenAI
DevOps:   Cloudflare, Docker, GitHub Actions, Netlify, clasp
Mobile:   Telegram Bot (grammY), LINE Bot, PWA
Desktop:  macOS, Neovim, Tmux, Hammerspoon, LizardType
```

### Key Package Ecosystem Choices

| Ecosystem | Package Manager | Runtime |
|-----------|----------------|---------|
| Python | uv (fast, modern) | pyenv for version management |
| Node/TS | pnpm | Node 22+ |
| Node/TS (alt) | Bun | Bun runtime (claude-telegram-bot) |
| R | renv | R 4.2+ |
| macOS | Homebrew (Brewfile) | — |
| Google Apps Script | clasp (CLI) | — |

---

## 15. His Philosophy & Mental Models

### "We Don't Want the Drill; We Want the Hole"

> "記得我們要的不是那台電鑽，而是牆上那一個洞。雖然分享最新電鑽的文比較有流量。"

Don't chase the latest AI tool. Chase the problem you need solved. Sharing the newest drill gets more likes, but the hole is what matters.

**How this manifests**:
- He doesn't use Claude Design, doesn't play with AI image generation, hasn't kept up with NotebookLLM
- He doesn't install every new AI thing (tried Lobster, deleted it in 30 minutes)
- He focuses on building tools that solve specific, real problems in his clinical and research workflow
- Then shares what he builds — not to chase trends, but because the solution might help others

### Agent as Design Critic, Not Just Executor

He doesn't just tell agents what to do — he proposes solutions and lets the agent critique them. The agent pushes back on unnecessary abstraction, suggests simpler approaches, and forces him to think more deeply about what he actually needs. Sometimes the right move is to retreat and redirect cognitive resources.

### The "Harness" Concept

An LLM without harness is dangerous. An LLM with harness is a research accelerator. The harness includes:
- Systematic review methodology
- Reference verification against PubMed/CrossRef
- Audit agents that check primary agent outputs
- Multi-gate quality control (research-guardian-skill)

The harness is what separates "LLMs hallucinate references" from "LLMs accelerate systematic reviews." Same model, different scaffolding.

### Reverse-Engineer Then Productize

Pattern repeated across projects (AnkiWeb, OpenEvidence, LINE, hospital systems):
1. Identify a platform that lacks an API
2. Reverse-engineer the internal API
3. Wrap it as a clean CLI
4. Wire it to CI/CD or package it as a skill
5. Share the pattern even if the specific implementation can't be open-sourced

### Solve Your Own Problems First

Nearly every project solves a problem he personally encounters:
- LINE messages disappear → LINE bot
- Anki cards are annoying to create → AnkiWeb CLI
- Google Slides formatting is repetitive → lizard-gslide-module
- Hospital scheduling is chaos → Google Sheets scheduling system
- Medical education is rote → hematok, board review, MCQ bank
- Evidence retrieval is slow → OpenEvidence MCP
- SSH blocked on hospital WiFi → ttyd-tmux-cf

This is the "dogfooding" approach: build what you need, then share.

### Cloudflare as Universal Infrastructure

He calls Cloudflare "賽博菩薩" (Cyber Bodhisattva) because it provides production-grade infrastructure at zero cost for personal projects. Workers, D1, R2, Durable Objects, Zero Trust, Tunnels — a complete backend stack on the free tier.

### The "Three Times" Rule

> "如果一個問題被問三次就發成文好了"

If people ask the same question three times, turn the answer into a published post. This is how he decides what to write about — community demand, not content planning.

---

## 16. Vibe Learning & Programming Pedagogy

### CS146S — Learning by Doing, Not Watching

> "2025 之後，發現自己真的難以「看著影片學會東西」。有了 Claude Code 這把錘子，看什麼都是釘子，直接動手比較有樂趣。"

His approach to learning anything technical:
1. Find the course syllabus/website (e.g., Stanford CS146S)
2. Give Claude Code the course link + your GitHub account
3. Say: "看看這個課程，搜搜網路上大家的學習筆記 esp. github repos，幫我設計一個 project，循序漸進地學習" (Look at this course, search for people's study notes especially GitHub repos, design a project for me to learn progressively)
4. Build the project with AI guidance, learning by doing

No watching lectures. No reading textbooks linearly. Build something real and learn through construction.

### His "Terrible" Programming Advice for Beginners

He admits his advice is "幾乎是幹話的等級" (almost nonsense-level):
> "找個想解決的問題問問 AI 怎麼辦" — Find a problem you want to solve, ask AI how.

But he acknowledges the gap: he learned coding before ChatGPT existed, through "笨方法學 Python" and "R for Data Science" — old-school debugging, step-by-step. He can't relate to learning programming entirely through AI, so his advice feels hollow.

**The deeper truth**: Start with Google Colab — zero installation, has GPU, runs in browser. You need zero environment setup to start. This is the beginner onboarding he actually recommends.

### How He Really Codes Now

He doesn't write code from scratch. His loop:
1. Describe the problem to Claude Code
2. Claude proposes a solution
3. He critiques: "這樣不行，為什麼要上那麼多層抽象？為什麼不這樣那樣…" (This doesn't work — why so many abstraction layers? Why not do it this way...)
4. Back and forth until the design is right
5. Claude writes the implementation
6. He reviews, tests, deploys

### The "Grand Rounds" AI Talk Strategy

When presenting AI to mixed audiences (novices + veterans):
- **No feature demos** — "去追新功能就是在推薛西弗斯的石頭，到山頂後，一個更新又要從頭推起" (Chasing new features is pushing Sisyphus's boulder — reach the top, one update, start over)
- **No model comparisons** — don't talk about which LLM is better
- **No Claude Design showcases** — trends are fleeting
- **Focus on**: What problems can agentic AI solve for overworked clinicians right now?
- **Principle**: "人多的地方不要去" (Don't go where the crowd is)

---

## 17. Git Safety in the Age of AI Agents

### The Problem

> "近期在用 Claude Code 的夥伴們，並不是每個人都懂得跑 git 的流程，很有可能有些人不懂就放任自己的 agent 執行一些危險操作，force push 那些。"

Non-technical collaborators using Claude Code may let their agents run dangerous git commands. Teaching everyone proper git is impractical.

### The Solution: CLAUDE.md Guardrails

In every repo, add explicit rules to `CLAUDE.md` and `settings.json`:

```
- Never force push
- Never skip hooks (--no-verify)
- Never rebase shared branches
- Always create a new branch for your changes
- Always open a PR, never push directly to main
- If you're unsure about a git operation, ask first
```

The agent reads these files on every session. System-level safety, no human training required.

### System Longevity Thinking

> "一個要活很久的系統，就需要當你本人不在的時候，其他人也可以繼續維護下去。"

For any system meant to last:
- **CI/CD**: How does it handle issues? Who takes over when you're on leave and something breaks?
- **User complaints**: Who mediates? How are feature requests introduced?
- **Build collaboration skills early**: Don't build solo even if you can. Make the system maintainable by others.
- **"Harness Engineers"**: The infrastructure people who keep things running — the unsung heroes without the "-ing" suffix glamour.

---

## 18. LLM Diagnosis & Clinical Reasoning

### The NEJM-AI Perspective

> "LLM 到底能不能診斷疾病？俊廷的那篇回顧，有蠻多文獻就是拿了一堆複雜 case 然後餵給 AI +/- 人類，噢，人類答錯了比較多，所以 AI 取代人類了？"

The typical LLM diagnosis paper: feed complex cases to AI ± human → human makes more errors → "AI replaces doctors!" But:
- Most of these studies are done by clinicians "跟我一樣數學不太好的臨床醫師" (clinicians with weak math, like himself)
- They get an API key, run cases, and can't properly analyze the statistics
- Humans in these studies are constrained — they can't ask follow-up questions, examine the patient, or use clinical intuition
- Human diagnostic process involves: noticing subtle signs, asking the right follow-up questions, physical exam, and pattern recognition from years of experience

### Why AI Diagnosis Studies Are Flawed

His key critique (abstract-ground perspective):
- LLMs excel at pattern matching within their training distribution
- Clinical diagnosis requires recognizing when you're OUTSIDE the distribution
- Real clinicians notice when "something doesn't fit" — the抽象化 (abstract pattern) vs 地基化 (ground-level detail) mismatch
- LLMs lack the embodiment and temporal awareness to notice these discrepancies

### The PGY病歴 AI Prompt

Practical prompt engineering for medical notes:
> "基於目前已知資訊，我還有什麼病史要收集或檢查要做的？以鞏固或排除我們的 DDx"

This avoids the common problem of AI just rephrasing the ED note. Instead, it generates the next clinical step — what history to collect, what exams to order to rule in/out the differential.

**His workflow**: Ask the prompt → walk back to the computer from the patient room → use phone voice input in Chinese → ask AI to translate to English → paste into the note.

---

## 19. The 你快勒馬 Principle (Anti-Overengineering)

### The Parable

> "有一天張飛和關羽快樂的在草地上騎馬，關羽卻不知道他前方是懸崖。張飛就對關羽大叫 : 「你快勒馬 ! 」關羽回頭就對張飛說 : 「我很快樂 ! 」... 於是關羽就掉下崖了。"

The pun: 你快勒馬 (nǐ kuài lēi mǎ) = "Quickly rein your horse!" but sounds like 我很快樂 (wǒ hěn kuài lè) = "I'm very happy!"

### The Lesson

When you have technical ideas, LLMs will **enthusiastically praise them and execute**, burning tokens to build over-engineered, over-abstracted systems you don't actually need. The LLM tells you you're happy as you ride off the cliff.

**The antidote**: Before building anything with AI:
1. Ask: Is this solving a real problem I have right now?
2. Ask: Can this be done with a simple CLI + ripgrep instead of a multi-layered RAG system?
3. Ask: Am I accumulating "me" (insight, skill, judgment) or just "data" (another framework, another wiki)?
4. Let the agent critique your proposal before letting it execute

### The Mystery Framework Encounter

Someone approached him claiming to have built a "world-changing framework" over 6 months, 100k words. When Lin looked at the repo, he couldn't understand what it did. The author insisted: "你用就知道" (Use it and you'll know). Lin refused — he's seen too many open-source repos hiding malicious intent.

Weeks later: the entire project, GitHub account, and Facebook account were **completely deleted** — not even Google cache remnants, just 404s.

> "如果真的對自己的作品引以為傲，就算在旁人眼裡是不堪，也應該要傾注熱情去為它辯護吧？"

If you're truly proud of your work, defend it passionately — even if others dismiss it.

---

## 20. Advanced Anti-Scraping Countermeasures

### Blood Journal Cloudflare Bypass

The challenge: Blood Journal has Cloudflare anti-bot protection, no official RSS, and PubMed API has ~1 week delay. How to get daily updates?

**What failed**: Claude Code tried countless times — changing headers, user agents, request patterns. Cloudflare's human verification blocked everything.

**What worked**: The Chrome Extension relay pattern (documented in §8), plus a fallback strategy using institutional EZproxy access which bypasses Cloudflare entirely since the request comes from the library's authenticated proxy.

### The 42k Police Data Analysis

For fun, he had Claude Code scrape 420,000 police records from Taiwan's National Police Agency and model deterrence:

> "支援鞭刑的論點預設：加重刑罰能降低犯罪。但 2018-2025 台灣資料顯示，(1) 近年嚴罰化改革的『效果』在穩健性檢驗下無一倖存；(2) 同期資料可以清楚識別破獲率的嚇阻效果——破獲率每提高 10%，犯罪下降 3%。嚇阻來自確定性，不是嚴厲性。"

**Finding**: Deterrence comes from **certainty** of getting caught (clearance rate +10% → crime -3%), not **severity** of punishment. All "tougher penalties" effects disappeared under robustness checks.

**Method**: Web scraping + statistical modeling + robustness tests — done entirely via Claude Code, just to prove a point.

---

## 21. Data Analysis Pipelines (Beyond Meta-Analysis)

### Competing Risks Analysis: IT-MTX in DLBCL

His hematology society presentation: "IT-MTX Prophylaxis in DLBCL — A Competing Risks Analysis of Secondary CNS Lymphoma Prevention"

> "在有限資源、cohort 不大的情況下，想辦法跟資料庫纏鬥，清洗資料，古法整理成可以分析的格式。這種 dirty work 短期內還是不會被 AI 取代。"

**Key insight**: The "dirty work" — wrestling with messy databases, cleaning data, transforming it into analyzable format — is NOT getting replaced by AI anytime soon. AI can help with the analysis and writing, but the data wrangling requires human judgment about what's clinically meaningful.

### Book Generation from TOC

> "商業暢銷書，博客來買書頁面上目錄加試讀頁，Prompt：照這架構跟文風把整本書寫完，WebSearch 以 APA 格式引用。常常發現寫出來的東西比原書好耶"

Give Claude a book's table of contents + sample pages from the preview → it writes the entire book. Often the AI version is BETTER than the original. Same approach works for online courses and tutorial series.

### Token Reset Day Experimentation

> "適合在 Token 週重置前把用量燒一燒"

He uses the end of his token billing cycle to run expensive experiments: generate entire books, run large-scale data analyses, process massive datasets. The "wasteful" experiments that teach you the most.

---

## 22. The Deeper Philosophy

### AI Cannot Make Doctors Think Independently

> "從入學開始到畢業，醫學教育制度都是想辦法獎勵聽話的乖孩子。"

Medical education rewards obedience from day one. The senior doctor's advice: "做醫生就是要面笑、嘴甜、腰軟、手腳緊" (Be a doctor: smile, sweet talk, bend at the waist, move fast). From medical student onward: be a good servant.

AI can't fix this. If the system produces doctor-servants, giving them AI produces servant-doctors with AI assistants. The root issue isn't technology — it's the culture of medical training.

### On His Full Auto-Research Prompt Going Viral

His full AI research prompt got ~200k views. Responses fell into camps:
- "So did you submit it?" (No.)
- "AI can't replace critical thinking" (He knows — that's not the point.)
- Luddites who bash AI then pitch their courses/books on "proper AI use"

His actual view: watch Claude Code operate from cursor to cursor. At each step, ask: "Is this scientifically sound? Is this methodologically appropriate? Does this finding make clinical sense?" The human's job is **scientific judgment at each decision point**, not code-writing or literature searching.

### The "Changed World" That Disappeared

A cautionary tale: someone claimed to have built a world-changing framework. Weeks later — GitHub deleted, Facebook deleted, everything gone.

His reflection: if you believe in your work, defend it. Don't disappear. Even if others dismiss it, passion and persistence matter more than perfection.

### Sisyphus and AI Features

> "去追新功能就是在推薛西弗斯的石頭"

Chasing new AI features is like Sisyphus pushing the boulder. Reach the peak, one update, and you start from the bottom. Don't chase features. Chase problems.

### The Speech He Gave to 360+ People

> "雖然講的是自己擅長的題目，壓力來自於有人聽過第二次了，為了不被說偷懶、無聊，所以原本前版的實例也要抓緊時事更新一下。"

Even when lecturing on his expertise, the pressure comes from repeat attendees. He updates examples with current events to avoid being called lazy. This is the craft of teaching — not just knowing the material, but keeping it fresh for those who've heard it before.

---

## Appendix A: Key Repositories Reference

| Repo | Stars | What To Learn |
|------|-------|---------------|
| [dotfiles](https://github.com/htlin222/dotfiles) | 77 | macOS dev environment setup, symlink management, Brewfile |
| [meta-pipe](https://github.com/htlin222/meta-pipe) | 91 | Reproducible 9-stage meta-analysis with AI agents |
| [mini-claw](https://github.com/htlin222/mini-claw) | 92 | Telegram AI bot architecture, session persistence |
| [claude-telegram-bot](https://github.com/htlin222/claude-telegram-bot) | 13 | Claude Code on mobile, MCP integration, file indexing |
| [learn-r-with-ai](https://github.com/htlin222/learn-r-with-ai) | 15 | AI-mediated statistics learning, 30-task curriculum |
| [lizard-gslide-module](https://github.com/htlin222/lizard-gslide-module) | 33 | Google Apps Script automation, clasp-based deployment |
| [end-to-end](https://github.com/htlin222/end-to-end) | — | Agentic research methodology, 3-layer architecture, 4-reviewer system |
| [robust-lit-review](https://github.com/htlin222/robust-lit-review) | — | Automated systematic review (Scopus, PubMed, Embase, DOI validation) |
| [flowdoc](https://github.com/htlin222/flowdoc) | — | Zero-dependency PRISMA/CONSORT/STROBE flow diagram generator |
| [ngs-tertiary-analysis-skills](https://github.com/htlin222/ngs-tertiary-analysis-skills) | — | Clinical genomics: BAM → ESMO clinical report |
| [hematok](https://github.com/htlin222/hematok) | — | TikTok-style medical education PWA (6,973 images) |
| [mcq-bank](https://github.com/htlin222/mcq-bank) | — | Collaborative study system with wiki explanations, mock exams |
| [openevidence-mcp](https://github.com/htlin222/openevidence-mcp) | — | MCP server for medical evidence, Chrome extension anti-bot pattern |
| [lizard-the-linebot](https://github.com/htlin222/lizard-the-linebot) | — | LINE → Turso DB data pipeline, personal knowledge capture |
| [ankiweb-add-card](https://github.com/htlin222/ankiweb-add-card) | — | Reverse-engineered AnkiWeb API CLI |
| [ttyd-tmux-cf](https://github.com/htlin222/ttyd-tmux-cf) | — | Persistent web terminal via Cloudflare Tunnel |
| [kahoot-cf](https://github.com/htlin222/kahoot-cf) | — | Self-hostable 1:1 Kahoot clone on Cloudflare |

## Appendix B: Learning Path (Suggested Order)

1. **Terminal foundation**: Zsh, tmux, Neovim — from [*The Terminal Way*](https://the-terminal-way.netlify.app/)
2. **Git mastery**: lazygit, conventional commits, git hooks, worktrees, semantic versioning
3. **CLI toolchain**: fzf, ripgrep, fd, lsd, bat, zoxide
4. **Dotfiles management**: symlink-based bootstrap, Brewfile, version-controlled config
5. **AI-assisted coding**: Claude Code, prompt engineering (prompt as product), session management
6. **Git safety for AI agents**: CLAUDE.md guardrails, CI/CD design for non-technical collaborators
7. **MCP**: Build a simple MCP server, understand the Chrome extension relay pattern, progressive loading
8. **Agentic workflow**: Single-operator model, propose-and-defend loop, multi-agent reviewer systems
9. **Anti-overengineering**: The 你快勒馬 principle, knowing when NOT to build
10. **Statistics + AI**: R + AI pair programming (the learn-r-with-ai method), meta-analysis pipeline, competing risks
11. **Causal inference**: Counterfactual reasoning, propensity scores, matching/weighting, TMLE
12. **DevOps**: Cloudflare Workers + D1 + R2 + Zero Trust + Tunnels stack
13. **Personal automation**: LINE/Telegram bots, 7 essential physician AI agents, knowledge capture
14. **Anti-bot patterns**: Chrome extension relay, HAR extraction, EZproxy institutional access
15. **LLM diagnosis literacy**: Understanding AI's clinical reasoning limitations, prompting for medical notes
16. **Vibe learning**: Project-based AI-assisted learning, CS146S method, book generation from TOC

---

*Compiled from Hsieh-Ting Lin's Facebook posts, 118 GitHub repositories, blog at htl.physician.tw, and book at the-terminal-way.netlify.app — June 2026.*
