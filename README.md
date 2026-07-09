# A.L.I.S. — Automated Lead Intelligence System

A signal-based lead scoring pipeline built in n8n. It scrapes open job postings, enriches the companies behind them, scores each company against a set of weighted buying signals, and tiers them Hot / Warm / Cold for outbound — with a full audit trail per company and no manual qualification.

Built as a portfolio case study for a recruitment ICP ("HEAD|MATCH"): companies that are actively failing to fill senior roles are the buying signal.

---

## The problem it solves

Lead qualification is normally a person reading company signals by hand — is this company hiring, are they funded, is a senior role sitting open — and guessing whether it's worth outreach. That judgment is a repeatable rule, not real intuition. A.L.I.S. makes the rule explicit and runs it automatically, so the qualification work happens without a human doing it forty times a day.

---

## Architecture

The pipeline runs in stages, left to right on the canvas:

**1. Input seed**
A keyword and category (e.g. "Software Engineer" / "Jobs") define the search.

**2. Job scraping — PhantomBuster**
Pulls open job postings and the companies posting them.

**3. Group by company**
A Code node collapses multiple job posts into one record per company, collecting all job titles, job count, and LinkedIn slug.

**4. Filter — Germany + senior roles**
Narrows to the target market and to companies with senior-level openings.

**5. Enrichment — Apollo + PhantomBuster profiles**
Company-level data (employee count, funding, growth) and people-level data (leadership, board members) are pulled in for scoring.

**6. Signal scoring layer**
Each company is scored against weighted buying signals (below). Scores are summed into a total.

**7. Tiering**
Total score maps to a tier: **Hot ≥ 20, Warm ≥ 10, Cold < 10.** Companies are sorted by score descending.

**8. Output — Airtable + Google Sheets**
Scored, tiered companies land in Airtable ("Matchday" base) with every signal value stored per record — the audit trail. Pipedrive is wired for downstream CRM handoff.

---

## The signals

Each signal is a specific, checkable condition with a weight reflecting how strongly it indicates a company is stuck filling a senior role.

| # | Signal | Weight | Logic |
|---|--------|--------|-------|
| 1 | Open 60+ days | 7 | Role posted 60+ days ago — trying and failing to fill for 2+ months |
| 2 | Role reposted | 8 | Same role posted again — first attempt didn't land |
| 3 | Generalist agency | 6 | Posted by a generic staffing/consulting agency with no specialist search capability |
| 4 | Senior role open | 7 | A CTO / VP Eng / Head of Eng / Principal / Tech Lead role is open — leadership gap |
| 5 | HR team ≤ 3% + 100+ employees + senior role open | 6 | Under-resourced HR carrying a senior search |
| 6 | Growing 15%+ YoY + senior role open | 9 | Scaling fast with a leadership gap — classic HEAD\|MATCH situation |
| 7 | HR team ≤ 5% + 100+ employees + senior role open | 8 | Placeholder likely covering a missing senior hire |
| 8 | CEO / founder personally posting | — | Identified by AI agent from the employee list — no dedicated hiring function |
| 9 | Board / investor involvement | — | Board or investor-level people present — pressure from above on the hire |
| 10 | Average tenure < 2.5 years | 7 | Retention problem compounding the hiring problem |

Signals 8 and 9 are extracted by an AI agent (structured JSON output, German-title-aware: Geschäftsführer, Aufsichtsrat, Beirat) rather than by rule, because they require reading a messy employee list rather than checking a field.

---

## Stack

n8n · PhantomBuster · Apollo · OpenAI (signal extraction agent) · Airtable · Google Sheets · Pipedrive

Node breakdown: 13 Code nodes, 11 HTTP Request, 5 Airtable, 5 Google Sheets, 2 AI agents, plus filtering, merging, and batching logic.

---

## Notes

- This repository contains the workflow structure. All credentials and API keys have been replaced with placeholders — supply your own in n8n's credential store to run it.
- Signals 1–4 and 10 are wired into the live scoring calculation; signals 5–9 are implemented in the enrichment and documentation layer and represent the scoring model's full design. The weighting model is configurable.
- A.L.I.S. is a methodology and a configurable framework. This is one instantiation, tuned to a recruitment ICP. The same architecture rescopes to any ICP by swapping the signals and weights.
