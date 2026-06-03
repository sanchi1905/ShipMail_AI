<div align="center">

# 🚢 ShipMail AI
### Shipping Email Intelligence Platform

[![IME](https://img.shields.io/badge/Built%20for-IME%20Evaluation-00B4D8?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0yMCAxOEg0VjZoMTZ2MTJ6Ii8+PC9zdmc+)](https://github.com/sanchi1905/ShipMail_AI)
[![License](https://img.shields.io/badge/License-MIT-F4A261?style=for-the-badge)](LICENSE)
[![Single File](https://img.shields.io/badge/Single%20File-HTML%20%2B%20JS-48CAE4?style=for-the-badge&logo=html5)](index.html)
[![No LLM API](https://img.shields.io/badge/Zero-LLM%20API%20Calls-059669?style=for-the-badge&logo=shield)](index.html)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-0096C7?style=for-the-badge&logo=github)](https://sanchi1905.github.io/ShipMail_AI/)

**An end-to-end AI-powered shipping email segregation and structured data extraction system.**  
Classifies raw maritime emails into Tonnage, Cargo VC, and Cargo TC — entirely using rule-based NLP with zero external API dependencies.

**Developed by [Sanchi Sisodia](https://github.com/sanchi1905) · Live Evaluation: 4 June 2026**

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Live Demo](#-live-demo)
- [Features](#-features)
- [Classification Engine](#-classification-engine)
- [Data Extraction](#-data-extraction)
- [Tech Stack](#-tech-stack)
- [How to Run](#-how-to-run)
- [Demo Emails](#-demo-emails)
- [Project Structure](#-project-structure)
- [Evaluation Criteria](#-evaluation-criteria)

---

## 🌊 Overview

ShipMail AI is a **production-grade, single-file web application** built for the IME (Indian Maritime Exchange) Shipping Email Intelligence evaluation. It solves the real-world problem of manually sorting and parsing thousands of daily shipping emails by:

1. **Automatically classifying** each email into the correct shipping category
2. **Extracting all structured fields** (vessel name, ports, laycan, DWT, etc.)
3. **Storing records** in a searchable, filterable, exportable in-memory database
4. **Matching** open vessels with compatible cargo opportunities in one click

> ⚠️ **Zero LLM API calls.** All classification and extraction is done using a custom rule-based NLP engine with regex pattern trees — no OpenAI, Anthropic, or Google Gemini.

---

## 🔗 Live Demo

**👉 [https://sanchi1905.github.io/ShipMail_AI/](https://sanchi1905.github.io/ShipMail_AI/)**

Or run locally by simply opening `index.html` in any modern browser — no server or build step required.

---

## ✨ Features

### 🧠 Intelligence Engine
- **3-category classification** with weighted signal scoring
- **Confidence scoring** (0–98%) based on pattern density
- **Disambiguation logic** for overlapping TC/VC signals
- **30+ TONNAGE signals**, **30+ Cargo VC signals**, **15+ TC signals**
- Classification completes in **< 100ms**

### 🎨 UI / UX
- Deep navy `#0A1628` + ocean teal `#00B4D8` + gold `#F4A261` maritime palette
- Animated ship icon (bobbing wave effect)
- **Radar/sonar spin** processing animation
- **Card flip + stagger reveal** for extracted fields
- **Ripple effect** on the Analyze button
- **Animated live counters** in the stats bar
- Floating particle shimmer background
- Skeleton loading placeholders
- Toast notifications (save / export / match)
- Smooth tab transitions

### 🗄️ Database
| Feature | Details |
|---|---|
| Live search | Real-time text highlighting across all fields |
| Category filter tabs | ALL / Tonnage / Cargo VC / Cargo TC |
| Date filter | All Time / Today / Last 7 Days / Last 30 Days |
| Row expand | Full field details + raw email preview |
| Export JSON | Downloads currently filtered records |
| Export CSV | Downloads currently filtered records |
| Print / PDF | Clean print layout, browser-native |
| Match Opportunities | Highlights compatible cargo for any Tonnage vessel |
| Color-coded rows | Blue = Tonnage · Green = VC · Amber = TC |

---

## 🔬 Classification Engine

The engine uses a **weighted multi-signal scoring system** — each email is scored against three independent pattern libraries:

### TONNAGE Signals (30+ patterns)
```
MV/M/V prefix · DWT · LOA · BEAM · Draft · Grain/Bale capacity
Crane specs · Hold/Hatch count · Bulk carrier types (Handysize, Supramax…)
Speed/Knots · IFO/MGO consumption · Flag · Class · GRT/NRT
Year built · P&I · Owner · Vessel keyword
```

### CARGO VC Signals (30+ patterns)
```
Laycan · Load Port · Discharge Port · Freight Rate (PMT/MTS)
FIOS · PWWD · SHEX · Commission % · Voyage Charter keyword
Commodities: MOLASSES, HRC, UREA, COAL, WHEAT, IRON ORE, STEEL,
             CEMENT, SUGAR, RICE, PHOSPHATE, BAUXITE, FLUORSPAR
→ / -> port separator · XXXX MT quantity pattern
```

### CARGO TC Signals (15+ patterns)
```
TCT keyword · Time Charter · Delivery Port · Redelivery Port
Duration (X-Y days/months) · WOG/WOGA · ECI/WCI/Med/GOA
SMX/UMX vessel size · Steels/Gens cargo · Period language
```

**Disambiguation:** When VC and TC signals overlap, the engine applies boosters:
- `TCT` keyword found → +3 points to TC score
- `DWT + open` found → +2 points to Tonnage score  
- `laycan` found in VC context → +2 points to VC score

---

## 📦 Data Extraction

Per-category regex extraction pipelines with cascading fallback patterns:

### TONNAGE Fields
| Field | Extraction Method |
|---|---|
| Vessel Name | `MV/M/V` prefix pattern, then `Vessel:` label |
| Owner / Account | `Owner:`, `Account:`, `c/o:` patterns |
| Open Port | `Open [at/in] <port>` pattern |
| Open Date | Date after `Open` keyword, or standalone date |
| Vessel Type | Named bulk carrier class (Handysize, Supramax, etc.) |
| Vessel Size (DWT) | `\d+ DWT` or `DWT: \d+` |

### CARGO VC Fields
| Field | Extraction Method |
|---|---|
| Account Name | `Account:`, `Charterer:` |
| Cargo Name | Commodity keyword list + `\d+ MT <commodity>` |
| Loading Port | `Load Port:`, `From:` before `→` |
| Discharge Port | After `→` or `to:`, `Discharge Port:` |
| Laycan | `Laycan:`, `Layday:`, `Mid/Early/Late <Month>` |
| Cargo Type | Bulk / Liquid / Breakbulk keyword |

### CARGO TC Fields
| Field | Extraction Method |
|---|---|
| Account Name | `Account:`, `For account of:` |
| Cargo Name | Grain/Steel/Gen cargo keywords |
| Delivery Port | `Delivery:`, `Dely:` |
| Redelivery Port | `Redelivery:`, `Redely:` |
| Duration | `X-Y days/months` pattern |
| Laycan | Date range pattern |
| Cargo Type | Bulk / Dry / General Cargo |

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Framework** | React 18 (via CDN, no build step) |
| **JSX Compiler** | Babel Standalone (browser runtime) |
| **Styling** | Vanilla CSS (custom design system, no Tailwind) |
| **Typography** | Google Fonts — Inter + Space Grotesk |
| **NLP Engine** | Pure JavaScript regex + keyword scoring |
| **State** | React `useState` / `useRef` / `useCallback` |
| **Data Storage** | In-memory JS arrays (session state) |
| **Export** | Browser Blob API (JSON + CSV download) |
| **Deployment** | GitHub Pages (static hosting) |

> No backend server · No database · No LLM API · No npm install · No build required

---

## 🚀 How to Run

### Option 1 — Just open the file
```
Double-click index.html in File Explorer
```

### Option 2 — Local server (optional, for strict CORS environments)
```bash
# Python
python -m http.server 3000

# Node.js
npx serve .
```
Then visit `http://localhost:3000`

### Option 3 — Live on GitHub Pages
```
https://sanchi1905.github.io/ShipMail_AI/
```

---

## 📧 Demo Emails

The app comes pre-loaded with 6 realistic records. You can also test with these examples using the **Quick Demo Email** pills:

| # | Type | Key Detail |
|---|---|---|
| 1 | 🚢 TONNAGE | MV SHENG AN HAI · DWT 56,564 · Open Xiamen, China · 2 Jun 2026 |
| 2 | 🚢 TONNAGE | MV BLUE STAR · DWT 37,947 · Open Gabes, Tunisia · 25 May 2026 |
| 3 | 📦 CARGO VC | 15-20K MT MOLASSES · Koh Si Chang → Kandla+Chennai · Mid Jul 2026 |
| 4 | 📦 CARGO VC | 20K MT HRC Steel · Jeddah → Bilbao · 25 Jun–5 Jul 2026 |
| 5 | ⏱ CARGO TC | 1 TCT Grains · Delivery Vancouver · Redelivery Chittagong · 10-17 Jun |
| 6 | ⏱ CARGO TC | 1 TCT Steels/Gens · Delivery ECI · Redelivery Med via GOA · 15-18 Jul |

---

## 📁 Project Structure

```
shipmail-ai/
│
└── index.html          # Complete application — CSS + React JSX + NLP engine
    │
    ├── CSS Styles       (~1,150 lines) — Design tokens, animations, layout
    ├── Classification   (~60 lines)  — TONNAGE / VC / TC signal arrays + scorer
    ├── Extraction       (~120 lines) — Per-category regex extraction pipelines
    ├── Demo Data        (~80 lines)  — 6 pre-loaded realistic records
    └── React App        (~650 lines) — Components, state, UI, export, match engine
```

---

## 🏆 Evaluation Criteria

| Criterion | Implementation |
|---|---|
| ✅ Classification accuracy | 30–45 domain signals per category with confidence scoring |
| ✅ Completeness of extraction | 6–7 structured fields per category |
| ✅ UI polish & impressiveness | Maritime theme, glassmorphism, 10+ CSS animations |
| ✅ Speed & smoothness | < 100ms classification, CSS GPU-accelerated animations |
| ✅ Scalability of engine | Signal arrays easily extended; scoring is O(n) |
| ✅ Vessel-cargo matching (bonus) | 🔗 Match button on Tonnage rows highlights compatible cargo |
| ✅ Zero external LLM dependency | Pure JS regex, no API keys needed |

---

<div align="center">

**ShipMail AI** · IME Shipping Email Intelligence Platform  
Built for Live Evaluation **4 June 2026**

Developed with ❤️ by **[Sanchi Sisodia](https://github.com/sanchi1905)**

</div>
