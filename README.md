# NIRVANA

**Neural Intelligence for Resource, Infrastructure, Velocity and Networked Asset Optimisation**

An AI-powered Lean Six Sigma engine that finds energy, cooling and water waste in AI data centres, and turns the power it frees into capacity for more GPUs.

> **Every GPU stays.** We remove the waste around them, so the same grid connection runs more compute for less electricity and water.

Built for **Google Startup Buildathon India 2026** on Google's AI and cloud stack.

---

## The problem

AI data centres are limited by power, not demand. The grid connection is fixed, and 30–40% of it goes to cooling and overhead instead of GPUs. Global average PUE has been stuck at about 1.54 for six years, and colocation and enterprise sites sit at 1.58–1.80. Monitoring tools show that consumption is high. They don't say why, what to fix first, what the fix is worth, or whether it worked.

## What NIRVANA does

It runs one DMAIC cycle per waste it finds:

| Step | What happens |
|---|---|
| **Define** | Facility profile: grid limit, GPUs, cooling type, tariff, water cost, city |
| **Measure** | Upload hourly meter/BMS CSV → cleaned, weather-joined, baselined in BigQuery |
| **Analyse** | Rules flag named waste patterns; Gemini explains root cause with cited evidence (5 Whys, fishbone) |
| **Improve** | Fixes ranked by ₹ saved, kW freed, water saved, payback, NPV, risk; DMAIC charter generated |
| **Approve** | Engineer reviews, operator approves. Nothing changes on site without sign-off |
| **Control** | Verified savings vs a weather- and load-adjusted baseline; drift alerts reopen the cycle |

**The demo moment:** upload a file and within minutes see *"You can add N GPUs without new power. Here are the top 3 fixes, their cost and the project plan."*

## Core principle

**Rules find the waste, Gemini explains it, people approve it.** Every number (kW, GPUs, ₹, litres, NPV) is computed in Python. Gemini only reasons over computed facts and returns schema-validated JSON.

## Tech stack

| Component | Google service |
|---|---|
| Web app and forms | Firebase Hosting + Firebase Auth |
| File storage | Cloud Storage |
| Data warehouse | BigQuery |
| Analysis engine and calculator | Cloud Run (Python) |
| Root-cause agent | Gemini via Vertex AI |
| Prompt prototyping | Google AI Studio |

## Docs

- [docs/PRD.md](docs/PRD.md): product requirements (problem, users, user stories, functional requirements, metrics, business model)
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md): system architecture (layers, data flow, services, data model, API, Gemini design, security)

## Planned repo layout

```
Project-Nirvana/
├── docs/                 PRD and architecture
├── web/                  Firebase front end (5 screens)
├── api/                  Cloud Run service: REST endpoints
├── engine/               Python: cleaning, baseline, metrics, waste rules, calculator
│   ├── rules/            one module per waste pattern
│   └── calculator/       capacity, money, water, NPV
├── agent/                Gemini prompts, response schema, guardrails
├── infra/                BigQuery DDL, Terraform / deploy scripts
└── data/sample/          demo CSV (NLR public data centre dataset)
```

## Status

Pre-build. MVP target: one full DMAIC loop on public data for the buildathon (Nov–Dec 2026).

## Team

| Person | Role | Owns |
|---|---|---|
| Anshuman Tewary | Founder · Product & Lean Six Sigma | Vision, waste-pattern library, DMAIC method, business model, pitch |
| Pinak Kundu | Co-founder · Engineering & AI *(proposed)* | Data pipeline, analysis engine, calculator, Gemini, deployment |
| Navjot Singh | Co-founder · GTM & Operations *(proposed)* | Customer discovery, pilot partner, pricing, demo data |
