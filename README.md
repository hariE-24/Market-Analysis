Overview
This project transforms raw advertising data from three major platforms — Facebook, Google Ads and TikTok — into a unified data model and an interactive three-page Power BI dashboard that enables cross-channel performance analysis.

Data Sources
FilePlatformRowsKey Metrics01_facebook_ads.csvFacebook Ads110spend, impressions, clicks, conversions, video_views02_google_ads.csvGoogle Ads110cost, impressions, clicks, conversions, conversion_value03_tiktok_ads.csvTikTok Ads110cost, impressions, clicks, conversions, video_views, likes

Data Model
Approach
All three source tables were loaded into Power BI via Power Query. Column names were standardised across platforms and a source column was added to each table before combining.
Unified Table
The three tables were appended using Table.Combine into a single unified table:
m= Table.Combine({facebook_ads, tiktok_ads, google_ads})
Result: 330 rows × 11 columns — one clean cross-platform dataset.
Final Table Structure
ColumnDescriptiondateDate of the record (Date type)sourcePlatform — Facebook, Google Ads, TikTokcampaign_idUnique campaign identifiercampaign_nameCampaign namead_group_idAd set / ad group / adgroup IDad_group_nameAd set / ad group / adgroup nameimpressionsTotal impressionsclicksTotal clickscostTotal spend in USDconversionsTotal conversionsvideo_viewsTotal video views (where applicable)
DAX Measures
All calculated KPIs are stored in a dedicated _DAX Measures table:
daxTotal Spend = SUM(Unified[cost])
Total Impressions = SUM(Unified[impressions])
Total Clicks = SUM(Unified[clicks])
Total Conversions = SUM(Unified[conversions])
CPA = DIVIDE([Total Spend], [Total Conversions])
CTR % = DIVIDE([Total Clicks], [Total Impressions]) * 100
CPM = DIVIDE([Total Spend], [Total Impressions]) * 1000
CPC = DIVIDE([Total Spend], [Total Clicks])
Current Date = "As of " & FORMAT(TODAY(), "DD MMM YYYY")

Dashboard
Page 1 — Executive Overview
Answers: How are we performing overall?

KPI cards — Total Spend, Impressions, Conversions, CTR
Donut chart — Spend share by platform
Line chart — Daily spend trend by platform
Line chart — Daily conversions trend by platform
Platform slicer — filter entire page by channel

Page 2 — Channel Performance
Answers: Which platform is most efficient?

KPI cards — Blended CPA, CTR, CPM, CPC
Bar charts — CPA, CTR, CPM by platform side by side
Gauge — Total Conversions vs target
Clustered bar — Spend vs Conversions by platform
Summary matrix table — all metrics per platform

Page 3 — Campaign Deep Dive
Answers: Which campaigns should we scale or cut?

Horizontal bar chart — CPA by campaign sorted lowest to highest
Scatter chart — Spend vs Conversions per campaign
Full campaign table — all 12 campaigns with Spend, Conversions, CPA, CTR


Key Findings
Overall Performance — January 2024
MetricValueTotal Spend$130,244Total Impressions40.5MTotal Clicks688,333Total Conversions13,363Blended CPA$9.75Blended CTR1.70%
Platform Efficiency
PlatformSpendConv.CPACTRCPMFacebook$18.3K2,395$7.641.96%$4.03Google$37.7K4,218$8.931.90%$5.22TikTok$74.3K6,750$11.001.61%$2.59
Top Performing Campaigns
CampaignPlatformCPACTRSearch Brand TermsGoogle$5.105.22%Conversions RetargetingFacebook$5.954.63%Shopping All ProductsGoogle$6.343.34%
Campaigns Needing Attention
CampaignPlatformCPAIssueSearch Generic TermsGoogle$24.805x above best performer — keyword review neededVideo Views CampaignFacebook$14.96Top of funnel — monitor closelyAwareness GenZTikTok$13.00Shift budget to conversion campaigns

Recommendations
1. Reallocate budget toward high efficiency campaigns
Google Brand Search at $5.10 CPA and Facebook Retargeting at $5.95 CPA are significantly underinvested. Increasing budget here will improve blended CPA immediately.
2. Fix Google Generic Search immediately
At $24.80 CPA this is the biggest inefficiency in the mix. Recommended actions:

Add negative keywords
Tighten ad group structure
Improve landing page relevance

3. Rebalance TikTok spend
Shift budget from pure awareness campaigns toward conversion focused campaigns. Same platform, better return.
Expected outcome: Implementing these three changes could bring blended CPA from $9.75 down to approximately $7.00 — generating hundreds of additional conversions on the same budget.

Tools Used
ToolPurposePower BI DesktopData loading, transformation, dashboardPower Query MData cleaning and unificationDAXCalculated measuresSQLUnified data model definition

How to Open

Download Improvado_Marketing_Analysis_Hari.pbix
Open in Power BI Desktop — free download at powerbi.microsoft.com
Data is embedded — no connection setup needed
Navigate between the 3 pages using the tabs at the bottom
