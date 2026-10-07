# Loan-Approval-Prediction

End-to-end ML pipeline for Loan Approval Prediction using Python, Pandas, and Scikit-Learn. Achieved 92.7% accuracy and 96.6% ROC-AUC using Logistic Regression.

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&height=150&section=header&text=Elite%20Loan%20Approval%20Prediction&fontSize=30&fontAlignY=38&animation=twinkling" />
</div>

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit_Learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge&logo=python&logoColor=white)

> **Predicting loan approvals using Data Analytics and Machine Learning to streamline banking decisions, reduce human bias, and mitigate credit risk.**

</div>

---

## 🧐 The "Why": Project Objective

In today's fast-paced financial world, loan approval is a critical aspect of banking. Traditionally, this process has been heavily dependent on manual assessments, making it time-consuming and prone to human bias. I conducted a comprehensive machine learning study on over 4,000 loan applications to decode the financial patterns of applicants and automate the decision-making process.

By processing historical and real-time financial data, I aimed to:
1. 🔍 **Identify the drivers of creditworthiness** (what makes an applicant a safe bet).
2. ⚖️ **Eliminate human bias** (ensuring fair and consistent evaluations for all).
3. 🤖 **Build a predictive engine** that can automatically forecast whether a loan should be approved or rejected with high accuracy.

---

## 📊 The Dataset

The project utilizes the `loan_approval_dataset.csv` dataset, which contains **4,269 records** and **13 features**.

**Key Features Include:**
- 🧑‍🤝‍🧑 **Demographic Factors:** Number of dependents, education, and self-employment status.
- 💰 **Financial Factors:** Annual income, loan amount, and loan term.
- 📈 **Credit History:** CIBIL score.
- 🏠 **Asset Values:** Residential, commercial, luxury, and bank asset values.
- 🎯 **Target Variable:** `loan_status` (Approved/Rejected).

---

## 🚀 The Pipeline: My Workflow

### 🧹 1. Data Cleaning & Preprocessing
Financial data needs to be spotless. I implemented a cleaning pipeline that:
- Removed non-predictive features like `loan_id`.
- Standardized column nomenclature by stripping trailing spaces and lowercasing.
- Validated data integrity, ensuring zero missing values across the 4,269 records.

### 📉 2. Exploratory Data Analysis (EDA)
I visualized the data to reveal hidden financial stories:
- **Demand Concentration:** Discovered that nearly 75% of loan applications were for amounts below ₹20,000,000 (2 Crore).
- **Income vs. Approval:** Analyzed that median income was identical for both approved and rejected loans, proving that income alone is *not* enough to predict approval.
- **Credit Mapping:** Mapped CIBIL score distributions to understand the risk profiles of the applicants.

### 🛠️ 3. Advanced Feature Engineering
To give the model a deeper understanding of an applicant's financial health, I engineered new predictive features:
- **Debt-to-Income Ratio:** A mathematical transformation (`loan_amount` / `income_annum`) to measure an applicant's repayment capacity.
- **Total Assets:** Aggregated residential, commercial, luxury, and bank assets into a single comprehensive wealth metric.

### 🧠 4. Predictive Modeling (Logistic Regression)
To automate the decision process, I built a **Logistic Regression** model. The data was split 80:20 (Train/Test) and scaled to ensure large numbers didn't overpower the algorithm. I also applied class balancing to handle target variable discrepancies.

---

## 🏆 Model Performance

The model achieved excellent discrimination ability, proving to be a highly reliable and scalable solution for automating loan approval decisions.

| Metric | Score |
| :--- | :--- |
| 🎯 **Accuracy** | **92.74%** |
| 📈 **ROC-AUC** | **96.60%** |
| 🔍 **Precision** | **87.61%** |
| 🔄 **Recall** | **94.12%** |
| ⚖️ **F1-Score** | **90.75%** |

> 💡 **Business Impact:** High recall ensures that most creditworthy applicants are correctly identified, while strong precision (87.6%) minimizes the expensive risk of "false approvals" (giving bad loans).

---

## 💻 Complete Python Implementation

<details>
<summary><b>🔥 Click here to view the full Python Code for this project</b></summary>
<br/>

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import LabelEncoder, StandardScaler
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix, roc_auc_score

# 1. Load and Clean Data
df = pd.read_csv('loan_approval_dataset.csv')
df.drop('loan_id', axis=1, inplace=True)
df.columns = df.columns.str.strip().str.lower()

# 2. Feature Engineering
le = LabelEncoder()
for col in ['education', 'self_employed', 'loan_status']:
    df[col] = le.fit_transform(df[col])

df['debt_to_income_ratio'] = df['loan_amount'] / df['income_annum']
df['total_assets'] = (df['residential_assets_value'] + 
                      df['commercial_assets_value'] + 
                      df['luxury_assets_value'] + 
                      df['bank_asset_value'])

# 3. Train-Test Split & Scaling
feature_cols = ['no_of_dependents', 'education', 'self_employed', 'income_annum',
                'loan_amount', 'loan_term', 'cibil_score', 'residential_assets_value', 
                'commercial_assets_value', 'luxury_assets_value', 'bank_asset_value', 
                'debt_to_income_ratio', 'total_assets']

X = df[feature_cols]
y = df['loan_status']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=18, stratify=y)

scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# 4. Model Training & Evaluation
log_reg = LogisticRegression(max_iter=1000, class_weight="balanced")
log_reg.fit(X_train, y_train)

y_pred = log_reg.predict(X_test)
y_proba = log_reg.predict_proba(X_test)[:, 1]

print(f"Accuracy: {accuracy_score(y_test, y_pred)*100:.2f}%")
print(f"ROC-AUC: {roc_auc_score(y_test, y_proba)*100:.2f}%")
