# Task 3: Loan Default Prediction

## 🎯 Objective

Build machine learning models to predict mortgage default probability and calculate expected losses for the retail banking portfolio.

## 📋 Problem Statement

The retail banking division is experiencing higher-than-expected default rates. They need to:
- Predict which borrowers will default in the next year
- Calculate expected losses for capital allocation
- Identify key risk factors driving defaults
- Segment portfolio by risk level

## 🔧 Solution Approach

### Model Architecture

**Compared 3 Classification Models:**
1. **Logistic Regression** - Interpretable, industry standard
2. **Random Forest** - Ensemble method, handles non-linearity
3. **Gradient Boosting** - Advanced ensemble, captures complex patterns

### Feature Engineering

**Original Features:**
- credit_lines_outstanding
- loan_amt_outstanding
- total_debt_outstanding
- income
- years_employed
- fico_score

**Engineered Features:**
- **debt_to_income** = total_debt / income
- **loan_to_income** = loan_amt / income
- **utilization** = total_debt / income

### Expected Loss Formula

```
Expected Loss = PD × EAD × LGD

Where:
  PD  = Probability of Default (model output)
  EAD = Exposure at Default (loan_amt_outstanding)
  LGD = Loss Given Default (0.90, assuming 10% recovery rate)
```

## 📊 Results

### Model Performance

| Model | AUC-ROC | Precision | Recall | F1-Score |
|-------|---------|-----------|--------|----------|
| **Logistic Regression** | **1.0000** ⭐ | **98.1%** | **100%** | **99.1%** |
| Random Forest | 0.9990 | 93.3% | 98.1% | 95.7% |
| Gradient Boosting | 0.9999 | 99.7% | 98.4% | 99.1% |

**Winner:** Logistic Regression (perfect discrimination)

### Portfolio Analysis

**Test Portfolio (2,000 loans):**
- Total Loan Amount: $8,373,362
- **Expected Loss: $1,552,471** (18.54%)
- Average Loss per Loan: $776

**Risk Segmentation:**

| Risk Level | Loans | % | Default Rate |
|------------|-------|---|--------------|
| Low | 1,570 | 78.5% | 0% |
| Medium | 31 | 1.6% | 17% |
| High | 399 | 20.0% | 95% |

### Feature Importance

1. **credit_lines_outstanding** 🔴 (Most Important)
2. years_employed
3. debt_to_income
4. utilization
5. fico_score

**Key Finding:** 5 credit lines = 100% default rate!

## 💻 Usage

```python
result = predict_expected_loss(
    credit_lines_outstanding=2,
    loan_amt_outstanding=5000,
    total_debt_outstanding=8000,
    income=50000,
    years_employed=3,
    fico_score=620
)

print(f"Expected Loss: ${result['expected_loss']:.2f}")
# Output: Expected Loss: $20.07
```

## 📁 Files

- `jpmc_task3_loan_default.py` - Complete ML pipeline
- `outputs/loan_default_eda.png` - 9 exploratory charts
- `outputs/model_performance.png` - ROC, PR, confusion matrix
- `outputs/feature_importance.png` - Risk drivers

## 📈 Business Impact

- **Capital Allocation:** Set aside $1.55M for losses
- **Risk Focus:** Monitor 399 high-risk loans (20%)
- **Underwriting:** Decline applicants with 5+ credit lines
- **Pricing:** Risk-adjusted interest rates

---

**Status:** ✅ Complete | **AUC:** 1.000 (Perfect) | **Expected Loss:** $1.55M
