# Wine Quality EDA: What Makes a Good Wine?

## Overview

For this project I was looking at what makes a wine good or bad, scoring the wine high or low, and whether the answer is the same for red and white wines. The data comes from the UC Irvine Machine Learning Repository and contains over 6,400 wine samples, each described by 11 chemical measurements and a quality score from human tasters (scale 0–10).

The two original files (one for red, one for white) were merged into a single data frame with a `type` column added to track wine type.

**Dataset:** Cortez et al. (2009). *Wine Quality*. UCI Machine Learning Repository. https://doi.org/10.24432/C56S3T

---

## Research Questions

1. Which physicochemical properties are most strongly associated with higher wine quality?
2. Do red and white wines follow the same patterns, or do different features matter for each type?

---

## What I Did

**Data loading:** The two CSV files are loaded directly from this repository using raw GitHub URLs, then combined with `pd.concat()`. The `type` column was converted to `pd.Categorical` to properly represent it as a label rather than a number.

**Distribution overview:** Plotted histograms for all 11 features split by wine type using a for loop. Red and white wines look quite different — white wine has much higher residual sugar and total sulfur dioxide, while red wine has higher volatile acidity. Several features were also heavily right-skewed, so log transformations were applied using `np.log()` before computing correlations.

**Correlation analysis:** Pearson correlations between each feature and quality were computed separately for red and white wine using a for loop with `np.corrcoef()`. Results were collected into a list of dictionaries and converted to a DataFrame — the same pattern used in the API exercises in class.

**High vs low quality comparison:** Low quality was defined as scores ≤5 and high quality as ≥7 using `df.loc[]` filtering. Mean values for each group were computed using a nested for loop over wine type and quality group, then visualized with violin plots and grouped bar charts.

**Regression by type:** A reusable function `scatter_regression()` was written to plot quality vs a feature with separate regression lines for red and white wine, using `np.polyfit()` to fit the lines.

---

## Main Findings

- **Alcohol** is the strongest predictor of quality for both wine types (r ≈ 0.48 for red, r ≈ 0.44 for white). Wines above 13% ABV score on average nearly a full point higher than wines below 9%.

- **Volatile acidity** hurts quality in both types, but the effect is stronger for red wine (r ≈ -0.39 vs -0.19). High volatile acidity is associated with a vinegar-like taste.

- **Sulphates** are a positive predictor for red wine (r ≈ 0.25) but barely matter for white wine (r ≈ 0.05). Same story for **citric acid** — positive for red, near-zero for white.

- **Density** is a stronger negative predictor for white wine (r ≈ -0.31), likely because white wines vary more widely in sugar content.

So the answer to the second research question is: partly the same, partly not. Alcohol is consistently important across both types, but several other features only matter for one type.

---

## References

Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009). Modeling wine preferences by data mining from physicochemical properties. *Decision Support Systems*, 47(4), 547–553.

Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009). *Wine Quality* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C56S3T
