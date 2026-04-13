# A.L.I.S. — AI Lead Intelligence System

> AI-powered lead scoring engine for the DACH market. Detects recruiting failure signals and scores companies automatically via a weighted signal engine.

![Status](https://img.shields.io/badge/Status-Case%20Study-blue?style=flat)
![Market](https://img.shields.io/badge/Market-DACH-lightgrey?style=flat)
![Stack](https://img.shields.io/badge/Stack-n8n%20%7C%20Apollo.io%20%7C%20Airtable-purple?style=flat)
![Type](https://img.shields.io/badge/Type-Self--Initiated-orange?style=flat)

---

## Overview

A.L.I.S. (AI Lead Intelligence System) is a self-initiated case study building an intelligent lead qualification engine purpose-built for the **DACH market**. Rather than scoring leads by firmographic data alone, A.L.I.S. detects **9 recruiting failure signals** derived from LinkedIn activity and Apollo.io enrichment data — surfacing companies that are actively scaling but struggling to hire.

The output: a clean Hot / Warm / Cold classification per company, updated automatically as new signals are detected.

---

## Signal Detection

A.L.I.S. monitors 9 proprietary recruiting failure signals including:

- Repeated job postings for the same role (hiring velocity mismatch)
- Extended time-to-fill patterns across departments
- Sudden headcount spikes without corresponding senior hires
- LinkedIn engagement patterns indicating talent brand weakness
- Role mismatch signals between job titles and company growth stage
- ...and 4 additional signals derived from DACH-specific hiring behavior

Each signal is weighted independently. The final score is computed via a **weighted signal aggregation engine**, producing a normalized score mapped to Hot / Warm / Cold tiers.

---

## System Architecture

```
LinkedIn Data ──┐
               ├──► Data Enrichment (Apollo.io + PhantomBuster)
Apollo.io ─────┘              │
                              ▼
                   Signal Detection Engine (n8n)
                   [9 weighted failure signals]
                              │
                              ▼
                   Weighted Scoring Model
                   [Hot / Warm / Cold]
                              │
                              ▼
                   Airtable CRM Output
                   [Enriched lead records + scores]
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Orchestration | n8n |
| Lead Data | Apollo.io |
| LinkedIn Scraping | PhantomBuster |
| Data Storage & CRM | Airtable |
| Signal Source | LinkedIn activity data |
| Architecture | Automated multi-source enrichment pipeline |

---

## Output

Each processed company record in Airtable includes:

- **Lead Score** — Hot / Warm / Cold classification
- **Signal Breakdown** — which of the 9 signals were triggered
- **Signal Strength** — weighted score per signal
- **Enrichment Data** — firmographics, headcount, funding stage, tech stack (via Apollo)
- **LinkedIn Signals** — activity patterns and hiring behavior data

---

## Case Study Context

A.L.I.S. was designed as a **self-initiated portfolio project** to demonstrate:

- Applied AI in B2B sales infrastructure
- Signal-based scoring as an alternative to rules-based qualification
- Automated enrichment pipeline design for DACH outbound motions

Full case study documentation available in the [Portfolio Drive](https://drive.google.com/drive/folders/16IQsA1VIEQMihnZebqVT_On0AmO6vvXv).

---

*Built by [Aria Irani](https://www.linkedin.com/in/aria-irani-7a9563261) · Berlin*
