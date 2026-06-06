# Improvado Marketing Data Analyst — Technical Assignment

**Candidate:** Hari  
**Role:** Middle+ Marketing Data Analyst  
**Company:** Improvado  
**Dataset Period:** January 2024

---

## Overview

This project transforms raw advertising data from three major platforms — Facebook, Google Ads and TikTok — into a unified data model and an interactive three-page Power BI dashboard that enables cross-channel performance analysis.



## Data Sources

| File | Platform | Rows | Key Metrics |
|------|----------|------|-------------|
| `01_facebook_ads.csv` | Facebook Ads | 110 | spend, impressions, clicks, conversions, video_views |
| `02_google_ads.csv` | Google Ads | 110 | cost, impressions, clicks, conversions, conversion_value |
| `03_tiktok_ads.csv` | TikTok Ads | 110 | cost, impressions, clicks, conversions, video_views, likes |

---

## Data Model

### Approach
All three source tables were loaded into Power BI via Power Query. Column names were standardised across platforms (`cost` → unified as `cost`, platform-specific fields normalised) and a `source` column was added to each table before combining.

### Unified Table
The three tables were appended using `Table.Combine` into a single unified table:

```m
= Table.Combine({facebook_ads, tiktok_ads, google_ads})
```

**Result:** 330 rows × 11 columns — one clean cross-platform dataset.

### Final Table Structure

| Column | Description |
|--------|-------------|
| `date` | Date of the record (Date type) |
| `source` | Platform — Facebook, Google Ads, TikTok |
| `campaign_id` | Unique campaign identifier |
| `campaign_name` | Campaign name |
| `ad_group_id` | Ad set / ad group / adgroup ID |
| `ad_group_name` | Ad set / ad group / adgroup name |
| `impressions` | Total impressions |
| `clicks` | Total clicks |
| `cost` | Total spend in USD |
| `conversions` | Total conversions |
| `video_views` | Total video views (where applicable) |

### DAX Measures
All calculated KPIs are stored in a dedicated `_DAX Measures` table:

```dax
Total Spend = SUM(Unified[cost])
Total Impressions = SUM(Unified[impressions])
Total Clicks = SUM(Unified[clicks])
Total Conversions = SUM(Unified[conversions])
CPA = DIVIDE([Total Spend], [Total Conversions])
CTR % = DIVIDE([Total Clicks], [Total Impressions]) * 100
CPM = DIVIDE([Total Spend], [Total Impressions]) * 1000
CPC = DIVIDE([Total Spend], [Total Clicks])
Current Date = "As of " & FORMAT(TODAY(), "DD MMM YYYY")
```

---

## Dashboard

### Page 1 — Executive Overview
*Answers: How are we performing overall?*

- KPI cards — Total Spend, Impressions, Conversions, CTR
- Donut chart — Spend share by platform
- Line chart — Daily spend trend by platform
- Line chart — Daily conversions trend by platform
- Platform slicer — filter entire page by channel

### Page 2 — Channel Performance
*Answers: Which platform is most efficient?*

- KPI cards — Blended CPA, CTR, CPM, CPC
- Bar charts — CPA, CTR, CPM by platform (side by side)
- Gauge — Total Conversions vs target
- Clustered bar — Spend vs Conversions by platform
- Summary matrix table — all metrics per platform

### Page 3 — Campaign Deep Dive
*Answers: Which campaigns should we scale or cut?*

- Horizontal bar chart — CPA by campaign (sorted lowest to highest)
- Scatter chart — Spend vs Conversions per campaign
- Full campaign table — all 12 campaigns with Spend, Conversions, CPA, CTR

---

## Key Findings

### Overall Performance (January 2024)

| Metric | Value |
|--------|-------|
| Total Spend | $130,244 |
| Total Impressions | 40.5M |
| Total Clicks | 688,333 |
| Total Conversions | 13,363 |
| Blended CPA | $9.75 |
| Blended CTR | 1.70% |

### Platform Efficiency

| Platform | Spend | Conv. | CPA | CTR | CPM |
|----------|-------|-------|-----|-----|-----|
| Facebook | $18.3K | 2,395 | $7.64 | 1.96% | $4.03 |
| Google | $37.7K | 4,218 | $8.93 | 1.90% | $5.22 |
| TikTok | $74.3K | 6,750 | $11.00 | 1.61% | $2.59 |

### Top Campaigns

| Campaign | Platform | CPA | CTR |
|----------|----------|-----|-----|
| Search Brand Terms | Google | $5.10 | 5.22% |
| Conversions Retargeting | Facebook | $5.95 | 4.63% |
| Shopping All Products | Google | $6.34 | 3.34% |

### Campaigns Needing Attention

| Campaign | Platform | CPA | Issue |
|----------|----------|-----|-------|
| Search Generic Terms | Google | $24.80 | 5x above best performer — keyword review needed |
| Video Views Campaign | Facebook | $14.96 | Top of funnel — expected but monitor |
| Awareness GenZ | TikTok | $13.00 | Shift budget to conversion campaigns |

---

## Recommendations

**1. Reallocate budget toward high efficiency campaigns**
Google Brand Search ($5.10 CPA) and Facebook Retargeting ($5.95 CPA) are significantly underinvested. Increasing budget here will improve blended CPA.

**2. Fix Google Generic Search immediately**
At $24.80 CPA this is the biggest inefficiency in the mix. Recommended actions:
- Add negative keywords
- Tighten ad group structure
- Improve landing page relevance

**3. Rebalance TikTok spend**
Shift budget from pure awareness campaigns (Awareness GenZ at $13.00 CPA) toward conversion-focused campaigns (Conversion Focus at $10.00 CPA). Same platform, better return.

**Expected outcome:** Implementing these three changes could bring blended CPA from $9.75 down to approximately $7.00 — generating hundreds of additional conversions on the same budget.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Power BI Desktop | Data loading, transformation, modelling, dashboard |
| Power Query (M) | Data cleaning and unification |
| DAX | Calculated measures |
| SQL | Unified data model definition |

---

## How to Open

1. Download `Improvado_Marketing_Analysis_Hari.pbix`
2. Open in Power BI Desktop (free download at powerbi.microsoft.com)
3. The data is embedded — no connection setup needed
4. Navigate between the 3 pages using the page tabs at the bottom

---
