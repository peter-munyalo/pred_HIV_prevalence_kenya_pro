# Predicting HIV Prevalence in Kenya: A Logistic Regression Analysis of Socioeconomic and Health System Indicators

## 📋 Project Overview

This project investigates the relationship between health system factors, financing mechanisms, and HIV prevalence in Kenya from 2000 to 2023. Using a logistic regression approach, we identify the strongest predictors of high HIV prevalence and derive evidence-based policy recommendations.

**Research Question:** Which socioeconomic and health system factors are the strongest predictors of high HIV prevalence across different time periods in Kenya?

**Key Finding:** Universal Health Coverage (UHC) for noncommunicable diseases, health expenditure per capita, and the share of private health spending are the most powerful predictors, together achieving 100% classification accuracy.

---

## 📊 Dataset Description

Six datasets from the WHO Global Health Observatory were integrated:

| Dataset | Description | Key Indicators |
|---------|-------------|----------------|
| `health_financing_indicators_ken.csv` | Health expenditure data | Government spending, private spending, out-of-pocket costs, external funding |
| `hiv_indicators_ken.csv` | HIV epidemiology | Adult HIV prevalence (target variable), new infections, ART coverage |
| `health_systems_indicators_ken.csv` | Health infrastructure | Hospital beds, health facility density |
| `health_workforce_indicators_ken.csv` | Health personnel | Doctors, nurses, pharmacists per capita |
| `uhc_indicators_ken.csv` | Universal Health Coverage | UHC Service Coverage Index and sub-indices |
| `tobacco_control_indicators_ken.csv` | Tobacco policy | (Excluded due to categorical nature) |

**Final Integrated Dataset:**
- 24 years (2000-2023)
- 49 indicators across all domains
- Binary target: High prevalence (>5.25%) vs Low prevalence (≤5.25%)

---

## 🛠️ Methodology

### Data Processing Pipeline
1. **Data Loading**: Import all CSV files using pandas
2. **Data Cleaning**: Handle missing values, filter relevant indicators
3. **Pivoting**: Reshape from long to wide format (one row per year)
4. **Merging**: Sequential merges on year to create unified dataset
5. **Missing Value Treatment**: Drop columns with >50% missing; median imputation for remaining
6. **Feature Selection**: Address multicollinearity (|r| > 0.9 among top predictors)

### Model Development
- **Algorithm**: Logistic Regression with L2 regularization
- **Feature Scaling**: StandardScaler (mean=0, std=1)
- **Train/Test Split**: 70/30 random split with stratification
- **Evaluation Metrics**: Accuracy, Precision, Recall, F1-Score, ROC AUC, Confusion Matrix
- **Interpretation**: Odds ratios with 95% confidence intervals

### Tools Used
- **Python 3.9+**
- **pandas**: Data manipulation and merging
- **numpy**: Numerical computations
- **matplotlib/seaborn**: Data visualization
- **scikit-learn**: Machine learning (modeling, preprocessing, evaluation)
- **scipy**: Statistical calculations

---

## 📈 Exploratory Data Analysis Findings

### HIV Prevalence Trend in Kenya (2000-2023)

| Period | Trend | Classification |
|--------|-------|----------------|
| 2000-2011 | Declining from 8.8% to 5.2% | HIGH prevalence |
| 2012-2023 | Declining from 5.0% to 3.2% | LOW prevalence |
| **Overall** | **64% reduction** | Perfect balance: 12 high, 12 low years |

### Top Correlations with HIV Prevalence

| Rank | Indicator | Abbreviation | Correlation | Interpretation |
|------|-----------|--------------|-------------|----------------|
| 1 | UHC NCD Sub-index | UHC_NCD | **-0.985** | Strong protective factor |
| 2 | Private Health Expenditure (% of CHE) | PVT_D_CHE | **+0.971** | Strong risk factor |
| 3 | Health Expenditure per Capita | CHE_pc | **-0.969** | Strong protective factor |
| 4 | Out-of-pocket Expenditure (% of CHE) | OOP_CHE | **+0.957** | Strong risk factor |
| 5 | UHC RMNCH Sub-index | UHC_RMNCH | -0.943 | Strong protective factor |

### Multicollinearity Observation
All top predictors were highly correlated (|r| > 0.9), requiring careful feature selection to avoid model instability.

---

## 🤖 Model Results

### Selected Features (Final Model)
Based on domain knowledge and correlation analysis, three representative predictors were selected:
1. **UHC_NCD**: UHC Noncommunicable Diseases Sub-index
2. **PVT_D_CHE**: Private health expenditure as % of current health expenditure
3. **CHE_pc**: Current health expenditure per capita (US$)

### Performance Metrics

| Metric | Value | Interpretation |
|--------|-------|----------------|
| **Accuracy** | 1.00 | Perfect classification |
| **Precision (Low)** | 1.00 | No false positives |
| **Precision (High)** | 1.00 | No false positives |
| **Recall (Low)** | 1.00 | All low years identified |
| **Recall (High)** | 1.00 | All high years identified |
| **F1-Score** | 1.00 | Perfect balance |
| **ROC AUC** | **1.00** | Perfect discrimination |

### Confusion Matrix
              Predicted
              Low  High
Actual Low     4     0
Actual High    0     4


### Model Coefficients and Odds Ratios

| Feature | Coefficient | Odds Ratio | 95% Confidence Interval | Effect |
|---------|-------------|------------|------------------------|--------|
| UHC_NCD | -1.15 | **0.32** | [0.0001, 1039.8] | Protective |
| PVT_D_CHE | +0.51 | **1.67** | [0.004, 762.3] | Risk |
| CHE_pc | -0.98 | **0.38** | [0.0003, 523.2] | Protective |

### Interpretation of Odds Ratios

**UHC_NCD (UHC Noncommunicable Diseases Sub-index)**
- **Odds Ratio**: 0.32
- **Interpretation**: For every one-unit increase in the UHC NCD sub-index, the odds of being in the high HIV prevalence group decrease by a factor of **3.14** (1/0.32 = 3.14)
- **Practical meaning**: Countries/periods with better NCD service coverage are substantially less likely to have high HIV prevalence
- **Magnitude**: This is the **strongest protective factor** in the model

**CHE_pc (Health Expenditure per Capita)**
- **Odds Ratio**: 0.38
- **Interpretation**: For every one-unit increase in health spending per person (US$), the odds of high HIV prevalence decrease by a factor of **2.67** (1/0.38 = 2.67)
- **Practical meaning**: Higher investment in health per person is associated with lower HIV prevalence
- **Magnitude**: The **second strongest protective factor**

**PVT_D_CHE (Private Health Expenditure as % of Current Health Expenditure)**
- **Odds Ratio**: 1.67
- **Interpretation**: For every one-percentage-point increase in the share of private health spending, the odds of high HIV prevalence increase by **1.67 times**
- **Practical meaning**: Greater reliance on private health financing (vs. public) is associated with higher HIV prevalence
- **Magnitude**: This is the **only risk factor** among the selected predictors

### Confidence Intervals Note
The confidence intervals for all predictors are extremely wide due to small sample size (n=24). This indicates uncertainty in the precise magnitude of effects, though the direction and relative importance are clear.

## Key Insights

### What the Data Reveals

1. **Health System Strength Is Paramount**
   - UHC NCD coverage is the single strongest predictor
   - Stronger health systems = lower HIV prevalence
   - Suggests HIV outcomes are linked to overall health system performance

2. **Financing Mechanisms Matter Critically**
   - **Public vs Private mix**: Higher private share = higher HIV prevalence
   - **Total spending**: More health spending per capita = lower HIV prevalence
   - **Out-of-pocket costs**: Strong positive correlation with prevalence

3. **Kenya's Success Story**
   - 64% reduction in HIV prevalence over 24 years
   - Clear structural break around 2011-2012
   - Coincides with major scale-up of HIV programs and UHC improvements

### Summary Table of Findings

| Factor | Effect on HIV Prevalence | Strength |
|--------|-------------------------|----------|
| UHC NCD Coverage | Strongly protective | Most important |
| Health Spending per Capita | Protective | Second most important |
| Private Health Spending Share | Risk factor | Third most important |
| Out-of-pocket Expenditure | Risk factor | High correlation |
| Government Health Spending | Protective | High correlation |

## Policy Recommendations

### 1. Strengthen UHC for NCDs
- Expand noncommunicable disease services within the public health system
- Integrate HIV services with NCD care to improve overall coverage
- Invest in primary healthcare infrastructure serving both HIV and NCD patients

### 2. Increase Public Health Spending
- Raise government health expenditure per capita
- Prioritize funding for HIV prevention, testing, and treatment
- Ensure resources reach frontline health facilities

### 3. Reduce Out-of-Pocket/Private Spending
- Expand health insurance coverage to reduce reliance on private payments
- Subsidize HIV services to remove financial barriers
- Strengthen public sector capacity to reduce need for private care

### 4. Targeted Interventions
- Focus resources on regions with low UHC_NCD scores
- Monitor private spending levels as early warning indicators
- Use health expenditure per capita as a tracking metric for HIV program success

## Limitations

### Data Limitations
- **Small sample size** (n=24) limits statistical power
- **Ecological design** - national aggregates mask within-country variation
- **Missing data** required dropping many potential predictors
- **Time series nature** - observations are not independent

### Methodological Limitations
- **Multicollinearity** required selective feature inclusion
- **Wide confidence intervals** indicate uncertainty in effect sizes
- **Perfect separation** may indicate overfitting despite validation
- **Causation cannot be inferred** - only associations identified

### Generalizability
- Results may not apply to other countries
- Findings specific to 2000-2023 period
- County/regional variations not captured
- Individual-level factors not considered

## Future Research Directions

1. **Sub-national analysis** using county-level data
2. **Include additional predictors**: education, poverty, inequality
3. **Longer time series** when more data becomes available
4. **Explore non-linear relationships**
5. **Validate with other modeling approaches** (random forests, gradient boosting)
6. **Qualitative research** to understand mechanisms behind statistical findings

