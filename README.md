# Deprivation & Destiny: How Socioeconomic Background Shapes Student Attainment Across England

A comprehensive data science analysis investigating the relationship between socioeconomic deprivation and educational attainment across English local authorities (2018/19–2024/25).

## Project Overview

This project answers four research questions:

1. **Do more deprived local authorities consistently produce lower Attainment 8 scores?**  
   Yes. Pearson correlation r = -0.61 (p < 0.001) — deprivation explains ~37% of attainment variance.

2. **Has the attainment gap between disadvantaged and non-disadvantaged pupils changed over time?**  
   The gap has **widened** from 14.69 points (2018/19) to 16.60 points (2024/25).

3. **Which local authorities perform above or below expectations?**  
   London boroughs consistently overperform; southern coastal towns consistently underperform relative to their deprivation levels.

4. **Can educational attainment be predicted from deprivation indicators?**  
   Yes. A tuned Random Forest model achieves **R² = 0.887** (MAE = 0.92 points) on 2024/25 data, with education deprivation ~10× more important than other dimensions.

## Key Findings

- **Deprivation is predictive but not deterministic:** 63% of attainment variation comes from factors beyond deprivation (school quality, leadership, community support).
- **Education deprivation matters most:** SHAP analysis shows education dimension is 10× more impactful than income, crime, or health deprivation.
- **Post-pandemic inequality widened:** Non-disadvantaged pupils recovered attainment post-COVID; disadvantaged pupils did not.
- **Geography reveals policy lessons:** London's success despite high deprivation (Newham R² = 51.3 in decile-1) offers a model for other regions.

## Data Sources

| Dataset | Source | Coverage |
|---|---|---|
| Attainment 8 | Department for Education (DfE) | 339 local authorities, 7 academic years (2018/19–2024/25) |
| IMD 2019 | Ministry of Housing, Communities & Local Government | 32,844 LSOAs aggregated to 317 local authorities |
| IMD 2025 | MHCLG | 33,755 LSOAs aggregated to 324 local authorities |

**Download:** All raw CSV files are in the `data/` folder.

## How to Use This Project

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Run the Analysis

Open `notebooks/01_analysis.ipynb` in Jupyter and run all cells top-to-bottom. The notebook is self-contained and generates all outputs.

```bash
jupyter notebook notebooks/01_analysis.ipynb
```

### 3. View Outputs

All visualisations and the trained model are saved to `outputs/`:

- **Charts:** `chart1_scatter.png` through `chart8_predicted_vs_actual.png`
- **Model:** `random_forest_attainment_model_tuned.pkl`
- **Database:** `education_deprivation.db` (SQLite)

## Project Structure

education-deprivation-analysis/

│

├── data/                          # Raw datasets (CSV)

│   ├── attainment8.csv

│   ├── imd2019.csv

│   └── imd2025.csv

│

├── notebooks/

│   └── 01_analysis.ipynb          # Main analysis notebook (all 7 sections)

│

├── outputs/                       # Generated charts, model, database

│   ├── chart1_scatter.png

│   ├── chart2_line.png

│   ├── ... (charts 3–8)

│   ├── random_forest_attainment_model_tuned.pkl

│   └── education_deprivation.db

│

├── README.md                      # This file

├── requirements.txt               # Python dependencies

└── .gitignore                     # Git ignore rules

## Key Methods

**Data Cleaning:**
- Merged 3 datasets on local authority code (98.3% match rate documented)
- Removed 19 unmatched LAs due to boundary reorganisations (1.7% of data)
- Handled small-cohort outliers (Isles of Scilly pupil counts < 5)
- SQL integration: stored cleaned data in SQLite for reusability

**Analysis:**
- Pearson correlation + hypothesis testing (scipy.stats)
- Residual analysis for over/underperformance detection
- NumPy for outlier detection and data quality metrics

**Modelling:**
- Train/test split (80/20)
- 5-fold cross-validation (confirms model stability)
- GridSearchCV hyperparameter tuning (108 configurations tested)
- Three models compared: Linear Regression, Random Forest, XGBoost
- SHAP feature importance analysis
- Final tuned Random Forest: R² = 0.887, MAE = 0.92 points

**Visualisations:**
- 8 different chart types (scatter, line, bar, box, heatmap, histogram, SHAP, predicted-vs-actual)
- All using Matplotlib and Seaborn

## Recommendations

1. **Refocus funding from income to education deprivation** — education deprivation is 10× more predictive than household poverty.
2. **Study overperforming London boroughs** — Newham and Hackney achieve strong results despite high deprivation; their practices warrant replication.
3. **Target underperforming coastal towns** — Gosport, Hastings, Tendring underperform by 5–8 points relative to deprivation level.
4. **Treat post-pandemic inequality gap as urgent** — the 16.6-point gap (up from 14.7) is the largest in six years.

## Limitations

- LSOA→LAD aggregation loses within-authority variation
- IMD is a 4–7 year snapshot; temporal mismatch exists
- Model is optimised for 2024/25; extrapolation to other years requires revalidation
- Ecological fallacy: local authority patterns don't necessarily apply to individual pupils

## Contact & Attribution

**Author:** Gifty Acquah  
**Course:** Code First Girls — Data Science & AI Specialisation (16-week)  
**Date:** June 2026

---

*This project demonstrates the full data science pipeline: problem definition → data collection/cleaning → exploratory analysis → statistical modelling → deployment-ready predictions → actionable insights for policy.*
