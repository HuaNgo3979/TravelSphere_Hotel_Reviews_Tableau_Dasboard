# TravelSphere - Hotel Reviews BI Dashboard

Individual assignment for **Modern Business Intelligence**, RMIT Melbourne (MBIT). A BI solution started from data cleaning and AI-assisted text analysis to an interactive Tableau dashboard suite, built for **TravelSphere**, a fictional luxury travel agency, to help its Head of Customer Experience refine its European partner hotel portfolio.

**Author:** Ngo Hua Quoc Thinh

🔗 **Live dashboard (Tableau Public):** https://public.tableau.com/app/profile/ngo.hua.quoc.thinh/viz/ASM2_Real-World_Dashboard_HuaQuocThinhNgo_s3863887/GeographicPerformance

---

## 1. Project Overview

TravelSphere needed a way to move beyond gut-feel hotel selection and understand, at scale, which European partner hotels genuinely deliver a luxury-grade guest experience. This project uses a 2017 hotel reviews dataset (156,681 cleaned reviews, 1,490 hotels, 206 reviewer nationalities, across London, Paris, Barcelona, Amsterdam, Milan, and Vienna) to build a four-dashboard, executive-ready Tableau report that answers:

- Which cities and hotels perform best, and which are reliable enough to recommend?
- Do reviewers from different nationalities rate hotels differently?
- What's actually driving satisfaction and dissatisfaction — and which internal team owns the fix?
- How does performance shift across the season?

---

## 2. Repository Structure

```
TravelSphere-Hotel-Reviews-BI-Dashboard/
├── README.md
├── docs/
│   ├── ASM2_Real-World_Dashboard_HuaQuocThinhNgo_s3863887.docx   (full written report)
│   └── ASM2_Real-World_Dashboard_HuaQuocThinhNgo_s3863887.pdf    (final submitted PDF)
├── tableau/
│   ├── ASM2_Real-World_Dashboard_HuaQuocThinhNgo_s3863887.twbx   (packaged workbook — open directly in Tableau Desktop)
│   ├── ASM2_Dash_Hotel_Reviews.twb                               (lightweight workbook, no embedded data extract)
│   └── ASM2_Real-World_Dashboard.png                             (dashboard preview screenshot)
└── data-prep/
    └── Cleaned_2017_Hotel_Reviews.tfl                            (Tableau Prep Builder flow — cleaning steps/recipe)
```

> **Note on data files:** The raw and intermediate hotel review CSVs (up to ~230MB each, including the AI-enhanced text-analysis exports) are excluded from this repository — they exceed GitHub's file size limits and aren't needed to review the deliverable. The source data is a public 2017 hotel reviews dataset (Hotel_Address, Review_Date, Reviewer_Nationality, Positive/Negative_Review, Reviewer_Score, lat/lng, consistent with the well-known *"515K Hotel Reviews Data in Europe"* dataset). The `.tfl` flow file documents every cleaning step so the pipeline is fully reproducible against that source data. If you need the packaged flow with embedded data, ask — it can be shared separately (e.g. via Drive) or added via Git LFS.

---

## 3. Data Cleaning & Processing ([`data-prep/`](TravelSphere-Hotel-Reviews-BI-Dashboard/data-prep))

Built in **Tableau Prep Builder**, in two stages:

**Stage 1: Initial cleaning**
- Filtered reviews to the 2017 calendar year.
- Removed duplicate review rows.
- Flagged missing `lat`/`lng` (kept for non-geographic analysis, excluded from maps).
- Extracted `City` from `Hotel_Address` via a controlled city dictionary (London, Paris, Barcelona, Amsterdam, Milan, Vienna).
- Derived `Review_Month` / `Review_Month_Number` for chronological seasonality analysis.

**Stage 2: AI-assisted text analysis**
- Used ChatGPT to clean review text, standardise neutral placeholder values (e.g. "No Negative", "N/A", blanks) so they weren't misread as sentiment, and generate: sentiment balance/ratio/label, AI-assisted topic tags, satisfaction/dissatisfaction reasons, and responsible-team flags
- Split, re-unioned, and validated the AI-enhanced output back in Tableau Prep, then exported the final Tableau-ready dataset

**Result:** 156,681 review records • 1,490 hotels • 206 nationalities • average score 8.39 (range 2.5–10.0). Coverage is January–early August 2017 only, so seasonality findings are treated as partial-year trends, not a full annual pattern.

---

## 4. The Dashboards ([`tableau/`](TravelSphere-Hotel-Reviews-BI-Dashboard/tableau))

Four linked, interactive dashboards with a consistent executive layout (KPI cards → diagnostic charts → operational detail), shared filters (city, hotel, month, satisfaction band, minimum review threshold), and a blue/red colour logic (blue = strong performance, red/orange = risk):

| Dashboard | Purpose | Key visuals |
|---|---|---|
| **1. Geographic Performance** | Identify high-performing cities & shortlist reliable partner hotels | KPI cards, City Satisfaction Ranking, Hotel Performance Map, Top Partner Hotel Candidates table (score, review count, negative %, risk level) |
| **2. Nationality Bias Analysis** | Detect rating differences across reviewer nationalities | Nationality Bias Scatter Plot, Harsh vs Generous Reviewer Nationalities, minimum-review threshold parameter |
| **3. Sentiment Driver Analysis** | Explain *why* scores are what they are | Positive/negative topic breakdowns, Responsible Team Priorities table, Positive vs Negative Topic Matrix, Top 10 Negative Sentiments |
| **4. Seasonality Performance** | Track volume/satisfaction/complaint drivers by month | Monthly Review Volume vs Satisfaction, Monthly Negative Drivers, City × Month Satisfaction Heatmap |

**Key calculated fields:** Average Reviewer Score, Review Count (reliability indicator), Satisfaction Band, Risk Level (satisfaction + negative-comment %), Satisfaction/Dissatisfaction Topic, Responsible Team, Recommended Team Action.

---

## 5. Business Use Cases (from the report [`docs/`](TravelSphere-Hotel-Reviews-BI-Dashboard/docs))

- **Use Case 1 — Barcelona luxury package:** Filtering Dashboard 1 to Barcelona (avg. score 8.56, 100+ reviews) and raising the review threshold surfaces *Avenida Palace* as a strong candidate — but Dashboard 3 shows 44% of its negative sentiment is room-related, flagging a quality-assurance check before booking, while Dashboard 4 confirms July's higher volume brings a slight dip in room satisfaction.
- **Use Case 2 — Paris vs. Amsterdam for stricter nationality segments:** Dashboard 2 shows reviewers from India, Saudi Arabia, and the UAE rate consistently below average in both cities, but more sharply in Paris. Dashboard 3 traces this to a higher concentration of room/location/facilities complaints in Paris, and Dashboard 4 shows Paris satisfaction stays lower even outside peak season — recommending stricter hotel screening for Paris bookings with these client segments.

## 6. Business Value

The dashboard suite turns historical review data into a repeatable decision framework for TravelSphere: (1) partner hotel selection based on performance *and* reliability, not just average score; (2) nationality-aware customer advisory; (3) service improvement by routing negative feedback to the responsible team. Together they support TravelSphere's goal of refining its European hotel portfolio for more personalised luxury travel experiences.

---

## 7. How to Use This Repo

- **Read the full report:** [`docs/ASM2_Real-World_Dashboard_HuaQuocThinhNgo_s3863887.pdf`](TravelSphere-Hotel-Reviews-BI-Dashboard/docs/ASM2_Real-World_Dashboard_HuaQuocThinhNgo_s3863887.pdf).
- **Explore the live dashboard:** [`Tableau Public link`](https://public.tableau.com/app/profile/ngo.hua.quoc.thinh/viz/ASM2_Real-World_Dashboard_HuaQuocThinhNgo_s3863887/GeographicPerformance) *(no software needed)*.
- **Open in Tableau Desktop:** `tableau/ASM2_Real-World_Dashboard_HuaQuocThinhNgo_s3863887.twbx` (packaged, includes the data extract).
- **Review the data prep pipeline:** open [`data-prep/Cleaned_2017_Hotel_Reviews.tfl`](TravelSphere-Hotel-Reviews-BI-Dashboard/data-prep/Cleaned_2017_Hotel_Reviews.tfl) in Tableau Prep Builder to see every cleaning step.
