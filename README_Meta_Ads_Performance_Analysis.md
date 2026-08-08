# 📊 Meta Ads Performance Analysis | Power BI

An interactive Power BI project analyzing advertising performance across **Facebook and Instagram** to evaluate campaign effectiveness, audience targeting, ad format performance, and budget efficiency.

The project transforms **400,000 ad events** into actionable marketing insights using data modeling, Power Query, DAX, and interactive dashboard design.

---

## 📌 Project Overview

Digital advertising performance depends on multiple factors, including platform, campaign, ad format, audience characteristics, targeting accuracy, and budget allocation.

This project analyzes advertising data from **Facebook and Instagram** to understand how campaigns perform throughout the marketing funnel:

**Impressions → Clicks → Engagement → Purchases**

The analysis focuses on four key areas:

1. Overall campaign and funnel performance
2. Performance trends and ad format effectiveness
3. Audience and targeting accuracy
4. Budget allocation and cost efficiency

---

## 🎯 Project Objectives

The main objectives of this project are to:

- Compare advertising performance between **Facebook and Instagram**.
- Evaluate campaign performance across the marketing funnel.
- Identify high-performing ad formats.
- Analyze audience demographics and targeting accuracy.
- Examine the relationship between engagement and conversion.
- Evaluate campaign budget allocation and cost efficiency.
- Identify opportunities for improving advertising performance.

### Key Business Questions

- Which platform performs better across key advertising KPIs?
- How efficiently do impressions convert into clicks and purchases?
- Which ad formats generate the strongest engagement and conversion?
- Which audience segments generate the most purchases?
- How accurately does actual audience delivery match targeting parameters?
- Which campaigns deliver purchases at the lowest cost?
- Does higher engagement necessarily lead to higher conversion?

---

## 🛠️ Tools & Technologies

- **Power BI** – Dashboard development, data modeling and visualization
- **Power Query** – Data preparation and transformation
- **DAX** – KPI, funnel and cost-efficiency calculations
- **Star Schema** – Analytical data modeling
- **Field Parameters** – Dynamic metric selection in dashboards
- **GitHub** – Project documentation and portfolio presentation

---

## 📊 Dataset

The dataset contains advertising campaign data from **Facebook and Instagram**, including campaign information, advertisements, users, targeting parameters, and user interaction events.

### Dataset Coverage

- **400,000 ad events**
- **2 advertising platforms:** Facebook and Instagram
- Multiple campaigns and ad formats
- Audience demographic and targeting information
- Events including impressions, clicks, shares, comments, and purchases

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

## 🧩 Data Model

The project uses a **star-schema-based analytical model** centered on the `ad_events` fact table.

### Core Tables

- `campaigns` – campaign-level information and budget
- `ads` – advertisement and targeting configuration
- `users` – audience demographic and geographic information
- `ad_events` – user interaction events
- `Calendar` – date dimension for time-based analysis

### Relationships
<img width="1218" height="656" alt="image" src="https://github.com/user-attachments/assets/92ed2ddb-841f-4575-895d-0f03fdb00954" />

```

## 🧹 Data Preparation & Modeling

### Data Preparation

The raw datasets were loaded and transformed using **Power Query**.

Key preparation steps included:

- Loading campaign, advertisement, user, and event data.
- Reviewing data types and field consistency.
- Splitting the `timestamp` field into Event Date and Event Hour.
- Creating a dedicated Calendar table.
- Preparing categorical fields for filtering and visualization.
- Creating the `Is_Target_Match` indicator to evaluate targeting accuracy.

### Dynamic Measure Selection

A **Power BI Field Parameter** was created to allow users to switch between metrics such as:

- Impressions
- Clicks
- Shares
- Comments
- Engagements

without creating separate visuals for each metric.

---

# 📐 Key DAX Measures

The project uses DAX to calculate marketing funnel, engagement, conversion, and cost-efficiency metrics.

### Impressions

```DAX
Impressions =
COUNTROWS(
    FILTER(
        ad_events,
        ad_events[event_type] = "Impression"
    )
)
```

### Clicks

```DAX
Clicks =
COUNTROWS(
    FILTER(
        ad_events,
        ad_events[event_type] = "Click"
    )
)
```

### Purchases

```DAX
Purchases =
COUNTROWS(
    FILTER(
        ad_events,
        ad_events[event_type] = "Purchase"
    )
)
```

### Engagements

```DAX
Engagements =
[Shares] + [Clicks] + [Comments]
```

### CTR

```DAX
CTR =
DIVIDE(
    [Clicks],
    [Impressions],
    0
)
```

### Engagement Rate

```DAX
Engagement Rate =
DIVIDE(
    [Engagements],
    [Impressions],
    0
)
```

### Conversion Rate

```DAX
Conversion Rate =
DIVIDE(
    [Purchases],
    [Clicks],
    0
)
```

### Purchase Rate

```DAX
Purchase Rate =
DIVIDE(
    [Purchases],
    [Impressions],
    0
)
```

### CPA

```DAX
CPA =
DIVIDE(
    [Total Budget],
    [Purchases],
    0
)
```

### CPC

```DAX
CPC =
DIVIDE(
    [Total Budget],
    [Clicks],
    0
)
```

### CPM

```DAX
CPM =
DIVIDE(
    [Total Budget],
    [Impressions],
    0
) * 1000
```

### Targeting Accuracy Rate

```DAX
Targeting Accuracy Rate =
DIVIDE(
    CALCULATE(
        COUNTROWS(ad_events),
        ad_events[Is_Target_Match] = 1
    ),
    COUNTROWS(ad_events),
    0
)
```

---

# 📈 Key Insights

## 1. Overall Marketing Funnel

Across all campaigns:

- **339.8K impressions**
- **40.1K clicks**
- **11.79% CTR**
- **46.1K engagements**
- **2.0K purchases**
- **0.60% Purchase Rate**
- **5.07% Conversion Rate**

The funnel shows that the campaigns generated strong click-through and engagement levels, but the proportion of impressions that ultimately resulted in purchases was much lower.

This highlights the importance of evaluating the full funnel rather than relying on engagement metrics alone.

---

## 2. Facebook vs Instagram

The project compares Facebook and Instagram across key performance indicators including:

- Impressions
- Clicks
- CTR
- Engagement Rate
- Conversion Rate
- Purchase Rate
- CPC
- CPA

The platform comparison helps identify whether differences in reach, engagement, and conversion justify different budget allocation strategies.

> Platform performance should be evaluated using multiple KPIs rather than a single metric, since a platform may generate higher engagement without necessarily producing better conversion efficiency.

---

## 3. Ad Format Performance

Different ad formats show different strengths.

### Engagement

- **Video** achieved the highest Engagement Rate at **14.12%**.
- Video also achieved the highest CTR at **12.20%** despite having the lowest impression volume among the four major formats.
- Stories generated the highest reach with approximately **36.9K impressions**.

### Conversion

- **Carousel** achieved the highest Conversion Rate at **5.38%**.
- Stories followed at **5.19%**.
- Image achieved **5.04%**.
- Video recorded **4.50%**.

This suggests that Video is particularly effective at attracting attention, while Carousel appears more efficient at converting clicks into purchases.

---

## 4. Audience Insights

### Demographics

- Female users accounted for approximately **41% of identified impressions**.
- Male users accounted for approximately **23%**.
- Around **36%** of impressions were associated with unspecified or broad gender targeting.
- Impressions were concentrated among users in their late 20s to mid-30s.

### Purchase-driving Interests

The leading interest categories by purchase volume were:

1. Art
2. Technology
3. Gaming
4. Travel
5. Lifestyle
6. Fitness
7. Sports

Art, technology, and gaming audiences generated the highest purchase volume in the dataset.

---

## 5. Targeting Accuracy

The analysis reveals a notable mismatch between intended and actual audience delivery.

Regardless of the targeted age group, the actual audience reached showed a strong concentration in the **35–44 age group**.

For example:

| Target Age Group | Actual Highest-Reach Group |
|---|---|
| 18–24 | 35–44 |
| 25–34 | 35–44 |
| 35–44 | 35–44 |

This indicates that actual delivery does not always closely match campaign targeting parameters.

The result suggests that targeting accuracy should be monitored alongside performance metrics when evaluating campaign effectiveness.

---

## 6. Engagement vs Conversion

Campaign-level analysis shows **no strong linear relationship** between Engagement Rate and Conversion Rate.

Some campaigns with similar engagement rates achieve significantly different conversion outcomes.

This suggests that:

> **High engagement does not necessarily mean high conversion.**

Therefore, campaign evaluation should combine engagement metrics with conversion and cost-efficiency metrics.

---

## 7. Budget & Cost Efficiency

The dataset contains approximately **$2.5M in total campaign budget**.

### Campaign Efficiency

- **Campaign_17_Launch** recorded the lowest CPA among the top campaigns at approximately **$0.95K**.
- It also recorded the lowest CPC at approximately **$52.66**.
- **Campaign_41_Winter** recorded a CPA of approximately **$2.43K** and CPC of **$140.86** despite having a similar budget size.

This represents a substantial efficiency gap between campaigns with similar budget levels.

### Overall Cost Benchmarks

Across the analyzed campaigns:

- **Average CPA:** $1.25K
- **Average CPC:** $63.27
- **Average CPM:** $7.46K

These benchmarks can be used to identify campaigns that significantly underperform or outperform the overall portfolio.

---

# 💡 Business Recommendations

Based on the analysis, several potential actions can be considered:

### 1. Evaluate platform allocation using multiple KPIs

Facebook and Instagram should not be compared using reach or engagement alone.

Budget allocation should consider:

- CTR
- Conversion Rate
- CPA
- CPC
- Purchase Rate

---

### 2. Use different creative strategies by objective

The results suggest different ad formats have different strengths:

- **Video** → strong engagement and attention
- **Carousel** → stronger click-to-purchase conversion
- **Stories** → strong reach and competitive conversion

Therefore, creative formats can be aligned with different campaign objectives rather than using one format for every stage of the funnel.

---

### 3. Investigate targeting mismatch

The concentration of actual delivery in the 35–44 age group across multiple targeting groups suggests that targeting settings and actual delivery should be monitored more closely.

Further analysis could examine:

- Platform-level targeting behavior
- Campaign-level targeting accuracy
- Audience size
- Budget
- Conversion performance by targeting segment

---

### 4. Optimize toward conversion efficiency

Because engagement does not show a strong linear relationship with conversion, campaigns should not be optimized solely for engagement.

CPA, CPC, Conversion Rate, and Purchase Rate should be monitored together when evaluating campaign efficiency.

---

# 📊 Dashboard

The project contains four interactive dashboards.

## Dashboard 1 — Overview

Provides an overall view of campaign performance and the marketing funnel.

Key metrics include:

- Impressions
- Clicks
- Engagements
- Purchases
- CTR
- Engagement Rate
- Conversion Rate
- Purchase Rate

<img width="1199" height="696" alt="image" src="https://github.com/user-attachments/assets/944de316-f062-4ed0-8887-5e59a18319da" />

---

## Dashboard 2 — Performance Trends & Seasonality

Analyzes campaign performance across:

- Time
- Day of week
- Hour
- Ad platform
- Ad format
- Engagement and conversion metrics

<img width="1204" height="705" alt="image" src="https://github.com/user-attachments/assets/af7b614b-0c1f-4378-875c-76209cadeb98" />

---

## Dashboard 3 — Audience Insights

Analyzes:

- Age
- Gender
- Location
- Interests
- Targeting accuracy
- Actual vs intended audience

<img width="1200" height="685" alt="image" src="https://github.com/user-attachments/assets/532e0046-929c-4b51-a817-e61123eefc8d" />

---

## Dashboard 4 — Budget & Cost Efficiency

Analyzes:

- Campaign budget
- CPA
- CPC
- CPM
- Conversion Rate
- Campaign-level efficiency

<img width="1194" height="688" alt="image" src="https://github.com/user-attachments/assets/afb38787-bc03-4f02-b54e-67ce35587923" />

---

# 🎯 Skills Demonstrated

- Marketing funnel analysis
- Facebook & Instagram performance comparison
- Data cleaning and transformation
- Data modeling using star schema
- DAX calculations
- KPI development
- Campaign performance analysis
- Audience segmentation
- Targeting analysis
- Cost-efficiency analysis
- Power BI dashboard development
- Data visualization
- Insight generation
- Data storytelling
- Business recommendations

---

# 📌 Conclusion

This project demonstrates an end-to-end marketing analytics workflow, from data preparation and modeling to KPI development, campaign analysis, visualization, and business recommendations.

The analysis goes beyond measuring advertising reach and engagement by examining the complete marketing funnel and connecting campaign performance with audience targeting and cost efficiency.

It demonstrates how Power BI can be used to transform advertising event data into structured insights that support data-driven marketing decisions.
