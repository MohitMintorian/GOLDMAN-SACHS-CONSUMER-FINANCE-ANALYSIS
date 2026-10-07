<div align="center">
  <!-- Cool Data Header GIF -->
  <img src="https://media.giphy.com/media/qgQUggAC3Pfv687qPC/giphy.gif" alt="Data Analytics Header" width="100%" height="200" style="border-radius: 15px;" />
</div>

<br>

<div align="center">
  <!-- Animated Auto-Typing Title -->
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=28&duration=2000&pause=500&color=00D26A&center=true&vCenter=true&width=800&lines=Technical+Appendix+%26+Extra+Info;Data+Dictionary+%26+Assumptions;Troubleshooting+%26+Future+Scope+🚀" alt="Animated Typing SVG" />
</div>

---

<br>

### 📖 <details>
<summary><b style="font-size: 18px; cursor: pointer;">✨ 1. Data Dictionary (Click to Expand)</b></summary>
<br>

Understanding the banking terminology used in the dataset:

| Column Name | Data Type | Business Definition |
| :--- | :--- | :--- |
| `annual_inc` | Numeric | The self-reported annual income provided by the borrower during registration. |
| `dti` | Numeric | **Debt-to-Income Ratio:** Total monthly debt payments divided by gross monthly income. |
| `loan_status` | Categorical | Current status of the loan (e.g., 'Fully Paid', 'Charged Off', 'Current'). |
| `cibil_score` | Numeric | The credit score of the borrower at the time of application. |
| `delinq_2yrs` | Numeric | The number of 30+ days past-due incidences of delinquency in the past 2 years. |
</details>

<hr>

### 🧠 <details>
<summary><b style="font-size: 18px; cursor: pointer;">⚙️ 2. Core Business Assumptions</b></summary>
<br>

To perform actionable analysis, the following financial assumptions were applied to the SQL queries:
* 🚩 **Risk Flagging (DTI Proxy):** A Debt-to-Income (DTI) ratio of > 40% was considered "High Risk" based on standard consumer banking thresholds.
* 📉 **Default Definition:** Both `Charged Off` and `Default` statuses were grouped together as "Non-Performing Assets (NPAs)" for default rate calculations.
* 📈 **YoY Growth Anomaly Handling:** In the Year-over-Year auto loan growth analysis, the first year's `NULL` value from the `LAG()` function was intentionally kept, as growth cannot be calculated without a prior baseline year.
</details>

<hr>

### 🛠️ <details>
<summary><b style="font-size: 18px; cursor: pointer;">🔥 3. Challenges Faced & Troubleshooting</b></summary>
<br>

Building an end-to-end pipeline for **270,000+ records** presented unique technical hurdles:

> **🛑 Challenge 1: Memory Outages during Data Ingestion**
> * **Issue:** Loading a massive CSV file directly into MySQL caused pandas memory overflows and database connection timeouts.
> * **Solution:** Implemented **Batch Processing** using Python's `chunksize=10000` parameter with SQLAlchemy. This ingested data efficiently without straining system RAM.

> **⏳ Challenge 2: Slow SQL Query Execution**
> * **Issue:** Complex Window Functions and multiple `JOIN` statements across 6 tables resulted in query execution times exceeding 10 seconds.
> * **Solution:** Applied **Database Indexing** (`CREATE INDEX`) on highly queried columns like `customer_id`, `loan_status`, and `issue_year`, reducing execution time to sub-seconds.
</details>

<hr>

### 🚀 <details>
<summary><b style="font-size: 18px; cursor: pointer;">🔮 4. Future Scope & Machine Learning</b></summary>
<br>

If given more time and computational resources, the next iterations of this project would include:
1. 📊 **Real-time Dashboarding:** Connecting the MySQL database directly to Power BI via DirectQuery for live portfolio monitoring.
2. 🤖 **Predictive Machine Learning:** Upgrading the Logistic Regression model to ensemble methods like **XGBoost or Random Forest** for better handling of non-linear financial patterns.
3. 🎯 **Hyperparameter Tuning:** Applying `GridSearchCV` to optimize the current loan prediction model's recall score to catch more potential defaulters.
</details>

<br>

<div align="center">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge" alt="Status" />
  <img src="https://img.shields.io/badge/Ready_For-Interviews-0078D4?style=for-the-badge" alt="Ready" />
</div>
