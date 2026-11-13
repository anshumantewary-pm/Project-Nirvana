# NIRVANA – Product Requirements Document

*Nov 12, 2026 · Owner: Anshuman Tewary*

## 1. Overview

NIRVANA is an AI-powered Lean Six Sigma engine that finds energy, cooling and water waste in AI data centres, and turns the power it frees into capacity for more GPUs.

**One-line pitch:** Every GPU stays. We remove the waste around them, so the same grid connection runs more compute for less electricity and water.

**Objective:** apply Lean Six Sigma (DMAIC) with AI to:

- Reduce heat waste and overcooling, so cooling energy matches what the equipment actually needs
- Improve cooling efficiency (more cooling delivered per kWh)
- Increase useful electricity: a larger share of every kWh reaches the GPUs instead of overhead
- Reduce water consumed per unit of compute
- Prove every saving with a weather- and workload-adjusted before/after comparison

**Context:** Google Startup Buildathon India 2026, on Google's AI and cloud stack.

### Team and ownership

| Person | Role | Owns | Key deliverables |
|---|---|---|---|
| Anshuman Tewary | Founder · Product and Lean Six Sigma | Product vision, waste-pattern library, DMAIC methodology, business model, pitch | PRD, waste rules spec, DMAIC charter template, pitch deck |
| Pinak Kundu | Co-founder · Engineering and AI | Data pipeline, analysis engine, calculator, Gemini integration, deployment | BigQuery schema, Cloud Run services, Gemini prompts, web app |
| Navjot Singh | Co-founder · Go-to-market and Operations | Customer discovery, pilot partner, pricing, demo data | 5 operator interviews, 1 pilot letter of interest, unit economics |

Pinak's and Navjot's areas are proposed, still to be confirmed. Area owners decide within their area; scope, pricing and pitch are agreed by all three.

## 2. Problem statement

AI data centres are limited by power, not demand: the grid connection is fixed, and a large share of it goes to cooling and overhead instead of GPUs.

- **Power is the bottleneck.** A new or larger grid connection can take years.
- **Cooling eats capacity.** Overcooling, hot/cold air mixing and degraded chillers, fans and pumps draw power that could run compute.
- **Water use is rising.** Evaporative cooling consumes large volumes, often in water-stressed regions.
- **Tools stop at dashboards.** Monitoring shows consumption is high, not why, what to fix first, what it's worth, or whether it worked.

**Design assumption:** every GPU is required. NIRVANA never recommends switching off needed compute.

### The Lean wastes in a data centre

| Lean waste | How it shows up | Example signal |
|---|---|---|
| Overprocessing | Cooling more than equipment needs | Rack inlet far below ASHRAE recommended range |
| Motion | Hot exhaust mixing with cold supply air | Small ΔT across racks, low return temperature |
| Defects | Chillers, filters, fans, pumps degrading | Cooling kW per IT kW rising at same weather and load |
| Inventory | Stranded capacity: power reserved, never used | Large gap between contracted and actual peak kW |
| Waiting | GPUs idle but drawing power | High power, low utilisation |
| Overproduction | Water lost beyond cooling need | Water per kWh of cooling above baseline |
| Non-utilised talent | Fixes known but never run as projects | Repeat alerts with no closed action |

### Researched problem statements

| # | Problem | Evidence | NIRVANA response |
|---|---|---|---|
| P1 | Efficiency has stalled | Global avg PUE ~1.54, flat for six years (Uptime 2025; Mgrid) | Continuous DMAIC cycles, not one-off audits |
| P2 | Colo/enterprise lag hyperscalers | PUE 1.58–1.80 vs 1.10–1.15 (Mgrid) | Benchmark vs peers; target the gap |
| P3 | Cooling uses a third+ of electricity | 30–40% (WEF via DCD) | Overcooling, air-mixing, free-cooling, degradation detectors |
| P4 | Stranded capacity | Tenants use <60% of leased capacity (Bisnow) | Actual peak vs contracted kW per rack/tenant |
| P5 | UPS capacity stranded | 10%+ UPS capacity lost (Uptime via The Register) | Electrical-chain view: load, losses, headroom |
| P6 | Grid connections scarce | Over-reservation blocks others (The Register) | Freed kW → extra GPUs within existing connection |
| P7 | Conversion losses | Transformer/UPS/PDU losses 8–12% (Mgrid) | Loss per stage; flag UPS in inefficient load bands |
| P8 | GPUs idle while drawing power | 85% of low-util issues fixable with small code changes (ICSE 2024) | Idle-GPU detector (pilot phase) |
| P9 | Water use rising in stressed cities | India DCs ~150 bn L (2024-25) → ~358 bn L by 2030 (CEEW via AIM) | WUE tracking, water-overuse detector, energy–water trade-off |
| P10 | India growth constrained by power and water | Economic Survey 2025-26 (Business Today) | Capacity-from-efficiency for Indian operators |
| P11 | Quiet equipment degradation | Coolant degradation raised pump energy and PUE (DCD) | Cooling kW/IT kW drift at same conditions |
| P12 | Waste heat thrown away | Mostly released unused (DCD) | Heat-recovery screen (later phase) |

**Problems that shape the design**

- **Measurement gap:** metric collection didn't improve in 2025 → NIRVANA must work with messy, partial data.
- **Skills gap:** ~2/3 of operators struggle to hire → package expert diagnosis for small teams.
- **Trust limits for AI:** operators accept AI for analysis, not equipment control → NIRVANA recommends and verifies; humans approve.

## 3. Goals, non-goals, success metrics

**Goals**

1. Detect energy, cooling and water waste from facility data automatically.
2. Explain the root cause of each waste with evidence, not guesses.
3. Rank fixes by money saved, power freed, water saved, cost and risk.
4. Generate a ready-to-run DMAIC improvement project for each fix.
5. Verify savings after implementation and watch for drift.

**Non-goals**

- Switching off or throttling GPUs that workloads need
- Direct automatic control of cooling equipment (MVP and pilot stay human-approved)
- Replacing DCIM/BMS; NIRVANA reads from it
- Building power generation (on-site solar is an optional later recommendation only)

**Success metrics (pilot targets, to be validated)**

| Metric | Measures | Target |
|---|---|---|
| PUE | Facility energy ÷ IT energy | −0.1 or better |
| Freed capacity | IT kW available within same grid limit | Report kW and equivalent GPUs |
| Cooling efficiency | Cooling kW per IT kW | −10% |
| WUE | Litres per kWh IT | −10% |
| Verified savings | ₹/yr, weather- and load-adjusted | ≥3× customer fee |
| Time to insight | Upload → first ranked fix | <5 min |
| Action rate | Fixes approved by operators | ≥50% |

## 4. Target users

First customers: mid-size facilities without in-house energy engineers (regional colos, enterprise server rooms, companies running their own GPU clusters). Hyperscalers are out of scope.

| Persona | Role | Needs |
|---|---|---|
| Facility manager | Runs cooling, power, maintenance | Ranked fixes, evidence, work plan |
| Energy / sustainability lead | Owns PUE, WUE, carbon | Verified, audit-ready savings |
| CFO / business head | Approves spend | ₹ saved, payback, NPV, risk |
| Capacity / infra planner | Decides GPU count | kW freed, GPUs that fit |
| CI lead | Runs Lean Six Sigma | DMAIC charters, control plans, tracker |

## 5. Core user flow (DMAIC)

1. **Define:** facility profile (grid kW, GPU count, kW/GPU, cooling type, tariff ₹/kWh, water cost, city).
2. **Measure:** upload CSV or connect a source; clean, add weather, build baseline.
3. **Analyse:** rules flag gaps vs baseline; Gemini explains root cause (5 Whys, fishbone), cites data, lists missing evidence.
4. **Improve:** rank fixes by ₹, kW, water, cost, payback, risk; generate DMAIC charter.
5. **Approve:** engineer reviews, operator approves → work order.
6. **Control:** verified savings vs adjusted baseline; control charts; drift starts a new cycle.

## 6. User stories

| ID | As a… | I want to… | Acceptance criteria | Pri |
|---|---|---|---|---|
| US-1 | Facility manager | Create a facility profile in <2 min | All profile fields saved and validated | P0 |
| US-2 | Facility manager | Upload meter CSV | ≤50 MB; flags missing columns, wrong units, gaps >2 h; shows row count and date range | P0 |
| US-3 | Facility manager | See PUE, WUE, cooling kW/IT kW over time | Hourly/daily/monthly; matches hand calc on sample | P0 |
| US-4 | Facility manager | See wastes ranked by ₹ lost/yr | Pattern, window, kWh, ₹, confidence; sorted by ₹ | P0 |
| US-5 | Engineer | Read why a waste happened | ≥2 cited data points, missing evidence, one field test | P0 |
| US-6 | Capacity planner | See extra GPUs possible | kW freed and GPUs via formula; recalculates on input change | P0 |
| US-7 | CFO | See cost, payback, NPV per fix | CAPEX range, annual savings, payback months, 5-yr NPV at 12% | P0 |
| US-8 | CI lead | Download DMAIC charter PDF | All charter fields + control plan; <10 s | P0 |
| US-9 | Engineer | Approve/reject a fix with reason | Logged with user, time, reason; rejected leave active list | P1 |
| US-10 | Energy lead | See verified savings | Adjusted baseline − actual, with confidence range, ≥4 weeks | P1 |
| US-11 | Facility manager | Get drift alerts | Alert when metric breaks control limit 3 days running | P1 |
| US-12 | Operator | Ask in Hindi or English | Answers cite the facility's own numbers | P1 |

## 7. Functional requirements

| ID | Module | Requirement | Pri |
|---|---|---|---|
| FR-1 | Facility profile | Capture grid limit, GPUs, kW/GPU, cooling type, tariff, water cost, city | P0 |
| FR-2 | Data intake | CSV upload of facility/IT/cooling kW and water; validate units and gaps | P0 |
| FR-3 | Data intake | Hourly weather for the site's city | P0 |
| FR-4 | Data intake | Live BMS/DCIM connectors (BACnet, Modbus, SNMP, vendor APIs) | P1 |
| FR-5 | Data intake | GPU telemetry (utilisation, power) | P1 |
| FR-6 | Baseline | Regression of energy and water vs outdoor temp and IT load | P0 |
| FR-7 | Metrics | PUE, WUE, cooling kW/IT kW, headroom per hour and period | P0 |
| FR-8 | Waste detection | Run waste-pattern rules; size, window, confidence | P0 |
| FR-9 | Waste detection | SPC charts and change-point detection | P1 |
| FR-10 | Root cause | Gemini ranked hypotheses with cited evidence, 5 Whys, fishbone | P0 |
| FR-11 | Root cause | Missing evidence and a field test per hypothesis | P0 |
| FR-12 | Fix library | Map causes to standard fixes with cost ranges | P0 |
| FR-13 | Calculator | kW freed, extra GPUs, ₹, litres, CO₂, payback, NPV | P0 |
| FR-14 | Prioritisation | Rank by NPV, payback, FMEA risk, effort | P0 |
| FR-15 | DMAIC generator | Charter: problem, baseline, cause, fix, cost, savings, owner, risk, control plan | P0 |
| FR-16 | Approval | Engineer review + operator approval before work order | P1 |
| FR-17 | Verification | Post-fix vs adjusted baseline; verified savings | P1 |
| FR-18 | Control | Monitor; drift alerts; reopen project | P1 |
| FR-19 | Reporting | PDF export of charters and savings reports | P0 |
| FR-20 | Knowledge base | Store closed-project results to improve estimates | P2 |

## 8. Data requirements

**Input CSV (hourly)**

| Column | Unit | Required | Notes |
|---|---|---|---|
| timestamp | ISO 8601 local | Yes | One row/hour |
| facility_kw | kW | Yes | Main meter |
| it_kw | kW | Yes | UPS output or PDUs |
| cooling_kw | kW | Recommended | Chillers, CRAH/CRAC, pumps, towers |
| water_litres | L/h | Recommended | Cooling-tower make-up |
| supply_air_c / return_air_c | °C | Recommended | For overcooling and mixing rules |
| chw_supply_c / chw_return_c | °C | Optional | Chilled-water loop |
| gpu_util_pct / gpu_kw | % / kW | Optional | Idle-GPU rule |

**Data quality rules:** reject it_kw > facility_kw or negative power; flag PUE outside 1.0–3.0 as meter error; convert kWh-per-interval to average kW; interpolate gaps <2 h only; report a 0–100 data quality score per analysis.

## 9. Waste pattern library

| Pattern | Detection rule | Likely fixes | MVP |
|---|---|---|---|
| Overcooling | Supply/inlet well below ASHRAE range while cooling kW high | Raise setpoints in steps; tune fans | ✅ |
| Hot/cold air mixing | Low ΔT across racks; return ≈ supply | Containment, blanking panels, seal gaps | ✅ |
| Chiller / cooling degradation | Cooling kW/IT kW rising at same weather and load | Clean coils/filters; refrigerant; resequence | ✅ |
| Missed free cooling | Mechanical cooling when economiser possible | Adjust changeover settings | ✅ |
| Water overuse | Water per kWh cooling above baseline; off-hours use | Cycles of concentration; fix leaks; dry cooling | ✅ |
| Fan/pump inefficiency | Power not following cube law | VSDs; reset pressure setpoints | — |
| Idle GPU power | High GPU power, low utilisation | Fill idle GPUs; power-cap | — |
| Stranded capacity | Peak IT kW far below contract | Re-plan rack power budgets | — |
| Waste heat loss | High return water temp near heat demand | Heat recovery study | — |

High energy use isn't waste by default: rules always compare against the baseline for that hour's weather and IT load.

## 10. Key calculations

- **PUE** = total facility energy ÷ IT energy; **WUE** = site water (L) ÷ IT energy (kWh)
- **IT capacity** = grid limit ÷ PUE; **Extra GPUs** = (IT cap after − IT cap before) ÷ kW per GPU
  - *Example (illustrative):* 1,500 kW at PUE 1.8 → 833 kW IT; at PUE 1.4 → 1,071 kW. At 1.3 kW/GPU ≈ 180 more GPUs.
- **Annual savings** = ΔkWh × tariff + Δwater × water cost − added running cost
- **Payback** = CAPEX ÷ annual savings; **NPV** = Σ savingsₜ/(1+r)ᵗ − CAPEX (r = 12%, n = 5)
- **Verified savings** = baseline predicted at actual weather and load − actual measured use (standard M&V)

## 11. Non-functional requirements

- **Human in control:** no direct equipment control in MVP or pilot.
- **Safe limits:** never breach ASHRAE, equipment specs, redundancy (N+1, 2N) or SLAs.
- **Explainability:** every finding shows data, confidence, missing evidence; numbers from code, not the LLM.
- **Accuracy:** per-site baseline error reported; savings shown as ranges.
- **Privacy:** customer data stays in their GCP project and region; encrypted; never used for training.
- **Access control:** viewer, engineer, approver, admin; audit log.
- **Performance:** one year of hourly data for one site analysed in <5 min.
- **Availability:** 99.5% for the pilot.
- **Usability:** a facility manager with no data-science skills completes the flow unaided.

## 12. Buildathon MVP scope

**In scope:** FR-1–3, FR-6–8, FR-10–15, FR-19; five waste patterns; one demo dataset; simulated before/after for Control.
**Out of scope:** live BMS connectors, GPU telemetry, approval workflow, multi-site, accounts beyond demo login.
**Demo data:** public data centre operational dataset from the National Laboratory of the Rockies (formerly NREL), plus an offline sample CSV.

**Day 1:** lock dataset and schema · profile form and CSV → BigQuery · metrics and weather-adjusted baseline · five rules · pitch draft
**Day 2:** Gemini root cause · calculator · DMAIC charter + PDF · results screen · deploy on Cloud Run + Firebase; rehearse twice

**Go/no-go:** sample CSV analysed <60 s · ≥3 findings with ₹ matching a spreadsheet check · valid Gemini JSON for every finding (cached fallback) · extra-GPU figure recalculates on grid-limit change · PDF downloads · live public URL + backup recording · every deck figure sourced or labelled illustrative

## 13. Business model and GTM

| Stage | Offer | Price (to validate) |
|---|---|---|
| Entry | Free 2-week waste audit from uploaded data | Free |
| Core | Shared-savings contract | 20–30% of verified annual savings |
| Scale | Continuous monitoring + DMAIC project management | Monthly per facility |

**Unit economics (illustrative):** 1 MW IT at PUE 1.8 and ₹8/kWh ≈ ₹12.6 cr/yr electricity. PUE 1.6 saves ≈ ₹1.4 cr/yr; a 25% share ≈ ₹35 lakh per facility per year.

**GTM:** one friendly pilot for a verified case study → mid-size colos and enterprise IT in Mumbai, Chennai, Hyderabad, Bengaluru, Noida → partner with energy auditors and FM firms.

**Why we win:** continuous, vendor-neutral, priced on results, and it proves its savings.

## 14. Risks and roadmap

| Risk | Mitigation |
|---|---|
| No access to facility data | CSV exports; free audit as hook; public dataset for demos |
| Operators distrust AI | Cited evidence; numbers from code; human approval |
| Recommendation risks uptime | Hard ASHRAE/redundancy/SLA limits; small reversible steps |
| Savings estimates wrong | Adjusted baseline; ranges with confidence |
| Incumbents (Schneider, Vertiv, EkkoSense) | Vendor-neutral, results-priced, Lean Six Sigma workflow, mid-market |
| Energy–water trade-offs | Every recommendation shows both impacts |

**Roadmap:** Buildathon (Nov–Dec 2026) MVP → Pilot (Q1 2027) one live site, BMS connector → Product (Q2–Q3 2027) approvals, SPC, GPU telemetry, 3–5 paying sites → Scale (Q4 2027+) portfolio view, partners, heat-recovery and water modules.

## Sources

- Uptime Institute Global Data Center Survey 2025
- Mgrid: Global data center PUE stalls at 1.54
- DCD: Why data center cooling is now business critical
- Bisnow: Data center tenants wasting power capacity
- The Register: Datacenters hoarding grid power, says Uptime Institute
- ICSE 2024: An Empirical Study on Low GPU Utilization of Deep Learning Jobs
- AIM: India's AI boom could double data centre water consumption by 2030 (CEEW)
- Business Today: Economic Survey flags power, water, GPU bottlenecks
