# ⚡ Strategic Investment Planner: Germany 2030
### EV Market Analysis & Charging Infrastructure Dashboard

<img width="1920" height="1080" alt="Screenshot 2026-05-17 at 15 27 01" src="https://github.com/user-attachments/assets/b2a2f081-730b-42c8-853f-12d6910f9544" />


> **An interactive Tableau dashboard built as a client-facing case study for a consulting and engineering firm specializing in automotive and energy. Designed to convince a Charge Point Operator (CPO) to invest in a data-driven strategic planning tool by demonstrating the full analytical chain: EV adoption → charging demand → infrastructure gap → revenue opportunity.

---

## 🎬 Live Demo (in process....)

**in process.....**▶️ **[Watch the Full Dashboard Demo on Loom / YouTube](#)
*See the sliders in action — market scenarios updating Revenue Gap and AFIR Compliance Index in real time.*

![Dashboard Demo](assets/dashboard-demo.gif)
*Moving the Market Adoption Speed slider from Stagnation (0.5×) → Boom (1.5×) — Revenue Opportunity Gap updates instantly.*

---

## 📌 Project Overview

This project was developed in response to a real-world analytics case study assignment from consulting and engineering firm specializing in automotive and energy. The challenge: use publicly available data to analyze any region's EV market and present findings in an **interactive digital tool** suitable for a live 30-minute client demo.

The final deliverable — the **Strategic Investment Planner: Germany 2030** — is a Tableau dashboard that allows a CPO to dynamically stress-test market scenarios, understand their infrastructure gap, and quantify their revenue opportunity in the German public charging market through 2030.

**The core analytical question the dashboard answers:**

> *Given the projected growth of the German EV fleet, how large is the gap between national charging demand and current public supply capacity — and what is that gap worth in euros to a CPO?*

---

## 📸 Dashboard Screenshots (in process....)

| Market Overview & EV Adoption | Charging Demand Engine |
|---|---|
| ![Overview](assets/screenshot-overview.png) | ![Demand](assets/screenshot-demand.png) |

| Infrastructure Gap Analysis | Revenue Opportunity & AFIR Compliance |
|---|---|
| ![Gap](assets/screenshot-gap.png) | ![Revenue](assets/screenshot-revenue.png) |

---

## 🗂️ Repository Structure (in process....)

```
├── CaseStudy.pptx                 # Presentation deck (methodology, assumptions, findings)
├── assets/
│   ├── dashboard-demo.gif           # Animated GIF showing slider interactivity
│   ├── screenshot-overview.png      # Market overview & EV adoption slide
│   ├── screenshot-demand.png        # Charging demand engine slide
│   ├── screenshot-gap.png           # Infrastructure gap analysis slide
│   └── screenshot-revenue.png       # Revenue opportunity & AFIR compliance slide
└── README.md                        # This file
```

> 🔒 **The interactive Tableau dashboard (`.twb`) and master dataset (`.xlsx`) are available upon request.**
> Reach out via [LinkedIn](#) or [email](#) — happy to walk through the tool live.

---

## 📊 Dashboard Architecture & Analytical Narrative

The dashboard follows a strict top-down analytical flow, designed to guide a CPO client from market context to investment decision:

```
1. Market Snapshot  →  2. EV Adoption Trends  →  3. Charging Demand Engine
        ↓
4. Supply Capacity Model  →  5. Gap Analysis  →  6. Revenue Opportunity & AFIR Compliance
```

---

## 🔧 The Computational Engine: Formula Deep-Dive

Every KPI in the dashboard derives from one of two master calculations: **demand** and **supply**.

---

### 📐 Formula 1: National Energy Demand (GWh)

```
C_National_Fleet_Demand_GWh =
    Fleet_Size × (14,000 / 100 × 19) × 1.12 / 1,000,000
```

| Constant | Value | Source & Reasoning |
|---|---|---|
| **Annual Mileage** | 14,000 km/yr | German Federal average for passenger cars (Kraftfahrt-Bundesamt) |
| **Energy Consumption** | 19 kWh / 100 km | Technical benchmark for a mid-range BEV (e.g., VW ID.3 / Tesla Model 3), including real-world climate control overhead |
| **÷ 100** | Normalization | Converts the rate from "per 100 km" to "per km" for multiplication with annual mileage |
| **× 1.12 (Charging Loss)** | +12% overhead | Energy lost as heat during AC-to-DC conversion inside the car's onboard charger. The CPO must deliver 12% more from the grid than what enters the battery — this is **grid-side demand**, not battery-side |
| **÷ 1,000,000** | Unit conversion | kWh → GWh for macro-market reporting |

---

### 📐 Formula 2: Supply Capacity (GWh)

```
C_Supply_Capacity_GWh =
    ((AC_Points  × 11 kW  × 8,760 hrs × AC_Efficiency)
  + (DC_Points  × 150 kW × 8,760 hrs × DC_Efficiency))
  / 1,000,000
```

| Constant | Value | Reasoning |
|---|---|---|
| **8,760** | Hours/year | 24 hrs × 365 days. Converts instantaneous power (kW) into theoretical maximum annual energy (kWh) — the ceiling if a charger ran at full power 100% of the time |
| **11 kW** | AC power rating | Standard Type 2 three-phase AC charging, typical for destination charging (workplace, retail) |
| **150 kW** | DC/HPC power rating | Conservative baseline for High Power Charging. Modern HPC reaches 300–350 kW, but 150 kW is used as a cautious 2030 planning minimum |
| **AC/DC Efficiency** | See sliders | Utilization multiplier — scales theoretical maximum down to actual operational output |

---

### 📐 Formula 3: Weighted Average Power (~50 kW Proxy)

For the infrastructure gap calculation (`C_Required_Total_Plugs`), the model uses a weighted average across the 80/20 AC-to-DC split:

```
Weighted Power = (11 kW × 0.80) + (200 kW × 0.20)
               = 8.8 + 40
               = 48.8 kW  ≈  50 kW  (rounded for planning)
```

**Why 80/20?** The current German public charging network (Bundesnetzagentur registry) is predominantly slow AC — destination chargers at parking lots, offices, and retail. The 80/20 split reflects both current deployment data and the AFIR Masterplan Ladeinfrastruktur II roadmap targets.

---

## 🎛️ The Scenario Controls: Slider Logic & Validation

All four sliders share a critical design principle called **Validated Logic**:

```tableau
IF MIN([Year]) <= 2025
THEN [Empirical Historical Baseline]
ELSE [Parameter / Slider Value]
END
```

Historical data (2017–2025) is **locked** to empirical, source-verified values. Sliders only activate for 2026–2030 projections. This ensures the dashboard cannot rewrite history and remains fully defensible in a client presentation — if a slider is moved and the 2023 numbers don't change, that is intentional and correct.

---

### 🎚️ Slider 1: Urban Business Intensity (Parameter 1)

**Controls:** `C_Addressable_CPO_Market_GWh` — the share of total national EV energy demand flowing through the **public** CPO network vs. private home charging.

| Setting | Value | Scenario Meaning |
|---|---|---|
| **Minimum** | 10% (0.10) | Highly rural market. Nearly all EV owners have private driveways with home wallboxes. CPOs capture very little demand. |
| **Historical Baseline** | 30% (0.30) | Validated for 2017–2025. Early adopters were predominantly homeowners charging privately. 70% of demand stayed off the public grid. |
| **Maximum** | 80% (0.80) | Dense urban extreme. Apartment dwellers in Berlin / Munich / Hamburg have no private parking — nearly all their demand flows to public CPOs. |
| **Step Size** | 0.05 (5%) | A 5% national shift represents hundreds of thousands of vehicles changing behavior — a realistic policy or demographic increment. |

**The strategic trigger:** As Germany scales toward 15M EVs, adoption shifts to urban apartment dwellers without private parking. Every 5% move toward 80% represents a structurally larger revenue capture opportunity for CPOs deployed in urban locations.

---

### 🎚️ Slider 2: AC Asset Efficiency (Parameter 2)

**Controls:** Utilization multiplier on all AC charging points in `C_Supply_Capacity_GWh`.

**Conceptual framing:** Out of 8,760 total hours per year, how many hours is an AC plug **actively dispensing energy**?

| Setting | Value | Real-World Meaning |
|---|---|---|
| **Minimum** | 5% (0.05) | ~1.2 hrs/day. A "dead" charger — poor location, barely 1 car served per day. |
| **Historical Baseline** | 13% (0.13) | ~3.1 hrs/day. One car charges during a typical shopping trip or workday. |
| **Maximum** | 30% (0.30) | ~7.2 hrs/day. Near-optimal. Hard to exceed due to the idle blocking problem. |
| **Step Size** | 0.01 (1%) | Fine-grained — 1% on an 11 kW charger is a small absolute energy increment. |

**The Idle Blocking Problem — why AC rarely exceeds 30%:** AC charging is slow (11 kW). A car at a supermarket might finish charging in 3–4 hours, but the driver stays for 6 hours. The plug is occupied but generating zero revenue for the final 2+ hours. This structural parking problem makes exceeding 30% utilization nearly impossible without active overstay pricing.

**State of Charge (SoC) context:** EV batteries charge non-linearly. From **80% → 100% SoC**, the Battery Management System (BMS) throttles incoming power to prevent thermal damage — low AC power (11 kW) handles this phase safely. AC is therefore ideal for **overnight / destination** charging where the car sits for 6–10 hours completing a slow, safe full top-up.

---

### 🎚️ Slider 3: DC / HPC Asset Efficiency (Parameter 3)

**Controls:** Utilization multiplier on all DC/HPC charging points in `C_Supply_Capacity_GWh`.

| Setting | Value | Real-World Meaning |
|---|---|---|
| **Minimum** | 5% (0.05) | ~1.2 hrs/day. 2–3 cars served. Failed location. |
| **Historical Baseline** | 9% (0.09) | ~2.1 hrs/day. ~4–5 cars/day at ~25–30 min per session. |
| **Maximum** | 40% (0.40) | ~9.6 hrs/day. ~19–20 cars/day. A premium Autobahn hub at near-full capacity. |
| **Step Size** | 0.01 (1%) | Critical granularity — 1% on a 150 kW charger ≈ 13,140 kWh/year × €0.50 ≈ **~€6,570 additional revenue per plug per year**. |

**Why DC caps at ~40%:** Even the best highway hubs face unavoidable friction — bay maneuvering (2–5 min), payment authorization (1–3 min), peak-hour slot gaps, and near-zero nighttime demand. 40% is the aspirational ceiling for a world-class CPO location.

**SoC context:** DC chargers bypass the car's onboard AC-to-DC converter and push current directly into the battery at very high power (150–350 kW), exploiting the **10% → 80% SoC window** where the battery accepts maximum charge freely. Above 80%, the BMS aggressively tapers accepted power to protect cell longevity — the commercial session effectively ends at 80% to maximize turnover.

**The margin story:** DC/HPC stations dispense 10–20× more energy per hour than AC, serve more customers per day, and command premium per-kWh pricing. The DC Efficiency slider is the **single most impactful revenue lever** in the entire dashboard.

---

### 🎚️ Slider 4: Market Adoption Speed (Parameter 4)

**Controls:** `C_Interactive_Fleet_Size` — projected total German EV fleet 2026–2030 as a multiplier on the government's 15M base target.

| Setting | Value | Scenario |
|---|---|---|
| **0.5** | Stagnation | ~7.5M EVs by 2030. Policy delays, subsidy removal, or infrastructure gaps suppressing demand. |
| **1.0 (Baseline)** | Base Case | ~15M EVs by 2030. Official **Masterplan Ladeinfrastruktur II** government target. |
| **1.5** | Boom | ~22.5M EVs by 2030. Driven by BEV-ICE price parity or accelerated regulatory ICE phase-out. |

**Why this is the most powerful lever:** Fleet size multiplies every downstream calculation. Shifting from 1.0 → 1.5 adds ~50% to the Revenue Opportunity Gap — instantly reframing the entire investment thesis.

---

## 📈 Key Performance Indicators

### ⭐ North Star KPI: Revenue Opportunity Gap (€)

```
Revenue_Gap_EUR =
    (C_National_Fleet_Demand_GWh − C_Supply_Capacity_GWh)
    × 1,000,000      [GWh → kWh]
    × €0.50 / kWh    [blended CPO retail price]
```

**Why €0.50/kWh?** Blended average CPO retail price in Germany — a mix of AC tariffs (~€0.40–0.45) and DC/HPC tariffs (~€0.55–0.75). Converts an abstract GWh shortfall into a direct, tangible revenue opportunity — the number that makes a CPO sit up and listen.

---

### 🇪🇺 Regulatory KPI: AFIR Compliance Index (kW / BEV)

```
AFIR_Index = Total_Installed_Power_kW / Total_EV_Fleet
           → Must be ≥ 1.3 kW per vehicle  (EU AFIR Regulation mandate)
```

The EU's **Alternative Fuels Infrastructure Regulation (AFIR)** requires a minimum of **1.3 kW of public charging power per registered BEV**. The dashboard displays a red / amber / green compliance status — immediately signalling where the market is **"At Risk"** vs. **"Compliant"** and where CPO investment is most urgent.

---

### 📊 Supporting KPIs

| KPI | Logic | Strategic Value |
|---|---|---|
| **Total EV Fleet** | Historical: KBA data. Projected: Base × Adoption Speed | Demand anchor for all calculations |
| **EV Market Share (%)** | EV Stock / Total Vehicle Fleet | Market maturity indicator |
| **EVs per Charging Point** | SUM(EVs) / SUM(Chargers) | Infrastructure pressure gauge |
| **Infrastructure Sufficiency (%)** | (Actual Plugs / Required Plugs) × 100 | Under/over-supply assessment |
| **Required Total Plugs** | Demand Gap (kWh) / (50 kW × 8,760 × avg. efficiency) | Physical investment target for CPO |
| **Energy Sales Potential (GWh/yr)** | Addressable CPO Market × CPO capture share | Top-line revenue sizing |
| **High-Margin Asset Mix** | AC vs. DC point count over time | Portfolio optimization signal |

---

## 📁 Data Sources

All data is publicly available. Full documentation with access dates and URLs is in the presentation deck.

| Source | Data Used |
|---|---|
| **Kraftfahrt-Bundesamt (KBA)** | German EV registrations & total vehicle fleet (2017–2024) |
| **Bundesnetzagentur** | Public charging point registry — AC/DC split, geographic distribution |
| **European Alternative Fuels Observatory (EAFO)** | EU-wide EV stock & charger benchmarks |
| **IEA Global EV Outlook** | International growth trend comparisons and scenario references |
| **Masterplan Ladeinfrastruktur II (BMWK)** | Government targets (15M EVs by 2030, AFIR compliance roadmap) |
| **Shell / Prognos Mobility Study** | Mileage assumptions, charging behavior baselines |
| **BDEW / Statista** | Consumer charging behavior split, AC vs. DC usage distribution |

---

## 🛠️ Tools & Technologies

| Tool | Role |
|---|---|
| **Tableau Desktop** | Dashboard design, parameter/slider logic, calculated fields, interactivity |
| **Microsoft Excel** | Data collection, cleaning, transformation, source documentation |
| **PowerPoint** | Client-facing presentation: market context, methodology, key findings |

---

## 💡 Key Findings

- Germany's EV fleet is on a trajectory toward **15 million vehicles by 2030** — a ~10× demand increase from ~1.5M in 2023
- At current deployment rates, the public charging network will face **structural undersupply by 2027–2028**, with the gap widening sharply thereafter
- **DC/HPC is the critical margin lever** — eeach 1% improvement in utilization translates to ~€6,570 in additional annual revenue per plug at €0.50/kWh
- As adoption shifts to urban apartment dwellers, the share of demand flowing to public CPOs could rise from 30% to 60–80%, dramatically expanding the addressable market
- Germany risks breaching the **EU AFIR 1.3 kW/BEV threshold by 2027** without accelerated infrastructure investment — a first-mover opportunity for proactive CPOs

---

## 🔒 Access to Dashboard & Dataset

The interactive Tableau workbook and underlying dataset are **not publicly hosted** to protect the analytical methodology.

If you'd like to explore the full tool or discuss the project in detail, feel free to reach out:

- 💼 [LinkedIn: www.linkedin.com/in/virendra-kowale](#) — connect and send a message
- 📧 [Email: kowalevirendra@gmail.com](#) — direct request for a live walkthrough

---

## 👤 Author

**Virendra Kowale**
Data Analytics · Business Intelligence · EV & Mobility Markets

[LinkedIn: www.linkedin.com/in/virendra-kowale](#) · [Portfolio: https://itsviru.github.io/VirendraKowale.github.io/](#) · [Email: kowalevirendra@gmail.com](#)

---

## 📜 License & Attribution

This project was created as an independent response to a consulting case study brief. All underlying data is sourced from public institutions (see Data Sources). The analytical methodology, dashboard architecture, calculated fields, scenario logic, and visual design are original work by the author.

---

*"Moving a slider re-calculates a billion-euro forecast. That's the point."*
