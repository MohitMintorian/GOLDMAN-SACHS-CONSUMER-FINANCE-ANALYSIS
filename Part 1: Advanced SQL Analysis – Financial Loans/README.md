Jaisa `image_1d7aa5.png` mein copy icon dikhaya gaya hai, waisa button laane ke liye maine poore README text ko ek single code block ke andar daal diya hai. Ab aap box ke top-right corner par click karke ek baar mein poora code perfectly copy kar sakte hain:

```markdown
<div align="center">
  
  <!-- Animated Typing Title -->
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=30&pause=1000&color=005C84&center=true&vCenter=true&width=800&lines=Global+FinBank+-+Financial+Loans+Analysis;Advanced+SQL+Capstone+Project;Portfolio+Risk%2C+Growth+%26+Performance+Audit" alt="Typing SVG" />
  
  <!-- Tech Stack Badges -->
  <br><br>
  <img src="https://img.shields.io/badge/MySQL-00000F?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas" />
  <img src="https://img.shields.io/badge/Financial_Analytics-FF6F00?style=for-the-badge&logo=google-analytics&logoColor=white" alt="Analytics" />
  <br><br>
</div>

## 🎯 Project Objective
Extract critical business insights from Global FinBank’s lending operations using **Advanced SQL** (CTEs, Window Functions, Data Binning, and Proxy Logic). This analysis helps bank leadership make data-driven decisions on **risk management, portfolio growth, and approval efficiency**.

<details>
  <summary><b>✨ Click Here to see the Technical Architecture & Ingestion Setup</b></summary>
  <br/>
  
  - 🗄️ **Database Management System:** MySQL Server (`project_capstone`)
  - 🐍 **Automated Data Ingestion:** Python script using Pandas batch processing (`chunksize=10000`) and SQLAlchemy to load massive datasets safely.
  - 🧠 **Advanced SQL Techniques:** CTEs, `LAG()` Window Functions, Multi-condition `CASE WHEN` logic, Data Binning, and Database Indexing for optimization.
  - 📊 **Reporting & Deliverables:** Comprehensive Master `.sql` script and Executive Presentation.
</details>

<br>

## 📂 Repository Structure
```text
📦 GOLDMAN-SACHS-CONSUMER-FINANCE-ANALYSIS
 ┣ 📂 01_Data_Ingestion      # Python script (csv_to_sql.py) for batch loading
 ┣ 📂 02_SQL_Analysis        # Master SQL script (Q1 to Q10) & Indexing script
 ┗ 📂 03_Visualizations      # Presentation Deck & Analysis Screenshots

```

## 🚀 Project Workflow: How I Built This

**Step 1: Data Ingestion & Pipeline (Python)**

> 📥 Received the raw financial dataset containing over **270,000 records**. Wrote a custom Python script (`csv_to_sql.py`) using `pandas` and `sqlalchemy`. Processed and loaded the massive dataset into MySQL using batch processing (`chunksize=10000`) to prevent memory overload and database crashes.

**Step 2: Database Setup & Optimization (MySQL)**

> 🗄️ Created the relational database (`project_capstone`) and defined schemas for `customer`, `loan`, and `state_region` tables. Applied query performance tuning by creating **Indexes** on high-traffic columns to ensure sub-second query execution.

**Step 3: Advanced SQL Analytics**

> ⚙️ Tackled 10 complex business problems focused on risk and portfolio health. Applied advanced logic: **CTEs** for income binning, **LAG() Window Functions** for YoY growth, and **CASE WHEN** statements to build proxy metrics (like DTI ratios and missed payment estimators).

**Step 4: Business Storytelling & Presentation**

> 📈 Translated raw SQL outputs into actionable business insights. Structured a professional executive presentation (via Gamma AI & Power BI) to effectively communicate findings (regional pricing, high-risk flags, loan success rates) to stakeholders.

## 📊 Advanced SQL Analysis Highlights

*Click on any question below to see the business logic and key analytical approach:*

**Business Logic:** Used SQL Window Functions to calculate YoY growth percentages to track automotive portfolio expansion.

* **Concept Used:** `LAG() OVER (PARTITION BY purpose ORDER BY issue_year)`
* **Insight:** First-year NULLs were logically handled and justified, showing a clear growth trajectory in the auto-loan sector.

**Business Logic:** Identified highly vulnerable accounts by combining credit grades with Debt-to-Income indicators.

* **Concept Used:** `CASE WHEN` logic mapping `loan_amount > 40% annual_inc` along with poor bank grades (E, F, G).
* **Insight:** Created clear risk brackets for the collections and underwriting teams to monitor.

**Business Logic:** Segmented the customer base to see which income groups had the highest default rates.

* **Concept Used:** Data Binning using `CASE WHEN` to group annual income into 4 tiers (Low, Lower-Middle, Upper-Middle, High), followed by aggregated default percentages.

**Business Logic:** Mapped Days Past Due (DPD) status into actionable EMI estimates.

* **Concept Used:** Mapped statuses like 'Late 16-30 days' or 'Default' to estimated missed EMIs (1 EMI, 2-4 EMIs, 5+ EMIs) providing immediate context for the recovery team.

## ⚡ Performance Optimization

To maintain sub-second query execution on 270k+ rows across 6 tables, database indexes were created on heavily joined and filtered predicates:

```sql
-- Database Optimization for Faster Query Execution
CREATE INDEX idx_loan_status ON loan(loan_status);
CREATE INDEX idx_customer_id ON loan(customer_id);
CREATE INDEX idx_issue_year ON loan(issue_year);

```

---
