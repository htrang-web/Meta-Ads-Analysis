# Meta Ads Performance Analysis | Power BI

An interactive Power BI project analyzing advertising performance across Facebook and Instagram, covering campaign effectiveness, audience targeting, ad format performance, and budget efficiency. The project turns 400,000 ad events into marketing insights using data modeling, Power Query, DAX, and dashboard design.

---

## Project Overview

Digital advertising performance depends on multiple factors — platform, campaign, ad format, audience characteristics, targeting accuracy, and budget allocation. This project analyzes advertising data from Facebook and Instagram to understand how campaigns perform through the marketing funnel:

**Impressions → Clicks → Engagement → Purchases**

The analysis is organized around four areas:

1. Overall campaign and funnel performance
2. Performance trends and ad format effectiveness
3. Audience and targeting accuracy
4. Budget allocation and cost efficiency

## Project Objectives

- Evaluate campaign performance across the marketing funnel.
- Identify high-performing ad formats.
- Analyze audience demographics and targeting accuracy.
- Examine the relationship between engagement and conversion.
- Evaluate campaign budget allocation and cost efficiency.

### Key Business Questions

- How efficiently do impressions convert into clicks and purchases?
- Which ad formats generate the strongest engagement and conversion?
- Which audience segments generate the most purchases?
- How accurately does actual audience delivery match targeting parameters?
- Which campaigns deliver purchases at the lowest cost?
- Does higher engagement lead to higher conversion?

---

## Tools & Technologies

- **Power BI** – dashboard development, data modeling, visualization
- **Power Query** – data preparation and transformation
- **DAX** – funnel, engagement, and cost-efficiency measures
- **Star schema** – analytical data model
- **Field parameters** – dynamic metric selection in dashboards

---

## Dataset

The dataset covers advertising campaign data from Facebook and Instagram, including campaign information, ads, users, targeting parameters, and interaction events.

### Coverage
- 400,000 ad events
- Multiple campaigns and ad formats
- Audience demographic and targeting information
- Event types: impression, click, share, comment, purchase

### Key Data Categories

| Category | Examples |
|---|---|
| Campaign | Campaign ID, Budget, Duration, Start Date, End Date |
| Advertisement | Ad ID, Platform, Ad Type, Target Age, Target Gender |
| Audience | Age, Gender, Country, Location, Interests |
| Ad Events | Impression, Click, Share, Comment, Purchase |
| Targeting | Target Age Group, Target Gender, Target Interests |
| Time | Event Date, Hour, Day of Week, Time of Day |

---

## Data Model

The project uses a star schema centered on the `ad_events` fact table.

- `campaigns` – campaign-level information and budget
- `ads` – advertisement and targeting configuration
- `users` – audience demographic and geographic information
- `ad_events` – user interaction events (400K rows)
- `Calendar Table` – date dimension for time-based analysis

**Relationships:** `campaigns` (1) → `ads` (*) on `campaign_id`; `ads` (1) → `ad_events` (*) on `ad_id`; `users` (1) → `ad_events` (*) on `user_id`; `Calendar Table` (1) → `ad_events` (*) on `Date` = `Event Date`.

<img width="1218" height="656" alt="ER diagram" src="https://github.com/user-attachments/assets/92ed2ddb-841f-4575-895d-0f03fdb00954" />

---

## Data Preparation & Modeling

Raw tables (campaigns, ads, users, ad_events) were loaded and shaped in Power Query:

- Split the `timestamp` field into Event Date and Event Hour.
- Built a dedicated Calendar table and related it to `ad_events[Event Date]`.
- Prepared categorical fields for filtering and visualization.
- Added an `Is_Target_Match` indicator to evaluate targeting accuracy.

A Power BI field parameter ("Select Dynamic Measure") lets the viewer switch between Impressions, Clicks, Shares, Comments, and Engagements on Dashboard 1 without duplicating visuals.

---

## Key DAX Measures

```DAX
Impressions = COUNTROWS(FILTER(ad_events, ad_events[event_type] = "Impression"))
Clicks = COUNTROWS(FILTER(ad_events, ad_events[event_type] = "Click"))
Purchases = COUNTROWS(FILTER(ad_events, ad_events[event_type] = "Purchase"))
Engagements = [Shares] + [Clicks] + [Comments]

CTR = DIVIDE([Clicks], [Impressions], 0)
Engagement Rate = DIVIDE([Engagements], [Impressions], 0)
Conversion Rate = DIVIDE([Purchases], [Clicks], 0)
Purchase Rate = DIVIDE([Purchases], [Impressions], 0)

CPA = DIVIDE([Total Budget], [Purchases], 0)
CPC = DIVIDE([Total Budget], [Clicks], 0)
CPM = DIVIDE([Total Budget], [Impressions], 0) * 1000

Targeting Accuracy Rate = 
DIVIDE(
    CALCULATE(COUNTROWS(ad_events), ad_events[Is_Target_Match] = 1),
    COUNTROWS(ad_events),
    0
)
```

---

## Key Insights

### Overall Marketing Funnel

- 339.8K impressions → 40.1K clicks (11.79% CTR) → 46.1K engagements → 2.0K purchases (0.60% purchase rate, 5.07% conversion rate).

Click-through and engagement are both strong, but only a small fraction of impressions ultimately convert to a purchase — evaluating the full funnel matters more than any single top-of-funnel metric.

### Ad Format Performance

**Engagement:**
- Video: 14.12% engagement rate, 12.20% CTR — highest of any format, despite the lowest impression volume (19.1K).
- Stories: strongest reach at ~36.9K impressions.

**Conversion:**
- Carousel: 5.38% conversion rate (highest)
- Stories: 5.19%
- Image: 5.04%
- Video: 4.50% (lowest, despite leading on engagement)

Video attracts attention most effectively; Carousel converts that attention into purchases most efficiently — the two aren't the same thing.

### Audience Insights

- Female users: ~41% of identified impressions; male users: ~23%; ~36% fall under unspecified/broad gender targeting.
- Impressions concentrate among users in their late 20s to mid-30s.
- Top interest categories by purchase volume: art, technology, gaming, followed by travel, lifestyle, fitness, and sports.

### Targeting Accuracy

Across all ad events, only 5.48% reached users who actually matched the ad's intended targeting criteria (Targeting Accuracy Rate), pointing to a meaningful gap between intended and actual audience delivery.

Breaking impressions down by targeted vs. actual age group also shows a recurring skew toward the 35–44 bracket regardless of which age group was targeted — a pattern worth validating further at the raw data level (e.g. confirming `age_group` and `user_age` line up consistently per user) before treating it as a confirmed delivery bias.

### Engagement vs. Conversion

Campaign-level data shows no clear linear relationship between engagement rate and conversion rate — campaigns with similar engagement land on very different conversion outcomes. High engagement isn't a reliable stand-in for conversion, so both should be tracked together, not one as a proxy for the other.

### Budget & Cost Efficiency

Total budget across campaigns: ~$2.5M.

- Campaign_17_Launch: lowest CPA (~$0.95K) and CPC (~$52.66) among top campaigns.
- Campaign_41_Winter: CPA ~$2.43K and CPC ~$140.86 on a similar-sized budget — roughly 3x less efficient than Campaign_17_Launch.
- Portfolio averages: CPA $1.25K, CPC $63.27, CPM $7.46K — useful benchmarks for flagging under/over-performing campaigns.

---

## Business Recommendations

**Use different creative strategies by objective.** Video works best for attention and engagement; Carousel converts clicks into purchases more efficiently; Stories offer strong reach with competitive conversion. Matching format to campaign goal (awareness vs. conversion) rather than reusing one format everywhere would likely improve results.

**Investigate the targeting mismatch.** The 35–44 concentration across every targeted age group points to a delivery issue worth digging into at the platform or campaign level — comparing targeting accuracy against audience size, budget, and conversion by segment.

**Optimize toward conversion, not just engagement.** Since engagement and conversion don't move together, campaign evaluation should weigh CPA, CPC, conversion rate, and purchase rate alongside engagement metrics rather than in place of them.

---

## Dashboard

### Dashboard 1 — Overview
Overall funnel view: impressions, clicks, engagements, purchases, CTR, engagement rate, conversion rate, purchase rate.

<img width="1199" height="696" alt="Dashboard 1 Overview" src="https://github.com/user-attachments/assets/944de316-f062-4ed0-8887-5e59a18319da" />

### Dashboard 2 — Performance Trends & Seasonality
Performance by time, day of week, hour, platform, and ad format.

<img width="1204" height="705" alt="Dashboard 2 Performance Trends" src="https://github.com/user-attachments/assets/af7b614b-0c1f-4378-875c-76209cadeb98" />

### Dashboard 3 — Audience Insights
Age, gender, location, interests, and targeting accuracy — actual vs. intended audience.

<img width="1200" height="685" alt="Dashboard 3 Audience Insights" src="https://github.com/user-attachments/assets/532e0046-929c-4b51-a817-e61123eefc8d" />

### Dashboard 4 — Budget & Cost Efficiency
Campaign budget, CPA, CPC, CPM, conversion rate, and campaign-level efficiency.

<img width="1194" height="688" alt="Dashboard 4 Budget Cost Efficiency" src="https://github.com/user-attachments/assets/afb38787-bc03-4f02-b54e-67ce35587923" />
