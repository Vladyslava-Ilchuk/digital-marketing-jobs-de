# Digital Marketing Job Market in Germany
 
A data analytics practicum project analyzing active Digital Marketing job postings in Germany, built end-to-end: data collection via a public REST API, cleaning and structuring in Python, and interactive visualization in Tableau.
 
**[View the presentation](presentation/Digital_Marketing_Job_Market_DE.pptx)** · **[Dashboard screenshots](dashboards/screenshots)**
 
---
 
## Project Overview
 
This project explores a single question: *what does active demand for Digital Marketing talent in Germany actually look like — in terms of employers, regions, seniority, and required skills?*
 
The analysis is based on **1,899 live job postings**, collected over a 30-day window (July–August 2026) directly from the [Bundesagentur für Arbeit](https://www.arbeitsagentur.de/) Jobsuche API — Germany's Federal Employment Agency.
 
## Data Pipeline
 
```
Search API (10 keywords) → Detail API (per posting) → Clean & Deduplicate → Structure for Tableau
```
 
1. **Collection** (`scripts/scrape_digital_marketing_jobs.py`)
   Queries the Jobsuche API across 10 German-market keywords (`Online Marketing`, `SEO`, `SEA`, `Performance Marketing`, etc.), paginating through results and fetching full posting details — description, employment type, and location — for each result.
2. **Skill & level extraction**
   Hard skills, soft skills, and seniority level (Junior / Middle / Senior) are extracted from the free-text job description using a curated set of German-language keyword patterns.
3. **Cleaning** (`scripts/clean_step2_tableau_prep.py`)
   Removes exact and near-duplicate postings (same employer + normalized title), filters out off-topic results caused by ambiguous keyword matches (e.g. "SEA" matching unrelated shipping/logistics postings), fills missing values, and enforces correct data types (dates as dates, IDs as text) for a lossless Tableau import.
4. **Output**
   Two analysis-ready tables, exported as both `.parquet` (preferred, preserves types exactly) and `.xlsx` (fallback):
   - `jobs_tableau` — one row per job posting
   - `jobs_skills_long` — one row per (job, skill) pair, for skill-frequency analysis
## Data Cleaning Summary
 
| Stage | Postings |
|---|---|
| Collected (raw, all keywords) | 9,231 |
| After exact-duplicate removal | 1,922 |
| **Final analysis-ready dataset** | **1,899** |
 
*16 postings removed as off-topic · 6 additional near-duplicate reposts removed*
 
## Dashboards
 
Two Tableau dashboards were built from the cleaned data:
 
- **Market Overview** — total postings, top employers, geographic distribution (by Bundesland and city), and posting activity over time
- **Skills & Seniority Deep Dive** — top hard and soft skills, seniority level distribution, and skill requirements broken down by role and level
See [`dashboards/screenshots`](dashboards/screenshots) for the final views.
 
## Key Findings
 
- The market is **highly fragmented** — the largest single employer accounts for under 1% of postings.
- **Nordrhein-Westfalen and Bavaria** together account for over a third of all postings, tracking Germany's largest economic centers.
- Posting activity follows a **clear business-week rhythm**, peaking Thursday and nearly disappearing on Saturday.
- **Soft skills dominate the language of job ads** — "Initiative" is mentioned more often than any single hard skill.
- **63% of postings don't state a seniority level**, and among those that do, Senior roles dominate — a real navigation challenge for entry-level candidates.
## Tech Stack
 
- **Python** — `pandas`, `requests`, `pyarrow`, `openpyxl`
- **Data source** — Bundesagentur für Arbeit Jobsuche API
- **Visualization** — Tableau
- **Environment** — Google Colab
## Limitations
 
- Keyword-based collection introduces some noise from ambiguous short keywords.
- Seniority level is inferred from free text, not a structured field — the true mix may differ.
- The dataset reflects a single 30-day window; seasonal patterns aren't captured.
- ~2% of postings have no specified location, likely fully remote roles.
## Author
 
**Vladyslava Ilchuk** — Data Analytics Weiterbildung, IT Career Hub
