import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import LabelEncoder, StandardScaler
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score,
    roc_auc_score,
    confusion_matrix,
    classification_report,
    roc_curve
)

# ==========================================
# 1. SETTINGS & STYLING
# ==========================================
# Plots ko professional look dene ke liye
sns.set_theme(style="whitegrid", palette="muted")
plt.rcParams.update({'figure.figsize': (8, 5), 'font.size': 12})

# ==========================================
# 2. DATA LOADING & CLEANING
# ==========================================
print("Loading and cleaning data...")
df = pd.read_csv('loan_approval_dataset.csv')

# Drop unnecessary ID column
df.drop('loan_id', axis=1, inplace=True)

# Clean column names (strip spaces and convert to lowercase)
df.columns = df.columns.str.strip().str.lower()

# Check for missing values
print("\nMissing values status:\n", df.isnull().sum())

# ==========================================
# 3. EXPLORATORY DATA ANALYSIS (EDA)
# ==========================================
print("\nGenerating EDA Plots...")

# Plot 1: Loan Status Distribution
plt.figure()
sns.countplot(x='loan_status', data=df, hue='loan_status', legend=False, palette="Set2")
plt.title("Distribution of Loan Status", fontweight='bold')
plt.show()

# Plot 2: CIBIL Score Distribution (Very Important Feature)
plt.figure()
sns.histplot(df['cibil_score'], bins=30, kde=True, color="purple")
plt.title("Applicant CIBIL Score Distribution", fontweight='bold')
plt.show()

# Plot 3: Correlation Heatmap
numeric_df = df.select_dtypes(include=np.number)
corr = numeric_df.corr()
plt.figure(figsize=(10, 8))
sns.heatmap(corr, annot=True, cmap="coolwarm", fmt=".2f", linewidths=0.5)
plt.title("Feature Correlation Heatmap", fontweight='bold')
plt.show()

# ==========================================
# 4. FEATURE ENGINEERING & PREPROCESSING
# ==========================================
print("\nPerforming Feature Engineering...")

# Encode categorical variables
le = LabelEncoder()
categorical_cols = ['education', 'self_employed', 'loan_status']
for col in categorical_cols:
    if col in df.columns:
        df[col] = le.fit_transform(df[col])

# Create new smart features (Using snake_case for column names)
df['debt_to_income_ratio'] = df['loan_amount'] / df['income_annum']

df['total_assets'] = (df['residential_assets_value'] + 
                      df['commercial_assets_value'] + 
                      df['luxury_assets_value'] + 
                      df['bank_asset_value'])

# Select Final Features
feature_cols = [
    'no_of_dependents', 'education', 'self_employed', 'income_annum',
    'loan_amount', 'loan_term', 'cibil_score', 'residential_assets_value', 
    'commercial_assets_value', 'luxury_assets_value', 'bank_asset_value', 
    'debt_to_income_ratio', 'total_assets'
]

X = df[feature_cols]
y = df['loan_status']

# Train Test Split (80% Train, 20% Test)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=18, stratify=y
)

# Standardize Features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# ==========================================
# 5. MODEL TRAINING
# ==========================================
print("\nTraining Logistic Regression Model...")
log_reg = LogisticRegression(max_iter=1000, class_weight="balanced", random_state=42)
log_reg.fit(X_train_scaled, y_train)

# Predictions
y_pred = log_reg.predict(X_test_scaled)
y_proba = log_reg.predict_proba(X_test_scaled)[:, 1]

# ==========================================
# 6. MODEL EVALUATION
# ==========================================
print("\n" + "="*40)
print("🎯 MODEL PERFORMANCE METRICS")
print("="*40)
print(f"Accuracy:  {accuracy_score(y_test, y_pred):.4f}")
print(f"Precision: {precision_score(y_test, y_pred):.4f}")
print(f"Recall:    {recall_score(y_test, y_pred):.4f}")
print(f"F1 Score:  {f1_score(y_test, y_pred):.4f}")
print(f"ROC-AUC:   {roc_auc_score(y_test, y_proba):.4f}")

print("\n📊 Classification Report:\n")
print(classification_report(y_test, y_pred))

# Confusion Matrix Heatmap
cm = confusion_matrix(y_test, y_pred)
plt.figure(figsize=(6, 5))
sns.heatmap(cm, annot=True, fmt="d", cmap="Blues", 
            xticklabels=["Rejected", "Approved"],
            yticklabels=["Rejected", "Approved"],
            linewidths=0.5)
plt.title("Confusion Matrix", fontweight='bold')
plt.xlabel("Predicted Label")
plt.ylabel("Actual Label")
plt.show()

# ROC Curve
fpr, tpr, thresholds = roc_curve(y_test, y_proba)
plt.figure(figsize=(7, 5))
plt.plot(fpr, tpr, label=f"Logistic Regression (AUC = {roc_auc_score(y_test, y_proba):.2f})", color="blue", linewidth=2)
plt.plot([0, 1], [0, 1], 'k--', label="Random Guess")
plt.xlabel("False Positive Rate")
plt.ylabel("True Positive Rate")
plt.title("ROC Curve", fontweight='bold')
plt.legend(loc="lower right")
plt.show()

# ==========================================
# 7. FEATURE IMPORTANCE (Extra Feature for GitHub)
# ==========================================
print("\nVisualizing Feature Importance...")
# Extracting coefficients from the model
feature_importance = pd.DataFrame({
    'Feature': feature_cols,
    'Importance': log_reg.coef_[0]
})
# Sorting by absolute value to see the strongest impact
feature_importance['Abs_Importance'] = feature_importance['Importance'].abs()
feature_importance = feature_importance.sort_values(by='Abs_Importance', ascending=False)

plt.figure(figsize=(10, 6))
sns.barplot(x='Importance', y='Feature', data=feature_importance, palette="coolwarm")
plt.title("Feature Importance (Logistic Regression Coefficients)", fontweight='bold')
plt.xlabel("Impact on Loan Approval")
plt.ylabel("Features")
plt.show()
