<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=28&pause=1000&color=7C3AED&center=true&vCenter=true&width=600&lines=Hey%2C+I'm+Abhay+Padmanabhan+%F0%9F%91%8B;AI+Engineer+%E2%80%94+Builds.+Ships.+Owns.;RAG+%7C+Voice+Agents+%7C+LLM+Orchestration;From+blank+repo+to+deployed+product." alt="Typing SVG" />

<br/>

[![Portfolio](https://img.shields.io/badge/Portfolio-abhaypadmanabhan.com-7C3AED?style=for-the-badge&logo=google-chrome&logoColor=white)](https://abhaypadmanabhan.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-apadmanabhan98-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/apadmanabhan98)
[![Email](https://img.shields.io/badge/Email-abhaykerala%40gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:abhaykerala@gmail.com)
[![Location](https://img.shields.io/badge/San_Francisco,_CA-📍-gray?style=for-the-badge)](.)

</div>

---

## About Me

I'm an AI Engineer who builds and ships full-stack production AI systems — solo, end-to-end. In the last year I've shipped a RAG learning platform, a voice dictation desktop app, a real-time automotive voice AI agent, and a multi-agent VS Code extension. I don't just prompt; I build the full stack around the AI layer.

**Background:** 3 years enterprise security automation at Wipro → MS Business Analytics at UC Davis (practicum at The Best Notary) → solo production AI product development.

**What I work with daily:** Claude Code, OpenAI Codex, FastAPI, Next.js, LangChain, Qdrant, LiveKit, Electron.

---

## What I've Shipped

### 🤖 AI Products (production, deployed)

| Project | What it does | Stack |
|---------|-------------|-------|
| [**AutoCoach**](#-autocoach--rag-learning-platform) | AI learning platform with RAG pipelines, adaptive quizzes, and Bayesian mastery tracking | FastAPI · Next.js 16 · LangChain · Qdrant · Kimi K2.5 · GPT-4o-mini · Supabase |
| [**Voxi**](#-voxi--voice-dictation-desktop-app) | Voice dictation Electron app — captures speech, transcribes via Groq Whisper, optionally reformats with Claude, injects text into any focused app | Electron · Groq Whisper · Claude Haiku/Sonnet · AudioWorklet · nut-js |
| [**Volini**](#-volini--real-time-automotive-voice-agent) | Real-time voice AI for cars — natural language questions about specs, pricing, safety ratings, answered via voice, end-to-end | LiveKit · Deepgram Nova-3 · Cerebras LLM · ChromaDB · Next.js |
| [**Pixel Agents**](#-pixel-agents--multi-agent-vs-code-extension) | Forked a VS Code Marketplace extension and shipped the OpenCode SSE integration — visual pixel-art dashboard tracking concurrent Claude Code + OpenCode agents | TypeScript · VS Code Extension API · Canvas 2D · SSE · React 19 |

### 📊 ML / Data Projects

| Project | What it does | Stack |
|---------|-------------|-------|
| [**Liquidity Forecasting Pipeline**](#-liquidity-forecasting-pipeline) | Real-time ELT pipeline for corporate treasury cash flow forecasting | GCP Pub/Sub · BigQuery · dbt · Python |
| [**Customer Churn Prediction**](#-customer-churn-prediction) | End-to-end ML pipeline with SHAP explainability for business stakeholders | Python · scikit-learn · SHAP · Random Forest |
| [**Radiology X-ray Classifier**](#-radiology-x-ray-classifier) | Multi-label chest X-ray classifier with Grad-CAM interpretability | PyTorch · ResNet · Grad-CAM · NIH Dataset |

---

## Project Deep-Dives

### 🎓 AutoCoach — RAG Learning Platform

> *AI-powered personalized learning: ingest documents → extract concepts → adaptive quizzes → Bayesian mastery tracking*

**The problem:** Students have no efficient way to turn dense study materials into active recall. AutoCoach automates the full study loop.

**Architecture:**
```
PDF/PPTX upload
    → LangChain chunking (1000 tokens / 200 overlap)
    → OpenAI batch embeddings → Qdrant vector store
    → Kimi K2.5 concept extraction (Pareto-corrected for score-inflation bias)
    → RAG-grounded quiz generation (document-scoped retrieval)
    → Three-tier answer evaluation: Kimi K2.5 → GPT-4o-mini → length heuristic
    → Bayesian-smoothed + EMA mastery update
    → Spaced repetition queue (resurface if mastery < 75% AND last tested > 2 days)
```

**Key decisions:**
- **Pareto post-processing on concept extraction** — fixed systematic LLM score-inflation bias; top 20% classified as "core" concepts driving the primary KPI
- **Bayesian mastery formula:** `smoothed = (correct+1)/(correct+tested+2)`, `new_score = 0.85*old + 0.15*smoothed` — prevents overconfident early mastery; caps at 95% until ≥5 tests AND ≥90% accuracy
- **Zero hard-failure paths** — three-tier fallback means no answer evaluation ever crashes
- **Optimistic locking** on XP/credit writes — 5-retry pattern for concurrent quota safety

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js_16-black?style=flat-square&logo=next.js&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?style=flat-square&logo=qdrant&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=black)
![Railway](https://img.shields.io/badge/Railway-0B0D0E?style=flat-square&logo=railway&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-black?style=flat-square&logo=vercel&logoColor=white)

---

### 🎙 Voxi — Voice Dictation Desktop App

> *Cross-platform Electron app: floating bubble → Groq Whisper → Claude formatting → keyboard injection into any app*

**The problem:** Voice-to-text tools are either cloud-dependent, context-unaware, or inject trash formatting. Voxi is local-first, model-composable, and clipboard-aware.

**Architecture:**
```
AudioWorklet PCM pipeline (16kHz mono, 250ms buffering)
    → Groq Whisper-Large-V3-Turbo (transcription)
    → Claude Haiku (optional formatting — cost-sensitive, high-frequency)
    → nut-js keyboard injection → focused app

"Hey Voxi" command mode:
    speech → intent extraction → clipboard read
    → Claude Sonnet with task-specific prompts (summarize / translate / debug / rewrite)
    → inject — zero UI interaction
```

**Key decisions:**
- **Multi-model cost split:** Claude Haiku for every dictation (fast, cheap), Claude Sonnet only for complex clipboard commands (slower, more capable) — deliberate latency/cost tradeoff
- **AES-256-CBC key storage** — API keys encrypted using machine-id-derived key, IV prepended in ciphertext
- **Three-tsconfig build system** — solved hard Electron ESM/CJS context boundary bug (renderer vs. preload vs. main)
- **SQLite correction learning loop** — stores raw→corrected pairs per app for automatic personalization without model fine-tuning
- Shipped as macOS universal DMG (arm64 + x64) + Windows NSIS installer

![Electron](https://img.shields.io/badge/Electron_29-47848F?style=flat-square&logo=electron&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React_18-61DAFB?style=flat-square&logo=react&logoColor=black)
![Groq](https://img.shields.io/badge/Groq_Whisper-F55036?style=flat-square&logo=groq&logoColor=white)
![Claude](https://img.shields.io/badge/Claude_Haiku%2FSonnet-D97757?style=flat-square&logo=anthropic&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)

---

### 🚗 Volini — Real-Time Automotive Voice Agent

> *Ask a car question out loud, get a spoken answer in real time — specs, pricing, safety ratings, comparisons*

**Architecture:**
```
LiveKit AgentSession
    → Deepgram Nova-3 STT (streaming, 25ms endpointing)
    → Cerebras Llama 3.1 8B (LLM)
    → Three-layer knowledge backend:
        (1) ChromaDB RAG + MiniLM-L6-v2 embeddings
        (2) SQLite cache (24h TTL + background preloading of top-10 cars)
        (3) asyncio.gather fan-out → NHTSA + fueleconomy.gov + CarQueryAPI + DuckDuckGo
    → Custom Smallest.ai TTS plugin (raw int16 PCM via AudioEmitter)
    → React Recharts latency HUD (STT + EOU + LLM TTFT + TTS, live)
```

**Key decisions:**
- **Three-stage entity resolution:** exact alias lookup (40+ entries: "miata"→Mazda MX-5) → fuzzy SequenceMatcher (≥0.55) → live NHTSA makes-list scan
- **Function-call bleed guard** — custom TTS wrapper drops text starting with `<function=` before synthesis (discovered through real LLM behavior in production)
- **Provider-swap abstraction** — iterated across 3 LLM providers (Groq → Cerebras → GPT) due to rate limits and tool-call format issues; clean interface kept swap cost minimal
- Per-turn latency instrumentation published over LiveKit data channel → live HUD

![Python](https://img.shields.io/badge/Python_3.12-3776AB?style=flat-square&logo=python&logoColor=white)
![LiveKit](https://img.shields.io/badge/LiveKit-E4354A?style=flat-square&logo=livekit&logoColor=white)
![Deepgram](https://img.shields.io/badge/Deepgram_Nova--3-101010?style=flat-square&logo=deepgram&logoColor=white)
![ChromaDB](https://img.shields.io/badge/ChromaDB-FF6B35?style=flat-square&logo=chroma&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js_15-black?style=flat-square&logo=next.js&logoColor=white)

---

### 🎮 Pixel Agents — Multi-Agent VS Code Extension

> *Forked from VS Code Marketplace · Shipped the OpenCode SSE integration · Visual pixel-art office tracking concurrent Claude Code + OpenCode agents*

**My contribution:** The original Pixel Agents extension visualizes Claude Code agents as pixel-art characters in an office world. I forked it and shipped the **OpenCode integration** — a new SSE tracking architecture that enables the extension to monitor OpenCode agents via HTTP event streaming alongside Claude Code's JSONL file-watching, unified behind a single message abstraction so the webview is completely framework-agnostic.

**The hard problem — multi-agent SSE routing:**
```
Problem: OpenCode runs multiple concurrent sessions. Naive "mostRecentAgentId" binding
misroutes events when N agents launch simultaneously.

Solution:
  1. Pre-launch snapshot: capture existing session IDs before terminal starts
  2. Poll /session endpoint until a NEW session ID appears → bind that ID to the terminal
  3. sessionIdToAgentId dispatch table routes all subsequent SSE events correctly
  → Works correctly with 10+ simultaneous agents
```

**Other technical highlights:**
- **Triple-layer file watching** (OS `fs.watch` + `fs.watchFile` stat polling + manual `setInterval`) with partial-line buffering — prevents JSON parse errors mid-write on append-only JSONL
- **Canvas 2D game engine outside React** — `OfficeState` imperative class holds all game state, 60fps `requestAnimationFrame` loop runs without React reconciler diffs — smooth at 10+ agents in a sandboxed WebviewViewProvider
- **Inference-based state detection** — dual idle signals: `system/turn_duration` records + 5s silence timer for text-only turns; permission bubble inferred via 7s non-progress timer
- **Claude Vision in asset pipeline** — `@anthropic-ai/sdk` auto-generates sprite metadata from purchased tileset PNGs, feeding a 7-stage import pipeline to `furniture-catalog.json`

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![VS Code](https://img.shields.io/badge/VS_Code_Extension_API-007ACC?style=flat-square&logo=visual-studio-code&logoColor=white)
![React](https://img.shields.io/badge/React_19-61DAFB?style=flat-square&logo=react&logoColor=black)
![Canvas](https://img.shields.io/badge/Canvas_2D-FF6B6B?style=flat-square&logo=html5&logoColor=white)
![Claude](https://img.shields.io/badge/Claude_Vision-D97757?style=flat-square&logo=anthropic&logoColor=white)

---

### 📈 Liquidity Forecasting Pipeline

> *Real-time ELT: GCP Pub/Sub → BigQuery → dbt staging/intermediate/mart → liquidity runway analytics*

Designed a streaming ingestion pipeline simulating real-time corporate treasury cash flows (receivables/payables). Built a multi-layer dbt warehouse in BigQuery with projected balance and liquidity runway analytics — automated end-to-end with no manual refresh steps.

![GCP](https://img.shields.io/badge/GCP-4285F4?style=flat-square&logo=google-cloud&logoColor=white)
![BigQuery](https://img.shields.io/badge/BigQuery-669DF6?style=flat-square&logo=google-bigquery&logoColor=white)
![dbt](https://img.shields.io/badge/dbt-FF694B?style=flat-square&logo=dbt&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)

---

### 📉 Customer Churn Prediction

> *End-to-end ML pipeline with SHAP explainability — black-box to business insight*

Logistic Regression + Random Forest with GridSearchCV tuning, class-imbalance correction, and a SHAP pipeline that surfaces top churn drivers for non-technical stakeholders. The explainability layer was the whole point.

![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![SHAP](https://img.shields.io/badge/SHAP-FF6B6B?style=flat-square)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)

---

### 🫁 Radiology X-ray Classifier

> *Multi-label chest pathology classification on NIH dataset with Grad-CAM visual interpretability*

ResNet CNN predicting Atelectasis, Effusion, and Infiltration simultaneously from NIH Chest X-rays. Full evaluation pipeline (Precision/Recall/F1/ROC-AUC per condition) with Grad-CAM to highlight regions influencing predictions — interpretability matters when the stakes are clinical.

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![ResNet](https://img.shields.io/badge/ResNet_CNN-FF6B35?style=flat-square)
![Grad-CAM](https://img.shields.io/badge/Grad--CAM-7C3AED?style=flat-square)

---

## Tech Stack

### AI / LLM / Agents
![RAG](https://img.shields.io/badge/RAG_Pipelines-DC244C?style=flat-square)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white)
![Claude](https://img.shields.io/badge/Anthropic_Claude-D97757?style=flat-square&logo=anthropic&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI_GPT--4o-412991?style=flat-square&logo=openai&logoColor=white)
![Groq](https://img.shields.io/badge/Groq_Whisper-F55036?style=flat-square)
![LiveKit](https://img.shields.io/badge/LiveKit_Agents_SDK-E4354A?style=flat-square)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?style=flat-square)
![ChromaDB](https://img.shields.io/badge/ChromaDB-FF6B35?style=flat-square)
![Deepgram](https://img.shields.io/badge/Deepgram-101010?style=flat-square)
![Cerebras](https://img.shields.io/badge/Cerebras-0052CC?style=flat-square)

### Languages & Frameworks
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-black?style=flat-square&logo=next.js&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![Electron](https://img.shields.io/badge/Electron-47848F?style=flat-square&logo=electron&logoColor=white)

### Data / Cloud
![GCP](https://img.shields.io/badge/GCP-4285F4?style=flat-square&logo=google-cloud&logoColor=white)
![BigQuery](https://img.shields.io/badge/BigQuery-669DF6?style=flat-square&logo=google-bigquery&logoColor=white)
![dbt](https://img.shields.io/badge/dbt-FF694B?style=flat-square&logo=dbt&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=black)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Railway](https://img.shields.io/badge/Railway-0B0D0E?style=flat-square&logo=railway&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-black?style=flat-square&logo=vercel&logoColor=white)

### AI Coding Tools (daily)
![Claude Code](https://img.shields.io/badge/Claude_Code-D97757?style=flat-square&logo=anthropic&logoColor=white)
![Codex](https://img.shields.io/badge/OpenAI_Codex-412991?style=flat-square&logo=openai&logoColor=white)
![OpenCode](https://img.shields.io/badge/OpenCode-101010?style=flat-square)
![Cursor](https://img.shields.io/badge/Cursor-000000?style=flat-square)

---

## Experience

<details>
<summary><b>The Best Notary — Data Analyst / Project Manager</b> · UC Davis MSBA Practicum · San Francisco, CA · Aug 2024 – Jul 2025</summary>

Built real-time analytics infrastructure and AI-powered automation for a notary marketplace. Led a cross-functional MSBA team working directly with business stakeholders.

- Built live **Airtable → Looker pipeline** — eliminated manual reporting, reduced decision latency from static snapshots to near-real-time
- Deployed **Slack AI chatbot** (Make + GPT) that queries notary data dynamically — **reduced manual task handling by ~30%**
- Engineered **Unified Notary Dataset** across multiple U.S. states via web scraping + structured ingestion — enabled market expansion and hiring strategy analysis
- Optimized notary-client **matching logic** (SQL + Python) — **improved matching accuracy by ~15%**
- Built data enrichment pipelines for law firm intelligence using Chromium automation (Yelp, Google Reviews)

</details>

<details>
<summary><b>Wipro — Cybersecurity Analyst / Project Engineer</b> · Pune, India · Jul 2021 – Jul 2024</summary>

Managed enterprise identity and access systems (IAM) for 2,000+ users. Promoted from Project Engineer to Cybersecurity Analyst.

- Automated user lifecycle management via **PowerShell** (provisioning, deprovisioning, GPO) — **reduced manual effort by ~33%**
- Deployed and optimized **CyberArk PAM** — **improved secure access workflows by ~20%**
- Managed **Active Directory for 2,000+ users** across multiple environments
- Implemented **Azure AD SSO + MFA** — achieved **100% client retention**
- Monitored infrastructure via LogicMonitor — maintained **>95% uptime**

</details>

---

## Education

🎓 **M.S. Business Analytics** — University of California, Davis · 2023–2025
🎓 **B.E. Computer Science** — India · 2017–2021

---

<div align="center">

[![GitHub Stats](https://github-readme-stats.vercel.app/api?username=abhaypadmanabhan&show_icons=true&theme=tokyonight&hide_border=true&count_private=true)](https://github.com/abhaypadmanabhan)
[![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=abhaypadmanabhan&layout=compact&theme=tokyonight&hide_border=true)](https://github.com/abhaypadmanabhan)

---

*Currently open to AI Engineer and Full-Stack AI roles in San Francisco.*
*F1 OPT → STEM OPT → H1B · E-Verify / sponsored companies preferred.*

[![Portfolio](https://img.shields.io/badge/abhaypadmanabhan.com-7C3AED?style=for-the-badge&logo=google-chrome&logoColor=white)](https://abhaypadmanabhan.com)
[![LinkedIn](https://img.shields.io/badge/Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/apadmanabhan98)

</div>
