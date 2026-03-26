<div align="center">

# 🍽️ Food Trends: Understanding Customer Preferences in F&B

### A Power BI Analytics Dashboard for the Food & Beverage Industry

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![NLP](https://img.shields.io/badge/NLP-Sentiment%20Analysis-blue?style=for-the-badge)
![DAX](https://img.shields.io/badge/DAX-Measures-orange?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

*Transform raw customer feedback into strategic business intelligence using AI, NLP, and interactive dashboards.*

</div>

---

## 📋 Table of Contents

1. [Project Overview](#-project-overview)
2. [Dashboard Preview](#-dashboard-preview)
3. [Architecture](#-architecture)
4. [Module 1 — Data Collection & Cleaning](#-module-1--data-collection--cleaning)
5. [Module 2 — Sentiment Analysis](#-module-2--sentiment-analysis)
6. [Module 3 — Product Insights](#-module-3--product-insights)
7. [Module 4 — Customer Segmentation](#-module-4--customer-segmentation)
8. [Module 5 — Strategy & Innovation Dashboard](#-module-5--strategy--innovation-dashboard)
9. [DAX Measures Reference](#-dax-measures-reference)
10. [Tech Stack](#-tech-stack)
11. [Business Impact](#-business-impact)

---

## 🌟 Project Overview

The Food & Beverage industry is customer-driven and deeply competitive. Businesses receive feedback across multiple channels — reviews, social media, surveys, and ratings — yet struggle to convert that raw data into actionable intelligence.

This project delivers a **5-module Power BI analytics solution** that:

- Ingests and cleans both **structured** (surveys, sales) and **unstructured** (reviews, social) data
- Runs **AI/NLP-powered sentiment classification** on customer feedback
- Benchmarks **product performance** across taste, price, and packaging
- Identifies distinct **customer behavioral personas** through clustering
- Powers a **Strategy & Innovation Dashboard** to track trends and guide R&D

> **Core question addressed:** How can F&B organizations systematically collect, clean, analyze, and visualize customer feedback to support proactive decision-making?

---

## 📸 Dashboard Preview

### Overview — Home Dashboard
![Home Dashboard](Screenshot%202026-03-26%20194449.png)

---

### Sentiment Analysis Dashboard
![Sentiment Analysis](Screenshot%202026-03-26%20200623.png)

---

### Product Insights Dashboard
![Product Insights](Screenshot%202026-03-26%20200900.png)

---

### Customer Segmentation Dashboard
![Customer Segmentation](Screenshot%202026-03-26%20201204.png)

---

### Strategy & Innovation Dashboard
![Strategy Dashboard](Screenshot%202026-03-26%20201620.png)

---

### Executive Summary Dashboard
![Executive Summary](Screenshot%202026-03-26%20201913.png)

---

## 🏗️ Architecture

```
Raw Data Sources
       │
       ▼
┌─────────────────────────────────────┐
│  Module 1: Data Collection & ETL   │
│  • Structured: CSV / SQL / Excel    │
│  • Unstructured: Reviews / Social   │
│  • Text cleaning: tokenize/normalize│
│  • Output: unified fact tables      │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  Power BI Data Model                │
│  dim_customer • dim_product         │
│  dim_region   • fact_reviews        │
│  fact_sales   • fact_ratings        │
└──┬──────────┬──────────┬────────────┘
   │          │          │
   ▼          ▼          ▼
Module 2   Module 3   Module 4
Sentiment  Product    Customer
Analysis   Insights   Segmentation
   │          │          │
   └──────────┴──────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  Module 5: Strategy & Innovation    │
│  • Keyword trends • Opportunity map │
│  • Pre/post launch sentiment track  │
└─────────────────────────────────────┘
```

---

## 🗃️ Module 1 — Data Collection & Cleaning

### Data Sources

| Source Type | Examples | Import Method |
|---|---|---|
| **Structured** | Survey results, sales logs, ratings | Power Query / SQL / Excel |
| **Unstructured** | Product reviews, social comments | API / web scraping / CSV export |

### Unified Schema

After ingestion, all sources are merged into a single analytics layer:

```
fact_reviews
  ├── user_id          (FK → dim_customer)
  ├── product_id       (FK → dim_product)
  ├── review_text      (cleaned string)
  ├── sentiment_score  (0.00 – 1.00)
  ├── sentiment_label  (Positive / Neutral / Negative)
  ├── review_date      (date)
  └── region_id        (FK → dim_region)
```

### Text Preprocessing Pipeline

```
Raw Review Text
      │
      ▼
 Tokenization  ──► Split sentences into individual word tokens
      │
      ▼
 Normalization ──► Lowercase · remove punctuation · stemming / lemmatization
      │
      ▼
 Deduplication ──► Remove exact-match or near-duplicate review entries
      │
      ▼
 Clean Review Text  ──► Ready for sentiment scoring
```

---

## 💬 Module 2 — Sentiment Analysis

Using **Power Query AI Insights (Text Analytics)**, each review receives a sentiment score between **0** (very negative) and **1** (very positive).

### Sentiment Classification Thresholds

| Score Range | Label | Color Code |
|---|---|---|
| 0.67 – 1.00 | ✅ Positive | Green |
| 0.34 – 0.66 | ⚠️ Neutral | Amber |
| 0.00 – 0.33 | ❌ Negative | Red |

### Key DAX Measures

```dax
Positive Feedback Ratio =
DIVIDE(
    COUNTROWS(FILTER(fact_reviews, fact_reviews[sentiment_label] = "Positive")),
    COUNTROWS(fact_reviews)
)

Avg Sentiment Score =
AVERAGEX(fact_reviews, fact_reviews[sentiment_score])

Sentiment Change MoM =
VAR CurrentMonth = [Avg Sentiment Score]
VAR PreviousMonth =
    CALCULATE([Avg Sentiment Score], DATEADD(dim_date[Date], -1, MONTH))
RETURN
    DIVIDE(CurrentMonth - PreviousMonth, PreviousMonth)
```

### Dashboard Visuals

- **Stacked column chart** — sentiment distribution per product
- **Clustered bar chart** — sentiment by feature (delivery, price, packaging)
- **Filled map** — regional satisfaction heatmap
- **KPI cards** — average sentiment score, positive ratio, negative ratio
- **Line chart** — sentiment trend over time (monthly)

---

## 📦 Module 3 — Product Insights

### What This Module Answers

| Business Question | Visual Used |
|---|---|
| Which product categories have the highest average rating? | Clustered bar chart |
| How does taste / price / packaging impact satisfaction? | Radar / spider chart |
| Is there a correlation between positive sentiment and revenue? | Scatter plot with trend line |
| Which products are declining in satisfaction? | Line chart with alert markers |

### Correlation Analysis: Sales vs Feedback

```
High Sentiment + High Sales  ──► Star products (maintain & expand)
High Sentiment + Low Sales   ──► Hidden gems (increase visibility)
Low Sentiment  + High Sales  ──► At-risk products (urgent improvement needed)
Low Sentiment  + Low Sales   ──► Candidates for discontinuation
```

---

## 👥 Module 4 — Customer Segmentation

Customers are clustered into three behavioral personas using **rating frequency**, **sentiment pattern**, and **product preference** signals.

### Persona Definitions

| Persona | Behaviour | Rating Pattern | Strategy |
|---|---|---|---|
| 🌟 **Loyal Fans** | Frequent buyers, consistently positive | High frequency, high score | Loyalty rewards, early access, referral programs |
| 🔴 **Critics** | Irregular buyers, negative feedback | Low-mid frequency, low score | Targeted engagement, complaint resolution, win-back campaigns |
| 👻 **Silent Users** | Regular buyers, rarely leave feedback | Moderate frequency, no score | Incentivized review prompts, NPS surveys, personalized outreach |

### Segmentation Features Used

```
Cluster Input Features:
  • purchase_frequency     (transactions per 90 days)
  • avg_sentiment_score    (mean of all user reviews)
  • avg_rating             (mean star rating)
  • product_diversity      (distinct products purchased)
  • days_since_last_review (recency of feedback)
```

### Interactive Filters Available

- 🌍 **Region** — filter by country or city
- 🎂 **Age Group** — filter by demographic bracket
- 🛒 **Purchase Channel** — filter by online / in-store / delivery app

---

## 🚀 Module 5 — Strategy & Innovation Dashboard

### 5.1 Trending Keyword Analysis

Word frequency analysis across all reviews surfaces **emerging customer concerns** and **demand signals** in real time.

```
High-frequency positive words  ──► Reinforce in marketing messaging
High-frequency negative words  ──► Flag for product / ops teams
Emerging new keywords          ──► Early warning for trend shifts
```

**Visuals:** Word cloud · Keyword frequency bar chart · Topic trend line

### 5.2 Marketing & R&D Opportunity Map

| Opportunity Zone | Sentiment Trend | Recommended Action |
|---|---|---|
| 📈 Rising satisfaction | Improving | Double down — scale marketing |
| 📉 Falling satisfaction | Declining | Investigate root cause — prioritize fix |
| 🆕 Untapped segment | Neutral / low engagement | Launch targeted campaign |
| 💡 Feature gap detected | Mixed | Add to R&D roadmap |

### 5.3 Pre-Launch vs Post-Launch Sentiment Tracking

Monitor the impact of every product launch with before/after sentiment comparison:

```
Timeline Markers:
  ──── [Pre-Launch Window: T-30 to T-0] ──── [Launch Day] ──── [Post-Launch Window: T+1 to T+90] ────

Metrics Tracked:
  • Avg sentiment score (pre vs post)
  • Volume of reviews (pre vs post)
  • Sentiment velocity (rate of change in first 30 days)
  • Top positive / negative keywords post-launch
```

---

## 📐 DAX Measures Reference

```dax
-- Negative Feedback Ratio
Negative Feedback Ratio =
DIVIDE(
    COUNTROWS(FILTER(fact_reviews, fact_reviews[sentiment_label] = "Negative")),
    COUNTROWS(fact_reviews)
)

-- Average Rating per Category
Avg Rating by Category =
AVERAGEX(
    RELATEDTABLE(fact_ratings),
    fact_ratings[rating_value]
)

-- Sentiment Score (Post-Launch 30 Days)
Post Launch Sentiment =
CALCULATE(
    [Avg Sentiment Score],
    fact_reviews[review_date] >= [Launch Date]
        && fact_reviews[review_date] <= [Launch Date] + 30
)

-- Customer Persona Label
Persona =
SWITCH(
    TRUE(),
    [Avg Sentiment Score] >= 0.67 && [Purchase Frequency] >= 5, "Loyal Fan",
    [Avg Sentiment Score] <= 0.33, "Critic",
    "Silent User"
)
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **BI & Visualisation** | Microsoft Power BI Desktop |
| **Data Transformation** | Power Query (M language) |
| **AI / NLP** | Power Query AI Insights — Text Analytics |
| **Calculations** | DAX (Data Analysis Expressions) |
| **Structured Data** | Excel · CSV · SQL Server |
| **Unstructured Data** | REST APIs · Web scraping exports |
| **Data Modelling** | Star schema with fact and dimension tables |

---

## 💼 Business Impact

| Outcome | Benefit |
|---|---|
| 🎯 Targeted marketing | Reduce wasted spend by focusing on high-value segments |
| 🛠️ Faster product fixes | Detect negative trends before they escalate |
| 📣 Smarter launch planning | Validate demand signals before committing R&D budget |
| 🔄 Improved retention | Identify at-risk customers and re-engage proactively |
| 📊 Executive visibility | Real-time KPIs replace manual monthly reports |
| 💡 Data-driven innovation | Align new product development with actual customer demand |

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">

Made with ❤️ using Power BI | NLP | DAX

</div>
