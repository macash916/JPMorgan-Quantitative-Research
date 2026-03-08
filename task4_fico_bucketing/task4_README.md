# Task 4: FICO Score Bucketing Optimization

## 🎯 Objective

Create optimal FICO score buckets for mortgage default prediction using quantization techniques.

## 📋 Problem Statement

Charlie's ML model needs categorical data (buckets), but FICO scores are continuous (300-850). We need to:
- Map FICO scores to rating buckets (lower rating = better credit)
- Optimize bucket boundaries for default prediction
- Create generalizable approach for future datasets
- Compare MSE vs Log-Likelihood optimization

## 🔧 Solution Approach

### Two Mathematical Approaches

**Approach 1: Mean Squared Error (MSE)**
- **Goal:** Minimize prediction error
- **Method:** Quantile-based (equal observations per bucket)
- **Formula:** MSE = (1/n) × Σ(Yi - Ŷi)²
- **Result:** 35% discrimination range

**Approach 2: Log-Likelihood Maximization**
- **Goal:** Maximize default separation
- **Method:** Decision Tree optimization (Gini impurity)
- **Formula:** LL = Σ[ki × ln(pi) + (ni - ki) × ln(1 - pi)]
- **Result:** 57% discrimination range ⭐

### Winner: Log-Likelihood (63% more discriminative)

## 📊 Results

### Comparison Table

| Metric | MSE | Log-Likelihood | Winner |
|--------|-----|----------------|--------|
| **Default Range** | 35% | **57%** | **LL** ✅ |
| **MSE Score** | **392.65** ✅ | 725.67 | MSE |
| **Log-Likelihood** | -4321.03 | **-4271.32** ✅ | **LL** |
| **Business Focus** | Data distribution | **Defaults** ✅ | **LL** |

### Optimal 5-Tier Rating System (Log-Likelihood)

| Rating | FICO Range | Default Rate | Count | Risk Level |
|--------|------------|--------------|-------|------------|
| **5** | **< 520** | **66%** | 301 (3%) | EXTREME ⚠️ |
| **4** | **520-552** | **46%** | 496 (5%) | HIGH |
| **3** | **552-580** | **34%** | 911 (9%) | MED-HIGH |
| **2** | **580-640** | **20%** | 3,438 (34%) | MEDIUM |
| **1** | **≥ 640** | **9%** | 4,854 (49%) | LOW ✅ |

### Key Finding: FICO 640 is the Magic Number

- **Below 640:** 20%+ default rate
- **Above 640:** 9% default rate
- **This is the critical threshold!**

## 💻 Code Structure

```python
def bucket_fico_scores(fico_scores, n_buckets=5, method='log_likelihood'):
    """
    Create optimal FICO score buckets for any dataset.
    
    Parameters:
    -----------
    fico_scores : array-like
        Array of FICO scores to bucket
    n_buckets : int
        Number of buckets (default: 5)
    method : str
        'mse' or 'log_likelihood'
    
    Returns:
    --------
    dict containing:
        - boundaries: Optimal bucket thresholds
        - ratings: Risk rating for each score
        - bucket_info: Statistical summary
    """
```

```python
def create_rating_map(fico_score, boundaries):
    """
    Map FICO score to rating (1 = best, 5 = worst).
    
    Example:
    --------
    >>> create_rating_map(720, boundaries)
    1  # Excellent credit
    
    >>> create_rating_map(550, boundaries)
    4  # High risk
    """
```

## 🚀 Usage

```python
# Bucket FICO scores
result = bucket_fico_scores(
    fico_scores=df['fico_score'],
    n_buckets=5,
    method='log_likelihood'
)

print("Boundaries:", result['boundaries'])
# Output: [520, 552, 580, 640]

# Rate individual borrower
rating = create_rating_map(585, result['boundaries'])
print(f"FICO 585 → Rating {rating}")
# Output: FICO 585 → Rating 2
```

## 📁 Files

- `jpmc_task4_fico_bucketing.py` - Complete implementation (650 lines)
- `outputs/fico_analysis.png` - 4 exploratory charts
- `outputs/bucketing_comparison.png` - MSE vs LL comparison (4 charts)

## 📊 Visualizations

### FICO Analysis (4 charts)
1. Distribution by default status
2. Default rate vs FICO (clear downward trend)
3. Cumulative distribution
4. Default rate by traditional ranges

### Bucketing Comparison (4 charts)
1. MSE approach - default rates (gradual decline)
2. LL approach - default rates (steeper gradient) ⭐
3. MSE boundaries on distribution
4. LL boundaries on distribution (concentrated at low FICO)

## 🎓 Key Learnings

1. **Log-Likelihood optimization is superior** for credit risk (63% better)
2. **FICO 640 is critical threshold** for risk segmentation
3. **Decision Trees** are effective for finding optimal splits
4. **Business objective** (predict defaults) should drive optimization
5. **Unequal bucket sizes** are optimal (not equal quantiles)

## 📈 Business Applications

### Use Case 1: Loan Approval
```
Applicant: FICO 585 → Rating 2 (Medium Risk)
Decision: Approve at 9% interest (vs 7% standard)
```

### Use Case 2: Portfolio Monitoring
```
Rating 5 loans: 301 × 66% = 199 expected defaults → High priority
Rating 1 loans: 4,854 × 9% = 437 expected defaults → Standard monitoring
```

### Use Case 3: Pricing Strategy
```
Rating 1: 7% interest (low risk)
Rating 3: 12% interest (medium-high risk)
Rating 5: Decline or 18%+ interest (extreme risk)
```

## 💡 Why Log-Likelihood Wins

**MSE Approach:**
- Optimizes for equal bucket sizes
- Minimizes variance within buckets
- Good for data representation
- **35% discrimination**

**Log-Likelihood Approach:**
- Optimizes for default prediction
- Maximizes separation between risk levels
- Focuses on business goal
- **57% discrimination** ⭐

**Result:** 63% improvement in ability to distinguish risk!

## 🔄 Comparison with JPMC Example

**JPMC's Example:**
- Only Log-Likelihood (no MSE comparison)
- Complex dynamic programming (60 lines)
- Hard-coded parameters
- No visualizations

**Our Solution:**
- Both MSE AND Log-Likelihood
- Clean sklearn implementation (650 lines)
- Generalizable function
- Professional visualizations
- **5x more comprehensive!**

---

**Status:** ✅ Complete | **Best Method:** Log-Likelihood | **Improvement:** +63%
