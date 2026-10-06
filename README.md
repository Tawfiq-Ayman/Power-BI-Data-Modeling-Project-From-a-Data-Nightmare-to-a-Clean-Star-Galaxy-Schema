📊 Power BI Data Modeling Project — From a Data Nightmare to a Clean Star/Galaxy Schema

A full end-to-end **Power BI data modeling portfolio project**, where a chaotic, messy, 23-table raw dataset was transformed into a clean, well-structured, high-performing **Star/Galaxy Schema** data model — following real-world dimensional modeling best practices.

This project was built as part of the **[Power BI Ultimate Course](https://www.youtube.com/@DataWithBaraa)** by **Baraa Khatib Salkini (Data With Baraa)**.

---

## 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Before vs. After](#-before-vs-after)
- [The Problem: A Data Modeling Nightmare](#-the-problem-a-data-modeling-nightmare)
- [The Approach](#-the-approach)
- [Key Concepts & Techniques Applied](#-key-concepts--techniques-applied)
- [Naming Conventions & Standards](#-naming-conventions--standards)
- [Project Files](#-project-files)
- [Skills I Gained From This Project](#-skills-i-gained-from-this-project)
- [Credits](#-credits)
- [Connect With Me](#-connect-with-me)

---

## 🧭 Project Overview

The goal of this project was to take a **raw, disorganized, real-world-style dataset** (the kind you'd actually find coming out of OLTP/operational systems) and rebuild it from the ground up into a **clean, scalable, business-ready data model** inside Power BI — one that is easy to understand, easy to maintain, fast to query, and accurate to report from.

The project covers the **entire data modeling lifecycle**:
- Exploring and understanding a messy raw dataset
- Identifying **Fact** vs. **Dimension** tables based on business logic (not just data types)
- Building clean, conformed **Dimension tables**
- Building accurate **Fact tables** at the correct **grain**
- Connecting multiple fact tables through **shared/bridge dimensions**
- Handling advanced real-world modeling challenges (ambiguous relationships, role-playing dimensions, junk dimensions, factless facts, accumulating snapshots)
- Implementing **Row-Level Security (RLS)** — both static and dynamic
- Building a proper **Date dimension** and a centralized **Measures table**

---

## 🖼️ Before vs. After

### ❌ Before — "Chaotic Data Modeling"
A loosely connected, inconsistent, and confusing collection of raw tables with unclear relationships, inconsistent naming, duplicated logic, and no clear Fact/Dimension separation.

![Chaotic Data Modeling](<img width="1141" height="715" alt="Chaotic Data Modeling" src="https://github.com/user-attachments/assets/1c9bade2-26a7-48dd-a907-15fde149b8c4" />#)

### ✅ After — "Clean Data Modeling"
A fully restructured **Star/Galaxy Schema**, with clearly separated Fact and Dimension tables (prefixed `fact_` / `dim_`), shared dimensions connecting multiple fact tables, a dedicated Date table, a centralized Measures table, and a security table for Row-Level Security.

![Clean Data Modeling](./images/Clean_Data_Modeling.png)

---

## 😵 The Problem: A Data Modeling Nightmare

The raw dataset (see [`dataset.xlsx`](./data/dataset.xlsx) and the unmodeled [`dataset.pbix`](./data/dataset.pbix)) arrived in a state very similar to what you'd get directly from operational/transactional systems:

- **23 loosely related tables** with inconsistent, unclear naming (e.g. `ORDERS_2025`, `ORDERS_2026`, `CUST_MASTER`, `customer_contacts`, `campaign_skus`, `CAMPAIGN_LOG`, `invoice_lines`, `order_line_items`, `Sheet1`, etc.)
- No clear distinction between **Facts** (business events/transactions) and **Dimensions** (descriptive context)
- Data **split across years** (e.g. separate orders tables per year) instead of being unified
- **Header/Detail (Header/Lines)** patterns copied directly from source systems, leading to the temptation of creating two separate, wrongly-grained fact tables instead of one correctly grained fact table
- Multiple fact-like tables (Sales, Inventory, Campaign Spend, Sales Targets, Order Process) with **no common way to connect them**, risking many-to-many relationships and inaccurate cross-filtering
- No dedicated Date table, no centralized measures, and no security layer

This exact kind of mess is what real-world Business Intelligence/Data Analyst work actually looks like — and solving it is the entire point of this project.

---

## 🛠️ The Approach

The project was executed in structured phases:

1. **Explore & Understand** — Opened every raw table, understood what each column represented, and identified which tables were truly *facts* (things that happen — orders, sales, spend, shipments) and which were *dimensions* (descriptive context — customers, products, campaigns, geography) — based on **business meaning**, not just on whether a column held text or numbers.

2. **Build Clean Dimensions** — Consolidated and cleaned the dimension tables using Power Query: merging duplicate/related tables, fixing data types, trimming and standardizing text, removing duplicates, handling missing values, splitting/merging/extracting columns as needed, and producing single, authoritative dimension tables (`dim_customer`, `dim_product`, `dim_campaign`, `dim_geo`, `dim_order_flag`, etc.).

3. **Handle the Header/Detail Pattern Correctly** — Recognized the classic trap of tables like `invoice_lines` / `INVOICES` or `order_line_items` / `ORDERS`, and instead of building two disconnected facts, built the core fact table from the **finest grain (the detail/line level)**, using the header table only to enrich context — protecting the numbers by validating totals after every merge step.

4. **Build Fact Tables at the Correct Grain** — Before aggregating anything, explicitly defined **what one row represents** in each fact table (the *Grain*), which determined whether `SUM`, `COUNT`, `DISTINCTCOUNT`, or `MAX` was the correct aggregation to use. Result: `fact_sales`, `fact_inventory`, `fact_campaign_spend`, `fact_sales_targets`, `fact_order_process`, `fact_promotion_coverage`.

5. **Connect Multiple Fact Tables the Right Way** — Instead of ever connecting a fact table directly to another fact table (which breaks the model and causes ambiguous/incorrect filtering), connected all fact tables through **shared/bridge/conformed dimensions**.

6. **Polish & Finalize the Model** — Added a `CALENDARAUTO()`-based Date dimension that automatically expands with the data, built a centralized `_Measures` table holding core DAX measures, applied consistent naming conventions across the entire model, and implemented Row-Level Security.

7. **Validate** — Re-checked baseline totals and row counts after every transformation step to make sure no numbers were silently broken during merges, joins, or filtering.

---

## 🧩 Key Concepts & Techniques Applied

### Power Query / ETL
- Data cleaning: `Trim`, `Capitalize Each Word`, `Replace Values`, `Replace Errors`, handling missing/null values
- `Remove Duplicates`, `Group By`
- `Split Column` (by delimiter, into rows or columns), `Merge Columns`, `Extract` vs. `Split`
- `Merge Queries` (Inner, Left Outer, Right Outer, Full Outer, Left Anti, Right Anti joins) vs. `Append Queries`
- `Pivot` / `Unpivot Columns`
- `Reference` vs. `Duplicate` queries (understanding query lineage and dependency chains)
- Organizing the Query Editor using query groups/folders, and controlling `Enable Load`

### Data Modeling
- **Star Schema** vs. **Snowflake Schema** vs. **Galaxy Schema** (multiple fact tables sharing dimensions)
- **Fact tables** vs. **Dimension tables** — distinguished by business role, not data type
- **Cardinality**: One-to-One, One-to-Many, Many-to-Many
- **Filter direction**: Single vs. Both, and how it causes ambiguity/filter loops across the model
- **Active vs. Inactive relationships**, activated on demand using `USERELATIONSHIP()`
- **Grain** — explicitly defining what a single row in a fact table represents, before writing any aggregation
- **Junk Dimension** — bundling multiple unrelated, low-cardinality flag/attribute columns into a single small dimension table instead of scattering them across the model
- **Role-Playing Dimension** — a single dimension (like Date) connected to a fact table through multiple relationships (e.g., Order Date, Ship Date), with one relationship active and the others activated inside measures via `USERELATIONSHIP()`
- **Factless Fact Table** — a bridge table holding only keys (no measures), used to represent relationships/coverage (e.g., `fact_promotion_coverage`)
- **Accumulating Snapshot Fact** — a single row per process instance that gets updated across multiple milestone dates throughout a multi-step business process (used in `fact_order_process`)
- **Shared / Bridge / Conformed Dimension** — the correct and only way to connect two fact tables that have different grains, instead of ever joining fact tables directly to each other

### Row-Level Security (RLS)
- The distinction between **Table-level**, **Column-level**, and **Row-level** security
- **Static RLS** — creating one role per value and manually assigning users to roles inside the Power BI Service
- **Dynamic RLS** — a single role using a DAX filter expression built with `USERPRINCIPALNAME()`, combined with `CALCULATETABLE`, `VALUES`, `FILTER`, and the `IN` operator against a dedicated `security` table
- The critical importance of **filter direction** for RLS to propagate correctly across the whole model
- Using the **"View As"** feature to test roles without publishing to the Service

### DAX
- `CALENDARAUTO()` to auto-generate a Date dimension that automatically expands as new data is added
- A centralized `_Measures` ("fake"/disconnected) table to organize core measures, such as:
  - `Total Sales` (`SUM`)
  - `Total Orders` (`DISTINCTCOUNT`)
  - `Total Active Customers` vs. `Total Customers`
  - `Average Order to Pay` (`DATEDIFF`)

---

## 🏷️ Naming Conventions & Standards

To keep the model consistent, readable, and maintainable, the following standards were applied across the entire project:

- `snake_case` naming for all tables and columns
- `fact_` prefix for all fact tables, `dim_` prefix for all dimension tables
- `_key` suffix for all surrogate keys used in relationships
- Clear, friendly, business-readable names instead of raw source-system column names
- **Single point of truth** principle — no duplicated logic or duplicated tables across the model
- Continuous validation of baseline totals/row counts after every transformation to "protect the numbers"

---

## 📁 Project Files

| File | Description |
|---|---|
| [`Power_Bi_Data_Modeling_Project.pbix`](./Power_Bi_Data_Modeling_Project.pbix) | ✅ The **final, completed** Power BI file — the clean Star/Galaxy Schema data model, fully built and ready to explore. |
| [`data/dataset.xlsx`](./data/dataset.xlsx) | 📄 The **raw, original dataset** (Excel) — the messy, un-modeled source data before any cleaning or restructuring. |
| [`data/dataset.pbix`](./data/dataset.pbix) | 📄 The **raw, starting Power BI file** — the project in its original "nightmare" state, before the data modeling process began. |
| [`images/Chaotic_Data_Modeling.png`](./images/Chaotic_Data_Modeling.png) | 🖼️ Screenshot of the original, messy data model (before). |
| [`images/Clean_Data_Modeling.png`](./images/Clean_Data_Modeling.png) | 🖼️ Screenshot of the final, clean Star/Galaxy Schema data model (after). |

> 💡 Open [`Power_Bi_Data_Modeling_Project.pbix`](./Power_Bi_Data_Modeling_Project.pbix) in Power BI Desktop to explore the final model, relationships, and measures directly.

---

## 🚀 Skills I Gained From This Project

Working through this project hands-on helped me build real, practical skills in:

- Reading a **messy, real-world dataset** and independently deciding how to structure it correctly, rather than following a pre-built model
- Correctly distinguishing **Fact tables from Dimension tables** based on business meaning
- Designing and building a **Star/Galaxy Schema** from scratch
- Making deliberate, grain-aware decisions about **cardinality and filter direction** to avoid ambiguous or incorrect relationships
- Solving advanced, realistic modeling challenges: **Junk Dimensions**, **Role-Playing Dimensions**, **Factless Fact Tables**, **Accumulating Snapshot Facts**, and **Shared/Bridge Dimensions**
- Connecting **multiple fact tables** cleanly through conformed dimensions instead of direct, incorrect fact-to-fact relationships
- Implementing both **Static** and **Dynamic Row-Level Security**
- Writing clean, consistent **DAX measures** and organizing them in a centralized measures table
- Applying consistent **naming conventions** and modeling standards across an entire project
- Validating data integrity by protecting baseline numbers throughout a multi-step transformation process

---

# 🚀 Final Takeaway

The biggest lesson from this project was that:

> **Power BI is not just about creating beautiful dashboards. The quality of the analysis depends heavily on the quality of the underlying data model.**

This project helped me move from thinking mainly about **visualization** to thinking about:

**Data → Grain → Business Process → Dimensions → Facts → Relationships → Validation → Analysis**

This is an important step forward in my journey toward becoming a **Data Analyst / BI Analyst**.

---

## 🔗 Connect With Me

- **LinkedIn:** [Add your LinkedIn profile link here]

---
