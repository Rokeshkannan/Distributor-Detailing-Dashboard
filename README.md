<div align="center">

# 📊 Distributor Detailing Dashboard (DDD)

### *Built from the field. Designed for the field.*

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-70%2B%20Measures-0078D4?style=for-the-badge)
![Star Schema](https://img.shields.io/badge/Data%20Model-Star%20Schema-brightgreen?style=for-the-badge)
![FMCG](https://img.shields.io/badge/Domain-FMCG%20Field%20Ops-orange?style=for-the-badge)
![Synthetic Data](https://img.shields.io/badge/Data-Synthetic-lightgrey?style=for-the-badge)

**15 Distributors · 30 SKUs · 3 SBUs · 15 Cities · 24 Months · Tamil Nadu**

</div>

---

## 🧭 The Story Behind This Dashboard

> *"The data was there. The problem was never the data."*

I spent **1.5 years as an Area Executive at ITC Limited**, managing 11–15 distributors across the Keeranur–Sivagangai territory in Tamil Nadu. Every month, I walked into Distributor Business Reviews (DBRs) — the most important conversation in the FMCG field calendar — carrying a multiple Excel sheets and a laptop screen full of pivot tables.

Those meetings were hard. Not because the distributors were difficult. But because **I couldn't make the story clear fast enough.**

Here's what those DBRs actually felt like:

- A distributor questioned my **ROI numbers mid-meeting** — and I had to scroll through three different cells to justify a single figure while he waited, arms crossed.
- I knew we had **dead and slow-moving SKUs** sitting in his godown eating up capital — but I couldn't show *which ones, how much, since when* without digging through a pivot table on the spot.
- I'd be **flipping between multiple Excel files** — one for sales, one for inventory, one for targets — while the distributor lost interest in the conversation.
- And sometimes, even when the data was right, the **Excel data look too raw** that the distributor simply didn't trust it.

Every month, the same friction. Every month, the same thought: *there has to be a better way to walk into this room.*

**This dashboard is that better way.**

---

## 🎯 What This Dashboard Solves

The **Distributor Detailing Dashboard (DDD)** is a Power BI report built specifically for the **monthly DBR workflow** of an Area Executive or ASM in FMCG field sales.

It consolidates everything you need for a structured distributor review into a single, executive-dark themed report — sales performance, target achievement, ROI, inventory health, and locked investment — with a **single distributor slicer** that drives the entire report.

Walk in with a laptop. Open one file. Speak with confidence.

**Built for two audiences:**
1. **FMCG Sales Professionals** — AEs, ASMs, RSMs who conduct monthly distributor reviews
2. **Freshers joining as a new AE** in a territory — use this as your onboarding lens to understand how distributor performance should be read and discussed

---

## 📋 Dashboard Pages

| # | Page | What It Answers |
|---|------|----------------|
| 🏠 | **Home** | Navigation hub — entry point with key KPI tiles |
| 📈 | **Overview** | How is the territory doing overall? Revenue, targets, YoY growth |
| 💰 | **Sales Performance** | Which SKUs, SBUs, and beats are driving or dragging sales? |
| 🏦 | **Financial Health** | What is each distributor's ROI, credit utilisation, and margin? |
| 📦 | **Inventory Analytics** | What's the stock status? ABC-FSN classification, Days of Stock |
| 🔒 | **Locked Investment** | How much capital is tied up in slow-moving and critical stock? |

---

## 🗂️ Data Architecture

### Star Schema Design

```
                         ┌─────────────────┐
                         │    Dim_Date     │
                         └────────┬────────┘
                                  │
   ┌──────────────────┐  ┌────────▼────────┐  ┌──────────────┐
   │  Dim_Distributor │──│  Fact_Sales     │──│   Dim_SKU    │
   └──────────────────┘  └────────┬────────┘  └──────────────┘
                                  │
                         ┌────────▼────────┐
                         │ Fact_Inventory  │
                         └─────────────────┘
```

| Table | Rows | Columns | Description |
|-------|------|---------|-------------|
| Fact_Sales | 19,528 | 47 | Orders, SKU-level billing, margins, targets |
| Fact_Inventory | 10,800 | 36 | Monthly stock movement, aging, turnover |
| Dim_Distributor | 15 | 10 | Distributor profile, tier, zone, credit limit |
| Dim_SKU | 30 | 8 | SKU master — SBU, category, pricing |
| Dim_Date | 730 | 6 | Jan 2024 – Dec 2025 (24 months) |

---

## 🧮 DAX Highlights

**70+ measures** organised across **14 folders** — every KPI an AE needs, pre-built.

```DAX
-- Target Achievement %
Achievement % =
DIVIDE([Net Sales Value], [Target Value], 0) * 100

-- Distributor ROI
Distributor ROI % =
DIVIDE(
    [Net Sales Value] - [Total Distributor Cost],
    [Total Distributor Cost],
    0
) * 100

-- Days of Stock (key inventory health KPI)
Days of Stock =
DIVIDE(
    AVERAGE(Fact_Inventory[Closing_Stock]),
    DIVIDE(SUM(Fact_Inventory[Sold_Qty]), 30),
    0
)

-- Locked Investment (slow-moving + critical stock value)
Locked Investment =
CALCULATE(
    SUMX(
        Fact_Inventory,
        Fact_Inventory[Closing_Stock] * Fact_Inventory[Cost_Price]
    ),
    Fact_Inventory[Stock_Status] IN {"Critical", "Low"}
)

-- YoY Revenue Growth
YoY Growth % =
DIVIDE(
    [Net Sales Value] - [LY Net Sales Value],
    [LY Net Sales Value],
    0
) * 100
```

**DAX Folder Structure:**
`Sales Metrics` · `Target & Achievement` · `Inventory KPIs` · `Financial Health` · `ROI Measures` · `YoY Comparisons` · `Distributor Profile` · `Beat Analytics` · `SKU Performance` · `Time Intelligence` · `Return Analysis` · `Margin Metrics` · `Coverage Metrics` · `Locked Investment`

---

## 📁 Repository Structure

```
distributor-detailing-dashboard/
│
├── README.md                        ← You are here
├── LICENSE                          ← MIT License
├── data/
│   ├── Raw_Sales_Revenue.csv        ← 19,528 rows | Orders, billing, margins
│   ├── Raw_Inventory.csv            ← 10,800 rows | Stock movement, aging
│   └── data_dictionary.md           ← All 83 columns explained + FMCG glossary
├── dashboard/
│   └── cp1.pbix                     ← Power BI Desktop file (open this)
├── assets/
│   └── screenshots/                 ← Dashboard page screenshots
└── docs/
    └── DDD_Viva_Presentation.pptx   ← Project presentation deck
```

---

## ⚙️ How to Use

### Prerequisites
- **Power BI Desktop** (free) → [Download here](https://powerbi.microsoft.com/desktop/)

### Steps

**1. Clone or download this repo**
```bash
git clone https://github.com/Rokeshkannan/Distributor-Detailing-Dashboard.git
```
Or click **Code → Download ZIP** and extract.

**2. Open the dashboard**
```
Power BI Desktop → File → Open Report → dashboard/cp1.pbix
```

**3. Refresh data source**
```
Home → Transform Data → Data Source Settings
→ Update path to point to the data/ folder on your machine
→ Close & Apply → Refresh
```

**4. Use the distributor slicer**
Every page is controlled by a single distributor slicer on the left panel.
Select any distributor → all 6 pages update instantly.

---

## 🗺️ Territory Coverage

**15 Distributors across Tamil Nadu**

> Chennai · Coimbatore · Madurai · Tiruchirappalli · Salem · Vellore · Tirunelveli · Erode · Tiruppur · Thanjavur · Dindigul · Nagercoil · Thoothukudi · Kanchipuram · Hosur

**Zones:** North · South · Central · West
**Tiers:** Tier 1 · Tier 2 · Tier 3
**SBUs:** Personal Care · Home Care · Home Essentials
**Brands (fictional):** ZESTO · PURIS · FOAMX · CLEANOZ

---

## 📌 Dataset Disclaimer

All data in this project is **100% synthetic**, generated using Python based on realistic FMCG field operations patterns. Distributor names, outlet names, owner names, GST numbers, and brand names are entirely fictional.

The **data structure, KPI definitions, business logic, and territory design** are modelled directly on real field experience managing distributors in Tamil Nadu's FMCG ecosystem. The goal was realism in the analytical layer, not in the raw data.

---

## 💡 For New Area Executives

If you're joining as a fresher AE and taking charge of a territory for the first time — this dashboard is also for you.

Use it to understand **what questions to ask** in a DBR:
- What is this distributor's ROI, and is it healthy?
- Which SKUs are not moving, and how much capital is locked?
- Is beat coverage improving or slipping?
- Are targets being achieved at the SBU level or just inflated by one category?

The dashboard doesn't replace the conversation. It **equips you for it.**

---

## 🔗 Related Projects

| Project | Description |
|---------|-------------|
| 🤖 [FMCG Sales Forecasting System](https://github.com/Rokeshkannan/FMCG-Sales-Forecasting-System) | XGBoost + LSTM hybrid ensemble forecasting model, deployed on Streamlit Cloud |

---

## 👤 About

**Rokeshkannan**
Former Area Executive · ITC Limited · Tamil Nadu
→ Transitioning to Data Analytics / BI Analytics

**Program:** PG Program in Data Science & Analytics with Applied AI — Imarticus Learning

**Core FMCG domain knowledge:** DBR · CFC · Beat Coverage · Lines Cut · Bill Count · LY/CY · SBU Tracking · ABC-FSN Classification · Distributor ROI

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/rokeshkannan)
[![Medium](https://img.shields.io/badge/Medium-Read%20Article-000000?style=flat&logo=medium)](https://medium.com/@rokeshkannan)

---

<div align="center">

*Built not just as a portfolio project — but as the tool I wish I had in the field.*

⭐ If this helped you, consider starring the repo.

</div>
