# Cannondale Sales Intelligence Dashboard (Power BI)

A multi-page **Power BI** report that analyzes sales performance across **time, geography, products, and customers**. The dashboard supports KPI monitoring, drill-down exploration, and scenario-style analysis using a **What-If price adjustment parameter**.

---

## Pages & analysis goals

### 1) Executive Summary
**Goal:** Overall performance monitoring.
- Weekly revenue trend
- Monthly KPI cards (Revenue / Orders / Returns)
- Orders by product category (Accessories / Bikes / Clothing)
- Top products table (Orders, Revenue, Return Rate)
- Highlights: most ordered / most returned product types 

### 2) Geographic Performance
**Goal:** Compare performance by geography.
- World map by country
- Region selection buttons (e.g., Europe / North America / Pacific)

### 3) Product Performance vs Target + What-If
**Goal:** Product-level deep dive (actual vs target) + scenario testing.
- Selected product card (example: “Fender Set - Mountain”)
- Gauges: monthly Revenue / Orders / Profit vs targets
- What-if slider: **Adjusted Price (%)**
- Metric selection (Profit / Returns / Revenue / Orders / Return %) with time series

### 4) Customer Insights
**Goal:** Customer segmentation and concentration.
- Unique customers and revenue per customer trend
- Orders by income level and occupation
- Top customers table and “top customer by revenue” callout

---

## Data model (Model View)

This report uses a **star-schema style model** with two main fact tables and multiple lookup (dimension) tables:

### Fact tables
- **Sales Data** (transaction-level sales facts)
  - Keys/fields visible: `CustomerKey`, `ProductKey`, `OrderDate`, `OrderNumber`, `OrderQuantity`, `OrderItem`, etc.
- **Returns Data** (returned items facts)
  - Keys/fields visible: `ProductKey`, `ReturnDate`, `ReturnQuantity`, `TerritoryKey`, etc.

### Lookup (dimension) tables
- **Product Lookup** (product attributes such as name, description, price, SKU, color/cost, etc.)
- **Product Subcategories Lookup**
- **Product Categories Lookup**
- **Calendar Lookup** (date table with fields like Date, Month, Day Name, etc.)
- **Territory Lookup** (geography: country/region/continent + territory keys)
- **Customer Lookup** (customer attributes: income, education, occupation, etc.)

### Relationships (high level)
- Lookup tables connect **one-to-many (1:*)** into the fact tables.
- **Sales Data** links to:
  - Customer Lookup (via `CustomerKey`)
  - Product Lookup (via `ProductKey`)
  - Calendar Lookup (via `OrderDate`)
  - Territory Lookup (via `TerritoryKey`, if present in your model)
- **Returns Data** links to:
  - Product Lookup (via `ProductKey`)
  - Calendar Lookup (via `ReturnDate`)
  - Territory Lookup (via `TerritoryKey`)

> This structure enables slicing measures (Revenue/Profit/Orders/Returns) by Date, Product hierarchy (Category → Subcategory → Product), Geography, and Customer segments.

---

## Measures & parameters (technical)

### Measure Table
The model includes a dedicated **Measure Table** to store DAX measures in one place (cleaner organization and easier maintenance).

Typical measures used by the visuals include:
- Total Revenue
- Total Profit
- Total Orders
- Total Returns
- Return Rate (%)
- Revenue per Customer
- Target comparisons (Actual vs Target)
- Adjusted Profit / Adjusted Revenue (for what-if)

### What-If parameter
The model includes:
- **Adjusted Price (%)** table: provides a slider to simulate price changes.
- A supporting measure (e.g., `Adjusted profit`) uses that parameter value to compute “what if price changes by X%”.

### Dynamic metric selectors
The model includes selector tables:
- **Product Metric Selection**
- **Customer Metric Selection**

---

## Files
- `Intelligence_report.pbix` — Power BI Desktop report file 
- `Cannodale_report.pdf` — Exported PDF preview of the report 

---

## How to open and explore
1. Install **Power BI Desktop**
2. Open `Intelligence_report.pbix`
3. Use slicers/filters to interact with visuals
4. On the Product page, change **Adjusted Price (%)** to see scenario impact vs targets

