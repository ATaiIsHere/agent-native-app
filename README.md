# Agent-native App Architecture

> **Personal Agent as the AI Runtime for Apps.**

Language / 語言： [繁體中文](#繁體中文) | [English](#english)

---

## 繁體中文

### 一句話

**Agent-native App** 是一種應用架構：App 負責可靠的資料、UI、API、工具與執行環境；需要 AI 判斷的邏輯，則委派給使用者自己的 **Personal Agent**。

它不是「每個 App 內建一個 chatbot」，而是讓 App 能被使用者的 Agent 理解、操作、維護，並接手需要 AI reasoning 的工作。

---

### 這個概念從哪裡來

這個模式來自實際使用 Hermes Agent 時建立多個自架服務的經驗，例如入口頁、File Browser、Mochi Office、S400 健康儀表板、Mochi Quest。

過程中遇到三個問題：

1. **Agent 目錄變髒**
   一開始把服務的 runtime data、DB、cache、uploads 都塞在 agent home 裡，導致 agent 目錄越來越亂。

2. **有些服務需要 AI，但不該各自養 Agent**
   像 Mochi Quest 這種服務，需要 AI 來拆目標、調整任務、判斷 reward 衝突。但如果每個 App 都內建一套 agent lifecycle，會重複管理 memory、prompt、permission、排程與治理。

3. **SaaS + Skill 不夠彈性**
   原本可以用現有 SaaS，再寫 Skill 教 Agent 何時操作。但 Skill 會依賴 SaaS 的資料模型與限制；若要做成通用 Skill 又太抽象，也不容易擴充成自己想要的系統。

所以最後形成這個邊界：

```text
App：UI / data / API / MCP server / validation / runtime state
Personal Agent：user context / reasoning / planning / AI decisions / cross-app orchestration
```

服務包成 Docker 獨立運行，資料留在服務自己的邊界內；Agent 目錄只安裝 Skill、connector、config。這樣 agent home 保持乾淨，服務也不依賴特定 Agent。Hermes Agent、Claude Code、Codex 或其他 MCP-capable agent 都可以接入。

---

### 傳統 AI App vs Agent-native App

```text
Traditional AI App
= App + embedded AI + isolated memory + product-specific chatbot

Agent-native App
= App + deterministic core + API/MCP/events/Skill
+ Personal Agent as external AI runtime
```

差異：

- 傳統 AI App：每個產品各自內建 AI，記憶與權限分散。
- Agent-native App：App 保持穩定可靠；使用者自己的 Agent 負責長期脈絡、AI 判斷與跨 App 編排。

---

### 架構分層

一個 Agent-native App 通常包含：

1. **Runtime Layer**
   Docker / Compose、API server、DB、Web UI、workers。

2. **Deterministic Core**
   app 狀態、資料模型、validation、權限、business rules、migration。

3. **Agent Connector Layer**
   MCP tools / resources、HTTP API、events / webhooks、structured schemas。

4. **AI Logic Contract**
   宣告哪些地方需要 AI 判斷：trigger、需要讀哪些 state、輸出 schema、write-back tool、approval rule。

5. **Agent Guidance Layer**
   `SKILL.md` 或等價文件，告訴 Agent 這個 App 什麼時候該用、怎麼用、哪些行為安全、哪些需要 human approval。

6. **Ops Contract**
   README / RUNBOOK / healthcheck / backup / restore / migration / rollback。

---

### 最小套件

一個最小 Agent-native App 可以長這樣：

```text
compose.yaml                 # reproducible runtime
app.yaml                     # machine-readable app manifest
ai-logic-contract.yaml       # AI delegation points
SKILL.md                     # agent guidance
README.md / RUNBOOK.md       # setup, ops, backup, troubleshooting
MCP server                   # tools/resources exposed to agents
Human UI                     # direct user control and visibility
Healthcheck                  # verifiable runtime status
```

重點不是檔名一定相同，而是邊界清楚：

```text
App owns state and deterministic behavior.
Agent owns user context and AI reasoning.
```

---

### 不是什麼

Agent-native App 不是：

- App 內建 chatbot
- 每個 SaaS 自己的 AI assistant
- MCP 的替代品
- 新的 agent framework
- 新的 LLM runtime
- Hermes 專屬 convention

它是把 Docker、API、MCP、Skill、README/RUNBOOK 組合成一個能被 Personal Agent 安裝、理解、操作與維護的應用模式。

---

### 範例：Mochi Quest

Mochi Quest 是一個個人成長 / 任務系統。

App 負責 deterministic logic：

- goals / tasks / rewards / streaks / wallet / assessments
- task completion ledger
- active plan state
- API validation
- Web UI

Personal Agent 負責 AI logic：

- goal clarification
- plan generation
- replan decision
- task difficulty adjustment
- reward conflict review
- daily review and notification decision

這讓 Mochi Quest 不需要內建自己的 agent。它只需要提供資料、UI、API、MCP tools 和 Skill；使用者自己的 Agent 會接手 AI reasoning。

---

### 目前狀態

這是一個 **concept draft / emerging pattern**，不是完成的標準。

目標是把一個實際踩坑後形成的模式講清楚：

> 讓 App 保有可靠狀態與服務邊界，讓使用者的 Personal Agent 成為可替換、可治理、跨 App 的 AI runtime。

---

## English

### One-liner

**Agent-native App** is an application architecture where the app owns reliable data, UI, APIs, tools, and runtime boundaries, while AI decision-making is delegated to the user's **personal agent**.

It is not about embedding a chatbot into every app. It is about making apps understandable, operable, maintainable, and extensible by the user's own agent.

---

### Where this came from

This pattern came from running Hermes Agent with multiple self-hosted services: an entry page, file browser, Mochi Office, S400 health dashboard, and Mochi Quest.

Three practical problems appeared:

1. **The agent home became messy**
   At first, service runtime data, databases, caches, uploads, and generated files lived too close to the agent home directory. The agent workspace became a dumping ground.

2. **Some services needed AI, but should not own an agent lifecycle**
   Apps like Mochi Quest need AI to break down goals, adjust tasks, and review reward conflicts. But embedding a separate agent lifecycle into every app would duplicate memory, prompts, permissions, scheduling, and governance.

3. **SaaS + Skill was not flexible enough**
   One option was to use existing SaaS products and write Skills for the agent. That works when the SaaS already fits the workflow, but Skills become tightly coupled to SaaS-specific data models and limitations. Making the Skill generic often makes it too abstract.

The cleaner boundary became:

```text
App: UI / data / API / MCP server / validation / runtime state
Personal Agent: user context / reasoning / planning / AI decisions / cross-app orchestration
```

Each service runs independently, usually as a Dockerized app. App data stays inside the app boundary. The agent home only installs Skills, connectors, and config. This keeps the agent environment clean and makes the app independent from a specific agent implementation. Hermes Agent, Claude Code, Codex, or other MCP-capable agents can connect to it.

---

### Traditional AI App vs Agent-native App

```text
Traditional AI App
= App + embedded AI + isolated memory + product-specific chatbot

Agent-native App
= App + deterministic core + API/MCP/events/Skill
+ Personal Agent as external AI runtime
```

The difference:

- Traditional AI apps embed product-specific AI, causing fragmented memory and permissions.
- Agent-native Apps keep the app reliable and deterministic, while the user's personal agent handles long-term context, AI decisions, and cross-app orchestration.

---

### Architecture layers

An Agent-native App usually includes:

1. **Runtime Layer**
   Docker / Compose, API server, database, Web UI, workers.

2. **Deterministic Core**
   App state, data model, validation, permissions, business rules, migrations.

3. **Agent Connector Layer**
   MCP tools / resources, HTTP APIs, events / webhooks, structured schemas.

4. **AI Logic Contract**
   Declares which decisions require AI: trigger, required state, output schema, write-back tool, approval rule.

5. **Agent Guidance Layer**
   `SKILL.md` or an equivalent file that tells the agent when to use the app, how to use it, which actions are safe, and which actions require human approval.

6. **Ops Contract**
   README / RUNBOOK / healthcheck / backup / restore / migration / rollback.

---

### Minimal package

A minimal Agent-native App may look like this:

```text
compose.yaml                 # reproducible runtime
app.yaml                     # machine-readable app manifest
ai-logic-contract.yaml       # AI delegation points
SKILL.md                     # agent guidance
README.md / RUNBOOK.md       # setup, ops, backup, troubleshooting
MCP server                   # tools/resources exposed to agents
Human UI                     # direct user control and visibility
Healthcheck                  # verifiable runtime status
```

The exact filenames are less important than the boundary:

```text
App owns state and deterministic behavior.
Agent owns user context and AI reasoning.
```

---

### What this is not

Agent-native App is not:

- an app with an embedded chatbot
- a SaaS-specific AI assistant
- a replacement for MCP
- a new agent framework
- a new LLM runtime
- a Hermes-only convention

It is an application pattern that combines Docker, APIs, MCP, Skills, and README/RUNBOOK-style operations so an app can be installed, understood, operated, and maintained by a personal agent.

---

### Example: Mochi Quest

Mochi Quest is a personal growth / quest system.

The app owns deterministic logic:

- goals / tasks / rewards / streaks / wallet / assessments
- task completion ledger
- active plan state
- API validation
- Web UI

The personal agent owns AI logic:

- goal clarification
- plan generation
- replan decisions
- task difficulty adjustment
- reward conflict review
- daily review and notification decisions

This means Mochi Quest does not need to embed its own agent. It only needs to expose data, UI, APIs, MCP tools, and a Skill. The user's personal agent handles the AI reasoning.

---

### Current status

This is a **concept draft / emerging pattern**, not a finished standard.

The goal is to describe a practical pattern that emerged from real system-building:

> Keep apps as reliable service boundaries, and let the user's personal agent become a replaceable, governable, cross-app AI runtime.

---

## License

MIT. See [`LICENSE`](LICENSE).
