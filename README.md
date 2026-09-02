[README.md](https://github.com/user-attachments/files/31737821/README.md)
<div align="center">

# 🛒 Sales & Customer Dashboards (Dynamic)

### A Two-Page Tableau Workbook with Parameter-Driven Year-over-Year Analysis

![Tableau](https://img.shields.io/badge/Tableau-E97627?style=for-the-badge&logo=tableau&logoColor=white)
![Dynamic Parameters](https://img.shields.io/badge/Dynamic-Parameters-4B5563?style=for-the-badge)
![Data Analysis](https://img.shields.io/badge/Data-Analysis-orange?style=for-the-badge)

</div>

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Dataset](#️-dataset)
- [Dashboards](#-dashboards)
- [Dynamic Year Parameter & YoY Logic](#-dynamic-year-parameter--yoy-logic)
- [Data Model](#️-data-model)
- [Key Insights](#-key-insights)
- [Tools & Techniques](#️-tools--techniques)
- [Project Structure](#-project-structure)
- [How to Use](#-how-to-use)
- [Contact Me](#-contact-me)

---

## 📌 Overview

**Sales & Customer Dashboards (Dynamic)** is a Tableau workbook (`.twbx`) built on the classic Superstore retail dataset. It's split into two connected dashboards — **Sales** and **Customer** — that share the same navigation bar and the same trick underneath: every KPI card is **parameter-driven**, so a single Year selector recalculates Current-Year vs. Prior-Year figures, growth %, and peak highlighting across every chart at once.

Rather than static totals, the workbook is built around **178 calculated fields** — current/prior-year splits, YoY growth ratios, LOD (`FIXED`) expressions, and table calculations (`WINDOW_MAX`, `WINDOW_AVG`) that automatically flag the best-performing point on every trend line and bar chart.

---

## 🗃️ Dataset

Built on the well-known **Sample Superstore** dataset, split into four relational tables.

| Table | Rows | Description |
|---|---|---|
| `Orders.csv` | 9,994 | Every order line: order/ship date, ship mode, customer, product, sales, quantity, discount, and profit |
| `Customers.csv` | 793 | Customer ID and name |
| `Location.csv` | 632 | Postal code, city, state, region, and country |
| `Products.csv` | 1,894 | Product ID, category, sub-category, and product name |

**Verified scope of the data:**
- 📅 Orders span **2020-01-03 to 2023-12-30** — four full years, which is exactly what the Year parameter drives
- 💰 Total Sales: **$2,297,200.86** · Total Profit: **$286,397.02** · Total Quantity: **37,873 units**
- 🧾 **5,009** distinct orders from **793** customers
- 🌍 Covers all of the **United States** — 4 regions (Central, East, South, West), 49 states, 531 cities
- 🏷️ **3** product categories (Furniture, Office Supplies, Technology) across **17** sub-categories and 1,862 unique products

---

## 📊 Dashboards

### 💰 Sales Dashboard
| KPI Cards | Supporting Visuals |
|---|---|
| `KPI Sales`, `KPI Profit`, `KPI Quantity` — each with YoY % change and a color legend (`Legend KPI`) | `Weekly Trends` — weekly sales trend with the peak week auto-highlighted<br>`Subcategory Comparison` — current vs. prior year sales by sub-category, with an above/below-average marker (`Legend Subcategory`) |

### 👥 Customer Dashboard
| KPI Cards | Supporting Visuals |
|---|---|
| `KPI Customers`, `KPI Orders`, `KPI Sales Per Customers` — each with YoY % change | `Customer Distribution` — customer spread across the country<br>`Top Customers` — ranked list of highest-value customers for the selected year |

Both dashboards share the same layout skeleton — a title bar, a horizontal KPI strip, a filter panel, and a two-chart body — plus a **navigation button** (with distinct active/inactive icon states) that lets the viewer flip between the two pages without leaving the workbook.

---

## 🎛️ Dynamic Year Parameter & YoY Logic

The whole workbook pivots on a single control: **`Parameter 1`** (the selected year, defaulting to 2023). Every KPI is built from a pair of calculated fields such as:

```
CY Sales  = IF YEAR([Order Date]) = [Parameters].[Parameter 1]   THEN [Sales] END
PY Sales  = IF YEAR([Order Date]) = [Parameters].[Parameter 1]-1 THEN [Sales] END

YoY Growth % = (SUM([CY Sales]) - SUM([PY Sales])) / SUM([PY Sales])
```

The same Current-Year / Prior-Year pattern powers Profit, Quantity, Customers (via `COUNTD`), Orders, and Sales-per-Customer — meaning **changing one parameter instantly refreshes every KPI card and chart on both dashboards.**

On top of that, table calculations do the visual storytelling automatically:
- `WINDOW_MAX(...)` flags the single highest bar or trend point so it stands out without any manual formatting
- `WINDOW_AVG(...)` classifies each sub-category as **"Above"** or **"Below"** the average, feeding the `Legend Subcategory` color scheme
- A conditional marker (`⬤`) highlights weeks where sales dropped below the previous week on the `Weekly Trends` line

---

## 🗺️ Data Model

The four tables relate through simple keys, forming a clean, denormalized retail model:

```
Customers.csv ──(Customer ID)──┐
                                ├──> Orders.csv <──(Product ID)── Products.csv
Location.csv ───(Postal Code)──┘
```

- `Orders` is the fact table — every other table enriches it with descriptive attributes
- `Customers` adds the customer name behind each `Customer ID`
- `Location` adds city/state/region/country behind each `Postal Code`
- `Products` adds category/sub-category/name behind each `Product ID`

---

## 💡 Key Insights

- Four full years of data (2020–2023) let the dynamic parameter genuinely compare year-over-year performance, not just a single snapshot.
- The **Technology** and **Furniture** categories drive a disproportionate share of the **17 sub-categories**, visible instantly through the above/below-average legend on the Sales Dashboard.
- With **793 customers** generating **5,009 orders**, the average customer places roughly **6.3 orders** — the `KPI Sales Per Customers` card turns that into a trackable, year-over-year KPI.
- The peak-highlighting logic (`WINDOW_MAX`) surfaces the single best week and best sub-category automatically — no manual annotation needed as new years of data are added.

---

## 🛠️ Tools & Techniques

- **Tableau Desktop** — dashboard design, actions, and packaged workbook (`.twbx`) publishing
- **Parameters** — a single Year selector driving every dynamic calculation
- **Calculated Fields (178 total)** — Current Year / Prior Year splits, YoY growth ratios
- **LOD Expressions** — `{FIXED ...}` for calculations independent of view-level granularity
- **Table Calculations** — `WINDOW_MAX`, `WINDOW_AVG` for automatic peak and above/below-average highlighting
- **Dashboard Actions & Navigation** — button-based navigation between the Sales and Customer dashboards with active/inactive icon states

---

## 📁 Project Structure

```
Sales-Customer-Dashboards/
├── README.md
├── Sales & Customer Dashboards (Dynamic).twbx
├── Orders.csv
├── Customers.csv
├── Location.csv
└── Products.csv
```

---

## 🚀 How to Use

1. Download `Sales & Customer Dashboards (Dynamic).twbx` from this repository — it's a packaged workbook, so the data is already embedded inside it.
2. Open it in **Tableau Desktop** or **Tableau Reader** (free viewer, no license required).
3. Use the **Year parameter** in the filter panel to switch between 2020–2023 and watch every KPI and chart update.
4. Use the navigation button at the top to flip between the **Sales Dashboard** and the **Customer Dashboard**.
5. The four source CSVs are included separately for reference or if you want to rebuild the data connection from scratch.

---

## 📬 Contact Me

<div align="center">

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:salemomar676@gmail.com)
[![Portfolio](https://img.shields.io/badge/Portfolio-4B5563?style=for-the-badge)](https://gamma.app/docs/Copy-of-Brand-Partnership-Proposal-lrp9yrhau9gdpj1)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/eng-omarsalem)

</div>

---

<div align="center">

Made with 🛒 and Tableau

</div>
