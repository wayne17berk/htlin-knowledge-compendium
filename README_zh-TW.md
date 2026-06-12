# 林協霆醫師 — 知識全集

> 整理自其 Facebook 貼文（265 篇，2022-2026）、GitHub（118 個 repos）、部落格 [htl.physician.tw](http://htl.physician.tw)、及著作 [*The Terminal Way*](https://the-terminal-way.netlify.app/)。
> 林協霆醫師任職於台北和信治癌中心醫院腫瘤內科部，為血液腫瘤科研究醫師，同時也是多產的醫師程式開發者。
> **核心哲學：**「工具塑造思維，思維決定效率」、「我們要的不是那台電鑽，而是牆上那一個洞」

---

## 目錄

1. [終端機精通與開發環境](#1-終端機精通與開發環境)
2. [Git 與版本控制](#2-git-與版本控制)
3. [現代 CLI 工具鏈](#3-現代-cli-工具鏈)
4. [AI／Agentic 工作流程與 MCP](#4-aiagentic-工作流程與-mcp)
5. [統計學與研究方法](#5-統計學與研究方法)
6. [給臨床醫師的因果推論入門](#6-給臨床醫師的因果推論入門)
7. [部署 Agent 產出的產品](#7-部署-agent-產出的產品)
8. [反爬蟲與網路抓取模式](#8-反爬蟲與網路抓取模式)
9. [醫療 AI 應用](#9-醫療-ai-應用)
10. [AI 時代的醫學教育](#10-ai-時代的醫學教育)
11. [個人知識管理](#11-個人知識管理)
12. [自動化與 DevOps](#12-自動化與-devops)
13. [提示工程與 AI 教學法](#13-提示工程與-ai-教學法)
14. [程式語言與技術棧](#14-程式語言與技術棧)
15. [他的哲學與心智模型](#15-他的哲學與心智模型)
16. [Vibe Learning 與程式教學](#16-vibe-learning-與程式教學)
17. [AI Agent 時代的 Git 安全](#17-ai-agent-時代的-git-安全)
18. [LLM 診斷與臨床推理](#18-llm-診斷與臨床推理)
19. [你快勒馬原則（反過度工程）](#19-你快勒馬原則反過度工程)
20. [進階反反爬蟲對策](#20-進階反反爬蟲對策)
21. [資料分析管線](#21-資料分析管線)
22. [更深層的哲學思考](#22-更深層的哲學思考)

---

## 1. 終端機精通與開發環境

### 技術棧（取自其 dotfiles，677 commits）

| 工具 | 用途 | 關鍵配置 |
|------|------|----------|
| **Zsh + Oh-My-Zsh** | Shell | Powerlevel10k 主題、vi mode、autosuggestions、syntax highlighting、fzf-tab |
| **Neovim** | 編輯器 | NvChad 架構、Python 整合（pyenv neovim3 venv） |
| **Tmux** | 終端機多工 | 持久性 session、面板管理 |
| **WezTerm** | 終端機模擬器 | GPU 加速、跨平台 |
| **Hammerspoon** | macOS 自動化 | 視窗管理、自訂快捷鍵 |
| **fzf** | 模糊搜尋 | 檔案搜尋、指令歷史、tab 補全 |
| **ripgrep** | 程式碼搜尋 | 比 grep 更快 |
| **fd** | 檔案搜尋 | 比 find 更快 |
| **lsd** | 檔案列表 | 現代化的 ls，含圖示與顏色 |
| **lazygit** | Git TUI | 終端機 Git 介面 |

### Zsh 外掛堆疊（7 個，lazy-loaded）

```
zsh-autosuggestions   → fish 風格的指令建議
zsh-syntax-highlighting → 即時語法著色
zsh-vi-mode           → shell 中的 vi 快捷鍵
zsh-lazyload          → 延遲載入以加速啟動
zsh-you-should-use    → 提醒你有現成的 alias 可用
fzf-tab               → 透過 fzf 進行模糊 tab 補全
```

### Bootstrap 系統（symlink 架構）

1. Clone 至 `~/.dotfiles`
2. `brew bundle --file="~/.dotfiles/Brewfile"` 安裝所有套件
3. `~/.dotfiles/start/link_dotfiles` 將所有設定 symlink 至 `~`
4. `sh ~/.dotfiles/macos.sh` 套用 macOS 預設值

### 終端機核心技能（取自 *The Terminal Way* 全書 22 章）

- **Shell 精通**: piping、redirection、job control、alias、function
- **Tmux**: session 管理、面板分割、copy mode、腳本化
- **Neovim**: modal editing、LSP、treesitter、自訂 snippets、Python 整合
- **文字處理三劍客**: sed、awk、grep（及其現代替代品）
- **Docker**: 容器化開發環境
- **SSH**: 遠端開發、tunnel、agent forwarding
- **除錯**: strace/dtrace、效能分析、log 分析

---

## 2. Git 與版本控制

### 日常 Git 工作流程（取自 *The Terminal Way* 第 9 章）

- 及早 commit，頻繁 commit
- 使用 conventional format 撰寫有意義的 commit message
- Branch-based 工作流程：feature branch → PR → merge
- Interactive rebase 以維持乾淨的歷史

### 進階 Git 實踐

- **Git hooks**: `.githooks/` 目錄用於 meta-pipe 的可重現性強制執行
- **機密掃描**: dotfiles 中包含 `.gitleaks.toml` 和 `.gitguardian.yaml`
- **Git worktrees**: 用於 claude-telegram-bot 的隔離式風險操作（`/worktree` 指令）
- **BFG**: dotfiles 中包含 `bfg-1.14.0.jar` 用於 git 歷史清理
- **語意化版本**: tagged releases（`v*.*.*`）配合 CI 觸發的建置

### 自動化發佈管線模式

取自其 Anki addon 工作流程：
```
GitHub Release → GitHub Action 觸發 → 自動更新 AnkiWeb 版本
```

他逆向工程 AnkiWeb 的內部 API（官方沒有 npm/pip 式的 GitHub 整合），然後串接到 CI/CD。這個模式——**逆向工程內部 API → 包裝成 CLI → 串接 CI/CD**——在他的專案中反覆出現。

### Git 工具

- **lazygit**: 用於 staging、committing、branching、rebasing 的終端機介面
- **GitHub CLI (gh)**: PR 管理、issue 追蹤、repo 操作
- **claude-with-webhook**: 自架的 Go webhook server，在 git 事件時觸發 Claude Code

---

## 3. 現代 CLI 工具鏈

### 經典工具的現代替代品

| 經典 | 現代替代品 | 原因 |
|------|-----------|------|
| `grep` | `ripgrep` (rg) | 更快、尊重 .gitignore、更好的預設值 |
| `find` | `fd` | 更快、更聰明的預設值、支援 regex |
| `ls` | `lsd` | 圖示、顏色、樹狀檢視 |
| `cat` | `bat` | 語法高亮、行號、分頁 |
| `cd` | `zoxide` | 基於使用頻率的智慧目錄跳轉 |

### fzf — 通用模糊搜尋器

**Shell 整合：**
- `Ctrl-R` → 模糊搜尋指令歷史
- `Ctrl-T` → 模糊搜尋檔案
- `fzf-tab` → 以互動式模糊選單取代 tab 補全

**在腳本中的使用：**
```bash
vim $(fzf)                                    # 互動式選擇檔案
git checkout $(git branch | fzf)               # 互動式切換 branch
kill $(ps aux | fzf | awk '{print $2}')        # 互動式砍 process
```

### 套件與版本管理

- **Homebrew**: 透過 Brewfile 管理 macOS 套件（+ Brewfont 管理字型）
- **pyenv**: Python 版本管理（多版本共存）
- **uv**: 快速的 Python 套件管理器（用於 meta-pipe、end-to-end）
- **pnpm**: Node.js 套件管理器（用於 mini-claw、lizard-linebot）
- **Bun**: 快速的 JS runtime（用於 claude-telegram-bot）
- **renv**: R 套件可重現性（用於 meta-pipe）

---

## 4. AI／Agentic 工作流程與 MCP

### 核心循環：提案 → 被批評 → 辯護或撤退

林醫師將其 agentic 工作流程描述為對話，而非命令：

> 「我常常是對著 Agent 提案，然後被它批鬥一輪：『我覺得這樣不行，你為什麼要上那麼多層抽象？你為什麼不這樣那樣…』，來回交手個幾輪，我很堅持要的功能要說服它。這樣跟 Agent 的互動會讓我把要解決的問題想細一點，有時候甚至會戰略性撤退，趕快轉進把認知資源投注在真正值得解決的問題上。」

### Agentic 工作流程

1. **定義規格** — 寫出清晰、詳細的 prompt 描述任務
2. **讓 agent 執行** — Claude Code 自主執行多步驟管線
3. **Agent 批評你的提案** — 不只是執行；agent 會對設計提出質疑
4. **辯護或撤退** — 由人類判斷哪些值得建造
5. **審查產出** — 在品質關卡進行人類審查
6. **迭代** — 根據結果優化 prompt，歸檔經過驗證的模式

### 睡前研究儀式

> 「最近睡前的儀式都是請 Claude 寫一篇 paper，當作隔天早上的晨間讀物。」

完整 prompt 模板：
> 「我想做個跟 ___ 有關的題目，請幫我看一下最近發表過的類似主題，以 High IF 期刊為偏好及審美。開始幫我想這個題目，設計方法、研究，再依據結果，以它的 novelty、robustness 的程度來看適合哪間期刊、去參考他們的寫作規範，latex + csl + bib ，寫完初稿後，需要請 4 位 subagents 提出 revision 意見，然後我們修改，這樣要至少四輪以上，直到所有 reviewer 都 "ACCEPT"，記得加上 AI usage disclaimer 。最終要交付的東西是一個 github private repo 、所有的 preprint 的素材都要提交要在 release 中」

**這就是 end-to-end agentic 研究管線**——在你睡覺時產出一篇完整論文，並由 subagent 進行多輪同儕審查。此概念已發表於 Nature。

### MCP（Model Context Protocol）— 他的實作

#### Chrome Extension MCP 模式（血淚教訓）

當 OpenEvidence 加入了 DataDome 反爬蟲保護後，所有瀏覽器模擬和 API 逆向工程的方法全部失效：

> 「自從上星期 OE 加入 DataDome 後，用 POST 方法都會撞到 403，內心大概有底所有模擬 browser 、反向解析 API 的方法的鼠貓遊戲都不持久。」

**洞察**: Chrome extension 擁有極大的權限——可以讀取 cookies、開啟分頁、重整頁面。若建立一個 Chrome extension 在本地跑一個 server 並維持 MCP 連線，當 agent 需要查詢受保護的網站時，就透過 MCP 叫 Chrome extension 在背景默默開啟一個分頁。因為該分頁已擁有所有 OE 的 cookies 和瀏覽器指紋，完全不會被 CF 或 DataDome 阻擋。

**Fire-and-forget + polling 模式**：
1. Agent 透過 MCP 提問 → Extension 使用真實瀏覽器 session 開啟背景分頁
2. 立即回傳一個 `id`
3. Agent 幾秒後 polling 取得完整回覆
4. 所有 cookies、瀏覽器指紋、session 資料都是真實的——與真人操作無法區分

**可泛化的模式**: 「之後對付反爬蟲的網站都可以採取這套策略 (將目光望向衛服部網站)。」

#### HAR 檔模式（替代方案）

在 Chrome extension 方案之前，他嘗試了 `har` 檔方法：
1. 從瀏覽器 dev tools 匯出 HAR
2. 從 HAR 中提取 cookies + 瀏覽器指紋
3. 使用兩者來構造能通過 DataDome 行為 ML 偵測的請求

> 「Claude Code 死活不肯動手，各種繞還是被它看穿了我的小心思...那就換 Codex 來用，但 Codex 分析了一輪後，給我一個更務實的建議」

**教訓**: 不同的 AI 模型有不同的道德邊界。一個拒絕了，另一個可能建議更務實的做法。但也可能：拒絕你的模型是在保護你免於採用一個脆弱的解決方案。

#### 其他 MCP / Claude Skills（15+）

- **openevidence-mcp**: TypeScript，瀏覽器 extension relay，從 Claude Code 查詢醫學文獻
- **openevidence-skill**: 純 Python stdlib 可攜版，progressive loading 避免 bloating
- **audit-oe-skill**: 平行 PubMed 驗證 OpenEvidence 引用
- **research-guardian-skill**: 多關卡自動化研究品質驗證
- **drug-drug-skill**: 實證藥物交互作用評估（仿 Micromedex）
- **ebmt-handbook-skill**: EBMT Handbook 第 8 版臨床指引 skill
- **toefl-skill**: TOEFL iBT 備考教練
- **zh-article-analyzer-skill**: 繁體中文文章深度分析
- **zh-ebn-report-skill**: 台灣護理實證報告（N1-N4 升等）教練
- **critique-defense-copilot-skill**: 面對網路攻擊／毀謗的冷靜應對教練
- **gh-repo-father-skill**: 從原始想法到 scaffolded repo 的 GitHub repo bootstrapper

### 多 Agent 架構（取自 end-to-end 專案）

**三層研究系統：**
```
Layer 1: 自主管線 → 產出初稿手稿
Layer 2: 審計 subagent → 檢查 Layer 1 產出，產生審計發現
Layer 3: 外部驗證 → 預先註冊的驗證
```

- **4 個不同角色的 reviewer agent**，各有獨立的 prompt 檔案
- 多輪審查，附逐字記錄
- **單一操作者**模式：一個人類指揮多個 AI agent
- **揭露標準**: prompt + commit hash + tagged release（而非傳統的方法章節敘述）
- 投稿至 *The Lancet Digital Health* Viewpoint，後續概念獲 *Nature* 刊載

### LLM 引用幻覺之爭

> 「2026 年如果一個人還會說 LLM / Agents 會引用虛假文獻的，那他對 LLM / Agents 的理解恐怕還停留在 2023 年」

**「馬具」(Harness) 概念**: 沒有馬具的 LLM 是危險的。有馬具的 LLM 是研究加速器。馬具包含：
- 系統性回顧方法論
- 以 PubMed/CrossRef 驗證引用
- 審計 agent 檢查主要 agent 的產出
- 多關卡品質控制（research-guardian-skill）

馬具就是將「LLM 會產生虛假文獻」與「LLM 加速系統性回顧」區分開來的關鍵。同樣的模型，不同的鷹架。

來自一位同事的回應：「如果需要 expert-level harness 才能可靠，那在產品角度其實就等於對 general population 不可靠——如果一把刀要經過外科訓練才會用，那這把刀對一般人來說就是危險物品。」

---

## 5. 統計學與研究方法

### 統合分析管線（meta-pipe，91 stars）

**9 階段可重現管線**——AI 輔助但統計嚴謹：

| 階段 | 內容 | 產出 |
|------|------|------|
| 01 Protocol | 定義 PICO、納入排除條件 | `pico.yaml`, `eligibility.md` |
| 02 Search | 多資料庫搜尋 | `dedupe.bib` |
| 03 Screening | 標題／摘要篩選 | `decisions.csv` |
| 04 Fulltext | 全文檢索 | `manifest.csv` |
| 05 Extraction | 資料萃取 | `extraction.csv` |
| 06 Analysis | 統計計算 | `figures/`, `tables/` |
| 07 Manuscript | 撰寫 | `manuscript.pdf`（Quarto） |
| 08 Reviews | GRADE 評估 | `grade_summary.md` |
| 09 QA | 最終驗證 | `final_qa_report.md` |

**範例專案**: 三陰性乳癌免疫檢查點抑制劑——5 篇 RCT，N=2,402。**14 小時 vs 100+ 小時手動。**風險比 1.26（95% CI 1.16–1.37，p=0.0015），GRADE 高品質證據。

### EZproxy 全文期刊下載

大多數機構圖書館使用 EZproxy 機制——由圖書館作為代理伺服器幫你取得期刊文章。模式：

1. 登入圖書館 → 取得 cookies
2. URL 轉換: `www.nejm.org` → `www-nejm-org.proxy.institution.edu:PORT/doi/full/{DOI}`
3. 從 HTML 解析 `<meta name="citation_pdf_url">`
4. 包裝成 CLI：一個指令下載全文 PDF
5. 對於 OA 文章：使用 Unpaywall API

未開源（每家圖書館有不同的內部規則），但模式已記錄：「相信大家聰明的 Opus 4.7 跟 GPT 5.5 應該有辦法看得懂。」

### 跨專案的統計方法

- **二元結果統合分析**: RR、OR、RD，固定／隨機效果模型
- **網絡統合分析**: 專屬 `ma-network-meta-analysis` 模組
- **發表偏差**: 漏斗圖、Egger's test、trim-and-fill
- **森林圖**: 標準 + 累積
- **GRADE 架構**: 證據品質評估（⊕⊕⊕⊕）
- **PRISMA 2020 合規**: 完整報告標準
- **存活分析**: Kaplan-Meier 曲線、Cox 回歸、log-rank test
- **MMRM**（混合模型重複測量）: 取自 roche-vabysmo-rwe-workshop
- **CMH**（Cochran-Mantel-Haenszel）: 分層分析
- **基因標記驗證**: Venet 2011 範式——以隨機基因集作為 null benchmark（tcga-brca-reanalysis）
- **拓樸資料分析**: scRNA-seq 細胞狀態可塑性量化（sctda-cancer-plasticity）
- **圖論**: 生物標記驅動指引中的無實證決策點（mbc-evidence-dag-paper）
- **NGS 三級分析**: BAM → ESMO 臨床報告，使用 OncoKB/ESCAT 分類（ngs-tertiary-analysis-skills）

### 以 AI 學習 R 的臨床統計學（learn-r-with-ai）

**30 個任務的課程，6 個部分：**
1. **快速入門**（任務 1-5）: R 環境、基本操作
2. **讀取資料**（任務 6-8）: 匯入、檢查、清理
3. **Table 1**（任務 9-14）: 描述性統計、組間比較
4. **發表級圖表**（任務 15-19）: 盒鬚圖、多面板圖、森林圖、漏斗圖
5. **統計檢定**（任務 20-24）: t-tests、ANOVA、chi-square、log-rank
6. **整合**（任務 25-30）: End-to-end 分析、報告撰寫

### 研究工具（盡可能零相依）

- **flowdoc**: 零相依 TypeScript 工具，產生 PRISMA/CONSORT/STROBE 流程圖（SVG）
- **robust-lit-review**: 自動化系統性回顧管線（Scopus、PubMed、Embase、DOI 驗證）
- **research-publishing-pipeline**: 搭配 Claude Code 的半自動研究寫作管線

---

## 6. 給臨床醫師的因果推論入門

> 取自其 ASH Asia Trainee Day 的授課內容。原則是：「如果一個問題被問三次就發成文好了。」

### 問題

觀察性研究顯示 A 與 B 的關聯。媒體與大眾的直覺：「A 導致了 B」。觀察性研究受到挑戰 → 有人打出「RCT 才是黃金標準」的魔法卡 → 但 RCT 在實務上往往不切實際（成本、倫理、時間）。

### 解決方案：反事實推理（ELI5）

他以 **JJ 林俊傑的〈可惜沒有如果〉** 作為教學範例：

> 「倘若那天，把該說的話好好說，該體諒的不執著…」

在 MV 中，JJ 認為自己的口拙導致了分手。要證明這件事，你需要一個**平行時空**——同樣的人設場景，但這次 JJ 有把該說的話好好說，然後追蹤兩個時空的結果，看最後有沒有在一起。

這就是**反事實推理**：同一個人在不同的介入下會有什麼不同的結果？

### 核心概念（依介紹順序）

| 概念 | ELI5 解釋 |
|------|----------|
| **Treatment Effect** | 介入（treatment）造成的結果（Y）的差異效果 |
| **ATE**（Average Treatment Effect） | 對整個母體平均 treatment effect |
| **Counterfactual** | 除了 treatment 之外一切相同的平行時空 |
| **Propensity Score（傾向分數）** | 總結所有干擾變數成一個接受 treatment 機率的綜合評分 |
| **Matching（匹配）** | 找到傾向分數相近的人——一個有接受 treatment、一個沒有——進行比較 |
| **Weighting（加權）** | 不只 1:1 匹配，而是以傾向分數對觀察值加權 |
| **TMLE**（Targeted Maximum Likelihood Estimation） | 「選他就對了」——近代獲得平衡 cohort 的首選方法，能較有說服力地展示因果效應 |

### 關鍵直覺

- **平行時空不存在** → 取而代之，找到特徵相似的人（一樣帥、一樣會唱歌、戀愛史相似），其中一個「有好好溝通」（treatment），另一個沒有（control）
- **傾向分數** = 將所有干擾變數濃縮成一個數字：接受 treatment 的機率
- 在傾向分數上進行匹配／加權後，可以得到**共變量平衡**——兩組看起來是可比較的，使因果主張更具說服力
- **Unknown unknowns** 和**穩健度檢查**是另一個獨立的討論主題（他說有興趣可以再寫一篇）

### Target Trial Emulation

被提及為設計觀察性研究的框架，使其能模仿 RCT。不是「我們觀察了這些人」，而是如同在設計一個臨床試驗般設計研究，然後尋找符合試驗設計的觀察性資料。

---

## 7. 部署 Agent 產出的產品

> 取自其 Facebook 貼文：Claude 任何 Agent 產生的產品要怎麼發佈。

### 託管平台比較

| 平台 | 最適合 | 關鍵優勢 | 關鍵限制 |
|------|--------|----------|----------|
| **GitHub Pages** | 初學者 | Repo → GitHub Actions → 一鍵部署 | Repo 必須是 public |
| **Netlify** | Astro、一般靜態網站 | 直接上傳資料夾，拖放 HTML 即可 | — |
| **Vercel** | React 生態系 | 針對 Next.js、serverless functions 優化 | 非 React 較不理想 |
| **Cloudflare Pages** | 所有東西 | 可存取整個 CF 宇宙（Workers、D1、R2、Zero Trust） | 需要一個網域 |

### 他的推薦

**「Cloudflare 是賽博菩薩」**——這句話至少在 3 則不同貼文中出現。免費層級極為慷慨，生態系統深厚。

### 「不需要 Agent 之力」的路徑
Netlify：辦個帳號 → 看到上傳區 → 把 HTML 資料夾丟進去 → 完成。主檔案必須命名為 `index.html`。

---

## 8. 反爬蟲與網路抓取模式

### 貓捉老鼠的問題

DataDome（及類似服務）不單純檢查 IP。它們使用 ML 分析：
- 行為模式（點擊速度、捲動行為、「思考速度」）
- 瀏覽器指紋（canvas、WebGL、字型渲染）
- Session 特徵

傳統方法失敗的原因：
- 模擬瀏覽器在 ML 層級可被偵測
- 逆向工程的 API 在目標加入反爬保護時就失效
- 基於 IP 的封鎖會跟你到不同的電腦

### Chrome Extension 架構（推薦方案）

```
Agent (MCP Client) ←→ Local MCP Server ←→ Chrome Extension ←→ 目標網站
                                            (真實 cookies、     (看到真實瀏覽器)
                                             真實指紋)
```

**為什麼有效**: 請求來自真實的 Chrome 瀏覽器，帶著使用者的已驗證 session。DataDome 看到的是一個真人，不是 robot。Extension 開啟背景分頁，發送查詢，立即回傳 ID，然後 agent polling 取得結果。

**倫理考量**: 他用於 OpenEvidence（一個他有權限使用的服務；他繞過的是 robot 偵測，而非付費牆）。「OE 本身有 100 queries/hour 的限制，不要碰到就好」——他尊重速率限制。

### HAR 檔方法（備案）

1. 開啟瀏覽器 dev tools → Network tab
2. 手動執行操作
3. 匯出為 HAR 檔
4. 從 HAR 中提取 cookies + 瀏覽器指紋
5. 使用這些以程式化方式構造請求

**限制**: IP 變更可能使此方法失效。「顯然，我前幾天 Po 的方法一換電腦就失效，大概是 IP 被標記了。」

### EZproxy 模式（合法全文取得）

適用於機構期刊存取：
1. 登入圖書館 → 取得 cookies
2. URL 轉換: `www.journal.org` → `www-journal-org.proxy.institution.edu:PORT/doi/full/{DOI}`
3. 從 HTML 提取 `<meta name="citation_pdf_url">`
4. 包裝成 CLI：一個指令 → 完整 PDF
5. 對於開放存取：Unpaywall API

---

## 9. 醫療 AI 應用

### 實證醫學 AI

- **OpenEvidence MCP**: 在 AI 編程 session 內查詢醫學文獻
- **PubMed/CrossRef 驗證**: 以審計 agent 進行平行引用驗證
- **PICO 改寫**: Groq 驅動的臨床問題表述（用於 Anki addon）
- **證據分級**: meta-pipe 中的自動化 GRADE 評估
- **breast-cancer-uptodate**: 每週自動產生乳癌治療趨勢報告

### 臨床基因組學

- **NGS 三級分析**: R 管線從 BAM 檔到 ESMO 2024 臨床報告
- **AMP/ASCO/CAP 分類**: Agentic AI vs. 規則式變異分類的基準測試
- **OncoKB 整合**: 臨床可行性註釋

### 醫學教育

- **hematok**: TikTok 風格的無限捲動，6,973 張 ASH Image Bank 血液學圖片（React PWA、Cloudflare）
- **hematology-board-review**: 72 個主題筆記 + 216 題自編 ABIM 風格題目（Python + Obsidian + 間隔重複 + AI 教練）
- **hemonc-daily-case**: 每日自動化病例摘要（Claude Code Routine）
- **learn-r-with-ai**: AI 中介的 R 語言學習——「你的工作是問對問題，不是寫對程式」
- **mcq-bank**: 協作式 MCQ 學習系統，含 wiki 式解釋、串列討論、計時模擬考（React、Cloudflare Workers、Hono、Zero Trust）
- **kahoot-cf**: 可自架的 Kahoot 複製品，Cloudflare Workers + Durable Objects + Zero Trust（1,000 人同時上線——Kahoot 收費 $49/月）
- **CompTIA-security-plus-notes**: 83 個主題、563 個概念、332 題練習題——以 Quartz 建構的數位花園

### 臨床工作流程

- **vghtpe-uro**: 以 Google Sheets + clasp（Apps Script）建構的醫院排班系統
- **lizard-gslide-module**: 自動化 Google Slides 格式化——批次處理、主題套用、浮水印切換（33 stars）
- **society-calendar**: AI 自動抓取台灣醫學會行事曆 → Google Calendar 同步

---

## 10. AI 時代的醫學教育

> 取自其引發大量討論的 Facebook 貼文，質疑醫學教育的根基。

### 診斷

今日的醫學教育建立在「共筆」模式上——學生整理講義、背誦、在考試中複製。有了 AI：
> 「現今所有醫學系做共筆都是把老師的講義丟給 NotebookLLM 然後噴一堆重點摘要搭配考古題就可以結案。」

問題不在於使用 AI——而是學生缺乏**先備知識**來評估 AI 的產出。沒有先備知識，就沒有**品味**——無法分辨好壞。學生成為 AI 產出的「應聲蟲」。

### 為什麼「加個批判性思考就好」會失敗

常見的回應是「改教批判性思考而非背誦」。但：
> 「對於醫學生來說，可能就是只在 Prompt 中多加一句『用批判性思考的方式來回答這一題』照樣用 AI 回答，也是一樣照單全收 AI 吐出來的東西。」

在 prompt 中加上「用批判性思考」不會創造批判性思考，如果學生缺乏評估回應的先備知識。

### 他的提案：教科書到考試模式

1. **取消大堂課**——「反正每年都講差不多，學生如我當年也是翹課」
2. **直接從教科書出題**——題目基於特定的教科書章節
3. **需要推理的 USMLE 風格題目**——「請依這段內容，以 USMLE 風格，需要推理，才能想出答案的 5 題」
4. **頻繁的小考**——每次上課都是小考
5. **讓學生自己想辦法用 AI**——「八仙過海，各顯神通」——他們會發展出自己的方法來消化教科書內容

**目標**: 當學生被迫深度接觸原始資料，並被賦予自由以任何方式使用 AI 來掌握該內容時，**元知識、批判思考就會自然長出來**——根本不用人教。

### 醫師必備的 AI Agent 工作系統（他的清單）

> 「我覺得作為一個醫師個體，有以下幾樣必備的AI Agent工作系統流程會讓生活快樂很多」

1. **自動填寫教學評分單**的 Agent
2. **自動上線上課程**的 Agent（登入、觀看、追蹤時數）
3. **收件匣分類 bot**——收集 email、LINE、Facebook 訊息 → 自動分類 → 依習慣加入行事曆和提醒事項
4. **期刊監控系統**——定期將頂尖期刊內容消化成摘要
5. **資訊到輸出的管線**——將資訊整理成簡報、部落格文章、社群媒體發文
6. **自動化研究管線**——可行性評估、圖表生成、文獻收集、排版
7. **Word 文件處理器**——自動處理各種 Word 檔案

---

## 11. 個人知識管理

### LINE 作為資料擷取工具

**lizard-the-linebot 架構：**
```
LINE 轉傳 → Cloudflare Worker（HMAC 驗證）→ Turso DB（冪等寫入）→ Claude Code agent → 行動
```

- **擷取**: 將任何 LINE 訊息轉傳給 bot
- **儲存**: Turso（libSQL）邊緣資料庫，具備型別特定欄位（文字、貼圖、檔案、位置）+ raw_payload 全包
- **二進位內容**: 從 LINE 內容 API 下載，以 `YYYY-MM/<messageId>` 儲存在 Cloudflare R2（LINE 只保留內容 ~7 天）
- **回覆閘控**: 僅回應明確的 @-提及，不回應 `@all` 或 1:1 私訊
- **成本**: 全部三項服務（Cloudflare Workers、Turso、LINE）在個人用量下皆適用免費層級

**哲學**: 「LINE 是黑盒子」——有用的資訊不斷流入，然後消失在糟糕的搜尋和無法匯出中。bot 將 LINE 重新定義為**純粹的擷取裝置**；「DB is the integration surface。」

### Claude Code Session 管理

- **session-collection**: 彙整對話歷史——提取思考過程、可重用的 prompt 模式、最終解法
- 從所有 AI 互動中建立可搜尋的知識庫
- 「思考過程、可重用的 prompt 模式與最終解法」

### 知識管理工具

| 工具 | 用途 |
|------|------|
| **Obsidian** | 臨床筆記、考試準備、連結思考、間隔重複 |
| **Anki** | 間隔重複（自訂 addon 整合 OpenEvidence/UpToDate/PICO） |
| **LINE bot** | 擷取收件匣訊息、會議連結、研究片段 |
| **Newsboat** | RSS 閱讀器（於 dotfiles 中配置） |
| **Todo.txt** | 任務追蹤（於 dotfiles 中配置） |
| **Email tool** | 個人化 TypeScript 郵件處理，減少收件匣雜訊 |

### Anki 生態系（他的貢獻）

- **anki-openevidence-addon**: 將選取的卡片文字傳送至 OpenEvidence/UpToDate/Google，搭配 Groq PICO 改寫
- **ankiweb-add-card**: CLI 工具，透過逆向工程的 AnkiWeb protobuf API 建立牌組／新增卡片——「目前體感上最流暢的做法」
  - 在 `.env` 中儲存帳密
  - 透過 httpx 自動認證取得 cookies
  - 透過 Makefile 包裝成 Claude Code skill
  - 可直接從 claude.ai 網頁介面呼叫
- **AnkiWeb CI/CD**: 由於 AnkiWeb 沒有官方的 GitHub 整合，他逆向工程了其發佈 API，並串接 GitHub Releases → 自動更新 AnkiWeb 版本

---

## 12. 自動化與 DevOps

### Cloudflare 生態系（他的首選平台）

他反覆稱 Cloudflare 為「賽博菩薩」。免費層級幾乎涵蓋個人規模的所有需求：

- **Workers**: Serverless 函式（LINE bot、Kahoot 複製品、Threads CLI OAuth、圖片託管、TLDR extension 後端）
- **Durable Objects**: 有狀態 serverless——即時多人同步（Kahoot 複製品，1,000 人同時使用）
- **D1**: 邊緣 SQLite（圖片託管、Kahoot）
- **R2**: 物件儲存（LINE 附件、圖片託管）
- **Zero Trust**: 存取控制（mcq-bank、ttyd-tmux-cf、Kahoot——只有授權使用者能存取）
- **Pages**: 靜態託管（個人網站、文件）
- **Tunnels**: 安全的本地服務存取，無需開啟 port

### 自動化模式（每日／每週／每月）

| 專案 | 頻率 | 功能 |
|------|------|------|
| **hemonc-daily-case** | 每日 | 產生血液腫瘤科病例摘要 |
| **polish-prompt** | 每日 | LLM 審查每日英文寫作 |
| **breast-cancer-uptodate** | 每週 | 透過 OpenEvidence 產生乳癌治療趨勢報告 |
| **society-calendar** | 隨需 | 抓取醫學會行事曆 → Google Calendar |
| **tma-edu-exam** | 每月 | 自動完成 TMA 繼續教育考試 |
| **lizard-gslide-module** | 隨需 | 批次格式化 Google Slides |
| **owa-sync** | 隨需 | Outlook Web Access 個人同步 |

### 自架基礎設施

- **ttyd-tmux-cf**: 可從任何瀏覽器存取的持久性網路終端機
  - ttyd + tmux 在家中的 Mac mini 上運行
  - Cloudflare Tunnel + Zero Trust 認證
  - Nerd Font 上傳至 R2 以正確渲染
  - 從任何電腦（甚至公用電腦）透過 `term.your-domain.com` 在無痕模式下存取
  - **使用情境**: 某些 WiFi 環境封鎖 SSH 但允許網頁瀏覽
- **img-hosting**: 自架 Imgur 替代品（R2 + D1）
- **pdf-presenter**: 輕量 CLI PDF 簡報器，具備瀏覽器簡報模式
- **claude-with-webhook**: GitHub webhook → 觸發 Claude Code 規劃（Go、Tailscale）

### macOS 自動化

- **Hammerspoon**: 視窗管理、自訂快捷鍵
- **LizardType**: 原生 macOS 按鍵說話聽打（Swift）——按住按鍵、說話、放開、文字貼上至游標處
- **macos.sh**: dotfiles 中的系統預設值腳本
- **launchd**: 服務管理（ttyd-tmux 持久服務）

---

## 13. 提示工程與 AI 教學法

### 將 Prompt 視為產品

取自其 `polish-prompt` 專案：
> 將 prompt 視為「文本產品」——版本管理、評估、迭代。

**工作流程：**
1. 以結構化文字撰寫 prompt
2. 對 prompt 進行版本控制
3. 根據標準評估 AI 產出
4. 根據結果迭代 prompt
5. 歸檔經過驗證的模式以供重複使用

### AI 中介的學習方法（取自 learn-r-with-ai）

**五步驟循環：**
1. 教師給予一個**任務**（以自然語言描述）
2. 學習者將任務描述貼給 AI（ChatGPT/Claude）
3. AI 產生程式碼
4. 學習者在 Positron/Posit.cloud 中執行程式碼
5. 群組一起審查結果並解釋

**核心原則**: 「你的工作是『問對問題』，不是『寫對程式』」

**五個學習目標：**
1. 清楚描述問題，使 AI 能夠撰寫程式碼
2. 大致理解 AI 產生的程式碼在做什麼
3. 知道在發生錯誤時如何要求 AI 修正
4. 產生適合學術論文發表的表格和圖表
5. 擁有一個可重複使用的分析模板

### AI 輔助研究的揭露標準

取自其 *Lancet Digital Health* 投稿：
> 最低揭露 = prompt + commit hash + tagged release
> （而非傳統的方法章節敘述）

每次 AI 互動都被記錄、版本化、並完全發布，沒有事後篩選。整個產出鏈都可以被稽核。

### Claude Code Skill 生態系

他將 skills 視為可攜的、版本化的、可安裝的套件。模式：
```bash
npx skills add <skill-name>
```

跨領域維護的 skills：
- 臨床：drug-drug、ebmt-handbook、zh-ebn-report、audit-oe
- 研究：research-guardian、robust-lit-review
- 教育：toefl、zh-article-analyzer
- 開發：gh-repo-father、agent-skills
- 個人：critique-defense-copilot、line-inbox

---

## 14. 程式語言與技術棧

### 語言分布（跨 118 repos）

| 語言 | 使用量 | 主要領域 |
|------|--------|----------|
| **Python** | ~40% 的 repos | 研究管線、自動化、skills、CLI 工具、逆向工程 |
| **TypeScript** | ~25% 的 repos | Bot（Telegram、LINE）、網頁應用、MCP servers、Cloudflare Workers |
| **R** | ~15% 的 repos | 統計、統合分析、生物資訊學、教學 |
| **Lua** | ~5% | Neovim 設定、Hammerspoon |
| **Shell** | ~5% | Dotfiles、自動化腳本 |
| **JavaScript** | ~5% | Google Apps Script（clasp）、Chrome extensions |
| **Go** | ~3% | Webhook servers、CLI 工具 |
| **Swift** | ~2% | macOS 原生應用（LizardType） |
| **TeX/LaTeX** | 工具 | 手稿、書籍、簡報（Quarto + Typst） |

### 技術棧摘要

```
前端：React, Astro, Vue, Vite, Slidev, Quarto, Typst
後端：Cloudflare Workers, Hono, Python/FastAPI, Go
資料庫：Turso (libSQL), D1, R2, SQLite
AI：   Claude Code, Claude Agent SDK, MCP, Groq, OpenAI
DevOps：Cloudflare, Docker, GitHub Actions, Netlify, clasp
行動：Telegram Bot (grammY), LINE Bot, PWA
桌面：macOS, Neovim, Tmux, Hammerspoon, LizardType
```

---

## 15. 他的哲學與心智模型

### 「我們要的不是電鑽，而是牆上那個洞」

> 「記得我們要的不是那台電鑽，而是牆上那一個洞。雖然分享最新電鑽的文比較有流量。」

不要追逐最新的 AI 工具。追逐你需要解決的問題。分享最新的電鑽獲得更多讚，但洞才是重點。

**如何體現**：
- 他不用 Claude Design、不玩 AI 生圖、沒在追 NotebookLLM
- 他不安裝每個新 AI 東西（龍蝦裝不到半小時就刪了）
- 他專注於建立能解決其臨床和研究工作流程中具體、真實問題的工具
- 然後分享他的成果——不是為了追逐潮流，而是因為這個解決方案可能幫助其他人

### Agent 作為設計評論者，而非單純的執行者

他不是只對 agent 下指令——他提出解決方案，讓 agent 批評它們。Agent 會反駁不必要的抽象層次、建議更簡單的做法，迫使他更深入地思考他真正需要的東西。有時候正確的決定是撤退，將認知資源重新導向。

### 「馬具」(Harness) 概念

沒有馬具的 LLM 是危險的。有馬具的 LLM 是研究加速器。馬具包含：
- 系統性回顧方法論
- 以 PubMed/CrossRef 驗證引用
- 審計 agent 檢查主要 agent 的產出
- 多關卡品質控制（research-guardian-skill）

馬具是區分「LLMs 會產生虛假文獻」與「LLMs 加速系統性回顧」的關鍵。同樣的模型，不同的鷹架。

### 逆向工程然後產品化

在專案中反覆出現的模式（AnkiWeb、OpenEvidence、LINE、醫院系統）：
1. 識別缺乏 API 的平台
2. 逆向工程內部 API
3. 包裝成乾淨的 CLI
4. 串接 CI/CD 或包裝成 skill
5. 分享模式，即使具體實作無法開源

### 先解決自己的問題

幾乎每個專案都在解決他個人遇到的問題：
- LINE 訊息消失 → LINE bot
- Anki 卡片建立很煩 → AnkiWeb CLI
- Google Slides 格式化很重複 → lizard-gslide-module
- 醫院排班一片混亂 → Google Sheets 排班系統
- 醫學教育淪為背誦 → hematok、board review、MCQ bank
- 證據檢索太慢 → OpenEvidence MCP
- 醫院 WiFi 封鎖 SSH → ttyd-tmux-cf

這就是「dogfooding」方法：建造你需要的，然後分享。

### Cloudflare 作為通用基礎設施

他稱 Cloudflare 為「賽博菩薩」，因為它為個人專案提供生產級基礎設施且零成本。Workers、D1、R2、Durable Objects、Zero Trust、Tunnels——一個完整的後端棧，全部在免費層級。

### 「三次」規則

> 「如果一個問題被問三次就發成文好了」

如果有人問同樣的問題三次，就把答案變成一篇公開文章。這就是他決定寫什麼的方式——社群需求，而非內容規劃。

---

## 16. Vibe Learning 與程式教學

### CS146S —— 動手做，不盯著看

> 「2025 之後，發現自己真的難以『看著影片學會東西』。有了 Claude Code 這把錘子，看什麼都是釘子，直接動手比較有樂趣。」

他學習任何技術的方法：
1. 找到課程大綱／網站（例如 Stanford CS146S）
2. 將課程連結 + 你的 GitHub 帳號提供給 Claude Code
3. 說：「看看這個課程，搜搜網路上大家的學習筆記 esp. github repos，幫我設計一個 project，循序漸進地學習」
4. 在 AI 的引導下建立專案，邊做邊學

不看影片。不線性閱讀教科書。建立真實的東西，透過建構來學習。

### 他給初學者的「糟糕」程式建議

他承認他的建議「幾乎是幹話的等級」：
> 「找個想解決的問題問問 AI 怎麼辦」

但他認知到落差：他在 ChatGPT 出現之前學會寫程式，透過「笨方法學 Python」和「R for Data Science」——老派的除錯，一步一步來。他無法體會完全透過 AI 學習程式的感受，所以他的建議感覺空洞。

**更深層的事實**: 從 Google Colab 開始——零安裝、有 GPU、在瀏覽器中運行。你需要零環境設定就能開始。這才是他實際推薦的初學者入門方式。

### 他現在真正如何寫程式

他不從頭寫程式。他的循環：
1. 向 Claude Code 描述問題
2. Claude 提出解決方案
3. 他批評：「這樣不行，為什麼要上那麼多層抽象？為什麼不這樣那樣…」
4. 來回直到設計正確
5. Claude 寫出實作
6. 他審查、測試、部署

### 大晨會 AI 演講策略

向混合聽眾（新手 + 老手）呈現 AI 時：
- **不展示功能**——「去追新功能就是在推薛西弗斯的石頭，到山頂後，一個更新又要從頭推起」
- **不比模型**——不談哪個 LLM 比較好
- **不展示 Claude Design**——潮流轉瞬即逝
- **聚焦於**: 什麼問題可以讓 agentic AI 為過勞的臨床醫師解決？
- **原則**: 「人多的地方不要去」

---

## 17. AI Agent 時代的 Git 安全

### 問題

> 「近期在用 Claude Code 的夥伴們，並不是每個人都懂得跑 git 的流程，很有可能有些人不懂就放任自己的 agent 執行一些危險操作，force push 那些。」

使用 Claude Code 的非技術協作者可能讓他們的 agent 執行危險的 git 指令。教每個人都學會正確的 git 是不切實際的。

### 解決方案：CLAUDE.md 護欄

在每個 repo 中，加入明確的規則到 `CLAUDE.md` 和 `settings.json`：

```
- 永不 force push
- 永不跳過 hooks（--no-verify）
- 永不 rebase 共享的 branch
- 永遠為你的變更建立新的 branch
- 永遠開啟 PR，永不直接 push 到 main
- 如果你對某個 git 操作不確定，先問
```

Agent 在每個 session 都會讀取這些檔案。系統層級的安全，不需要人類培訓。

### 系統長壽思維

> 「一個要活很久的系統，就需要當你本人不在的時候，其他人也可以繼續維護下去。」

對於任何要長久存在的系統：
- **CI/CD**: 如何處理各種 issue？如果你請假東西壞了，誰接手？
- **使用者抱怨**: 誰出面協調？功能需求如何導入？
- **盡早建立協作能力**: 即使能獨立完成也不要只有你自己。讓系統能被他人維護。
- **「Harness Engineers」**: 維持系統運作的基礎設施人員——沒有光鮮亮麗的「-ing」後綴，但是真正的無名英雄。

---

## 18. LLM 診斷與臨床推理

### NEJM-AI 觀點

> 「LLM 到底能不能診斷疾病？俊廷的那篇回顧，有蠻多文獻就是拿了一堆複雜 case 然後餵給 AI +/- 人類，噢，人類答錯了比較多，所以 AI 取代人類了？」

典型的 LLM 診斷論文：餵給 AI 複雜案例 ± 人類 → 人類犯較多錯誤 →「AI 取代醫師！」但是：
- 大部分這類研究是由「跟我一樣數學不太好的臨床醫師」所進行的
- 他們拿到 API key、跑案例、卻無法妥善分析統計
- 這些研究中的人類受到限制——他們不能問追蹤問題、不能檢查病人、不能使用臨床直覺
- 人類的診斷過程涉及：注意到微妙的徵兆、問對的追蹤問題、理學檢查、多年經驗累積的模式識別

### 為什麼 AI 診斷研究有缺陷

他的關鍵批評（抽象化─地基化觀點）：
- LLM 在其訓練分布內的模式匹配表現出色
- 臨床診斷需要識別何時你處於分布之外
- 真實的臨床醫師會注意到「有什麼地方不對勁」——抽象化（抽象模式）與地基化（地面層細節）之間的不匹配
- LLM 缺乏體現性和時間意識來注意到這些差異

### PGY 病歷 AI Prompt

給病歷撰寫的實用 prompt engineering：
> 「基於目前已知資訊，我還有什麼病史要收集或檢查要做的？以鞏固或排除我們的 DDx」

這避免了常見的 AI 僅僅重述急診病歷的問題。取而代之，它產生了下一個臨床步驟——要收集哪些病史、要安排哪些檢查來 rule in/out 鑑別診斷。

**他的工作流程**: 問 prompt → 從病房走回電腦 → 用手機語音輸入中文 → 請 AI 翻成英文 → 貼進病歷。

---

## 19. 你快勒馬原則（反過度工程）

### 寓言故事

> 「有一天張飛和關羽快樂的在草地上騎馬，關羽卻不知道他前方是懸崖。張飛就對關羽大叫 : 『你快勒馬 ! 』關羽回頭就對張飛說 : 『我很快樂 ! 』... 於是關羽就掉下崖了。」

雙關語：你快勒馬（趕快勒住馬）聽起來像「我很快樂」！

### 教訓

當你有技術想法時，LLM 會**熱情地讚美並執行它們**，燃燒 token 來建立你根本不需的過度工程、過度抽象的系統。LLM 在你騎向懸崖時告訴你「我很快樂」。

**解藥**: 在用 AI 建造任何東西之前：
1. 問：這是否在解決我現在真實存在的問題？
2. 問：這能用簡單的 CLI + ripgrep 完成，而不需要多層 RAG 系統嗎？
3. 問：我在累積「我」（洞察、技能、判斷）還是只是累積「資料」（又一個框架、又一個 wiki）？
4. 讓 agent 在執行之前先批評你的提案

### 神秘框架遭遇

有人來找他，聲稱用半年寫了十萬字，做出一個「可以改變世界的框架」。林醫師看了 repo，完全搞不清楚裡面在賣什麼藥。作者堅持：「你用就知道。」林醫師拒絕了——他看過太多開源文件裡面包藏禍心的案例。

幾週後：整個專案、GitHub 帳號、Facebook 帳號**完全被刪除**——連 Google 快照都沒有，只剩 404。

> 「如果真的對自己的作品引以為傲，就算在旁人眼裡是不堪，也應該要傾注熱情去為它辯護吧？」

如果你真的以你的作品為傲，捍衛它——即使別人看不起它。

---

## 20. 進階反反爬蟲對策

### Blood Journal Cloudflare 繞過

挑戰：Blood Journal 有 Cloudflare 反爬蟲機制、沒有官方 RSS、PubMed API 有大約一週的時間差。如何每日自動化取得最新文獻？

**失敗的方法**: Claude Code 嘗試了無數次——更換 headers、user agents、請求模式。Cloudflare 的人類驗證阻擋了一切。

**成功的方法**: Chrome Extension relay 模式（見 §8），加上使用機構 EZproxy 存取的備援策略，後者完全繞過 Cloudflare，因為請求來自有權限的圖書館代理伺服器。

### 42 萬筆警政資料分析

他閒來無事，請 Claude Code 搜集 42 萬筆警政署資料，然後建模跑一下：

> 「支援鞭刑的論點預設：加重刑罰能降低犯罪。但 2018-2025 台灣資料顯示，(1) 近年嚴罰化改革的『效果』在穩健性檢驗下無一倖存；(2) 同期資料可以清楚識別破獲率的嚇阻效果——破獲率每提高 10%，犯罪下降 3%。嚇阻來自確定性，不是嚴厲性。」

**發現**: 嚇阻效果來自**確定性**（破獲率 +10% → 犯罪 -3%），而非**嚴厲性**。所有「加重刑罰」的效果在穩健性檢驗下全部消失。

**方法**: 網路爬蟲 + 統計建模 + 穩健性測試——完全透過 Claude Code 完成，只為了證明一個觀點。

---

## 21. 資料分析管線

### 競爭風險分析：IT-MTX 在 DLBCL 的應用

他在血液年會的報告：「IT-MTX Prophylaxis in DLBCL — A Competing Risks Analysis of Secondary CNS Lymphoma Prevention」

> 「在有限資源、cohort 不大的情況下，想辦法跟資料庫纏鬥，清洗資料，古法整理成可以分析的格式。這種 dirty work 短期內還是不會被 AI 取代。」

**關鍵洞察**: 「骯髒工作」——與混亂的資料庫搏鬥、清理資料、轉換成可分析的格式——在短期內不會被 AI 取代。AI 可以幫助分析和寫作，但資料整理需要人類對臨床意義的判斷。

### 從目錄生成整本書

> 「商業暢銷書，博客來買書頁面上目錄加試讀頁，Prompt：照這架構跟文風把整本書寫完，WebSearch 以 APA 格式引用。常常發現寫出來的東西比原書好耶」

給 Claude 一本書的目錄 + 試讀頁面樣本 → 它寫出整本書。AI 版本通常比原書更好。同樣的方法適用於線上課程和教學系列。

### Token 重置日實驗

> 「適合在 Token 週重置前把用量燒一燒」

他在 token 計費週期結束前，用來運行昂貴的實驗：產生整本書、運行大規模資料分析、處理海量資料集。這些「浪費」的實驗教會你最多。

---

## 22. 更深層的哲學思考

### AI 無法讓醫師獨立思考

> 「從入學開始到畢業，醫學教育制度都是想辦法獎勵聽話的乖孩子。」

醫學教育從第一天起就獎勵服從。資深醫師的建議：「做醫生就是要面笑、嘴甜、腰軟、手腳緊。」從醫學生娃娃開始，就要大家當個好奴才。

AI 無法解決這個問題。如果體系生產醫師僕人，給他們 AI 只會產出有 AI 助理的僕人醫師。根本問題不是科技——而是醫學訓練的文化。

### 他的全自動研究 Prompt 爆紅之後

他的 AI 全自動做研究 prompt 達到 ~20 萬瀏覽。回應分成幾個陣營：
- 「所以你拿出去發了嗎？」（沒有。）
- 「AI 無法取代批判性思考」（他知道——那不是重點。）
- 盧德主義者先罵一頓 AI，然後趕快叫讀者報名他的課程、買他的書，來學會「正確使用 AI 」

他真正的觀點：看 Claude Code 從游標到游標的運作。在每一步問：「這在科學上是否合理？這在方法學上是否適切？這個發現是否有臨床意義？」人類的工作是**在每個決策點的科學判斷**，而不是寫程式或做文獻搜尋。

### 「改變世界」的消失

一個警示故事：有人聲稱做出了一個可以改變世界的框架。幾週後——GitHub 被刪除、Facebook 被刪除，一切消失。

他的反思：如果你相信你的作品，捍衛它。不要消失。即使別人看不起它，熱情和堅持比完美更重要。

### 薛西弗斯與 AI 功能

> 「去追新功能就是在推薛西弗斯的石頭」

追逐新的 AI 功能就像薛西弗斯推石頭。到達山頂，一個更新，又要從頭推起。不要追逐功能。追逐問題。

### 給 360+ 人的演講

> 「雖然講的是自己擅長的題目，壓力來自於有人聽過第二次了，為了不被說偷懶、無聊，所以原本前版的實例也要抓緊時事更新一下。」

即使是在自己專精的題目上演講，壓力來自於重複參加的人。他用時事更新範例，以免被說偷懶。這就是教學的工藝——不只是懂內容，而是讓聽過的人也能感到新鮮。

---

## 附錄 A：關鍵 Repositories 參考

| Repo | Stars | 學習重點 |
|------|-------|----------|
| [dotfiles](https://github.com/htlin222/dotfiles) | 77 | macOS 開發環境設定、symlink 管理、Brewfile |
| [meta-pipe](https://github.com/htlin222/meta-pipe) | 91 | 可重現的 9 階段統合分析，搭配 AI agent |
| [mini-claw](https://github.com/htlin222/mini-claw) | 92 | Telegram AI bot 架構、session 持久性 |
| [claude-telegram-bot](https://github.com/htlin222/claude-telegram-bot) | 13 | 手機上的 Claude Code、MCP 整合、檔案索引 |
| [learn-r-with-ai](https://github.com/htlin222/learn-r-with-ai) | 15 | AI 中介的統計學習、30 任務課程 |
| [lizard-gslide-module](https://github.com/htlin222/lizard-gslide-module) | 33 | Google Apps Script 自動化、clasp 部署 |
| [end-to-end](https://github.com/htlin222/end-to-end) | — | Agentic 研究方法論、3 層架構、4 reviewer 系統 |
| [robust-lit-review](https://github.com/htlin222/robust-lit-review) | — | 自動化系統性回顧（Scopus、PubMed、Embase、DOI 驗證） |
| [flowdoc](https://github.com/htlin222/flowdoc) | — | 零相依 PRISMA/CONSORT/STROBE 流程圖產生器 |
| [hematok](https://github.com/htlin222/hematok) | — | TikTok 風格醫學教育 PWA（6,973 張圖片） |
| [openevidence-mcp](https://github.com/htlin222/openevidence-mcp) | — | 醫學文獻 MCP server、Chrome extension 反爬蟲模式 |
| [lizard-the-linebot](https://github.com/htlin222/lizard-the-linebot) | — | LINE → Turso DB 資料管線、個人知識擷取 |
| [ankiweb-add-card](https://github.com/htlin222/ankiweb-add-card) | — | 逆向工程的 AnkiWeb API CLI |
| [ttyd-tmux-cf](https://github.com/htlin222/ttyd-tmux-cf) | — | 透過 Cloudflare Tunnel 的持久性網路終端機 |

## 附錄 B：建議學習路徑

1. **終端機基礎**: Zsh、tmux、Neovim——從 [*The Terminal Way*](https://the-terminal-way.netlify.app/) 開始
2. **Git 精通**: lazygit、conventional commits、git hooks、worktrees、語意化版本
3. **CLI 工具鏈**: fzf、ripgrep、fd、lsd、bat、zoxide
4. **Dotfiles 管理**: symlink bootstrap、Brewfile、版本控制設定
5. **AI 輔助編程**: Claude Code、prompt engineering（prompt as product）、session 管理
6. **AI agent 的 Git 安全**: CLAUDE.md 護欄、為非技術協作者設計 CI/CD
7. **MCP**: 建立簡單的 MCP server、理解 Chrome extension relay 模式、progressive loading
8. **Agentic 工作流程**: 單一操作者模型、提案與辯護循環、多 agent reviewer 系統
9. **反過度工程**: 你快勒馬原則、知道何時不要建造
10. **統計 + AI**: R + AI 配對程式設計（learn-r-with-ai 方法）、統合分析管線、競爭風險
11. **因果推論**: 反事實推理、傾向分數、匹配／加權、TMLE
12. **DevOps**: Cloudflare Workers + D1 + R2 + Zero Trust + Tunnels 棧
13. **個人自動化**: LINE/Telegram bot、7 大必備醫師 AI agent、知識擷取
14. **反爬蟲模式**: Chrome extension relay、HAR 提取、EZproxy 機構存取
15. **LLM 診斷素養**: 理解 AI 的臨床推理限制、為病歷撰寫設計 prompt
16. **Vibe learning**: 專案導向的 AI 輔助學習、CS146S 方法、從目錄生成書籍

---

*整理自林協霆醫師的 Facebook 貼文（265 篇）、118 個 GitHub repositories、部落格 htl.physician.tw、及著作 The Terminal Way——2026 年 6 月*
