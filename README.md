# Conversion Ranker

**Conversion Ranker** is a machine learning project designed to optimize sales efficiency by predicting which leads are most likely to convert into paying customers. By analyzing user behavior and demographic data, the model generates a probability score for each lead, allowing sales teams to prioritize high-potential targets.

---

## Project Overview

The primary objective was to build a classification model to identify potential customers ("converters") from a pool of 4,612 leads. The dataset posed a typical business challenge: a class imbalance where only ~29.9% of leads resulted in a conversion.

The goal was to maximize **Recall** (to ensure valuable leads aren't missed) while maintaining high **Precision** (to prevent wasting resources on non-converters).

---

## Data Dictionary & EDA

The dataset consists of 15 features encompassing demographic details and interaction metrics. Key features analyzed include:

* **Interaction Metrics:** `time_spent_on_website`, `website_visits`, `page_views_per_visit`.
* **Demographics:** `age`, `current_occupation`.
* **Sources:** `first_interaction`, `referral`.

**Key Insight:** Exploratory Data Analysis (EDA) revealed that **time spent on the website** is highly correlated with conversion. While most users have low engagement, those with higher session durations showed a distinct increase in conversion likelihood. Additionally, `current_occupation` showed clear segmentation, with "Professionals" making up the majority of the lead pool (56.7%).

---

## Data Preparation

To prepare the data for modeling, I implemented the following steps:

* **Encoding:** Converted categorical variables (e.g., `first_interaction`, `profile_completed`) into dummy variables.
* **Cleaning:** Dropped non-predictive unique identifiers (`ID`) to prevent noise.
* **Stratified Split:** Split the data into training (70%) and testing (30%) sets, using stratification to preserve the 30/70 class imbalance in both subsets.

---

## Modeling Strategy

I experimented with two classifiers to establish a baseline and then improve performance.

### 1. Decision Tree Classifier (Baseline)
I started with a Decision Tree using `class_weight='balanced'`. While it achieved perfect recall on the training set, it suffered from significant overfitting, with test recall dropping to 0.62 and precision to 0.65. This indicated the model was memorizing the data rather than learning generalizable patterns.

### 2. Random Forest Classifier (Final Model)
To reduce variance and improve generalization, I transitioned to a Random Forest ensemble. I utilized `GridSearchCV` to tune hyperparameters, optimizing for **Recall**.

* **Best Parameters:** `n_estimators=200`, `max_depth=5`, `class_weight='balanced'`, `min_samples_leaf=50`.
---

## Results & Evaluation

The tuned Random Forest model significantly outperformed the Decision Tree on unseen data.

| Metric (Test Set) | Score |
| :--- | :--- |
| **ROC AUC** | **0.917** |
| **Recall (Class 1)** | **0.85** |
| **Precision (Class 1)** | 0.67 |
| **F1-Score** | 0.75 |

The high ROC AUC score (0.917) confirms the model is highly effective at distinguishing between converters and non-converters.

---

## Business Impact

The primary value of this project is the ability to rank leads by probability.

* **Feature Importance:** The model identified that `first_interaction_Website` and `time_spent_on_website` are the strongest predictors of success, informing marketing where to focus their optimization efforts.
* **Efficiency Gain:** By targeting the **top 20%** of leads ranked by the model, the sales team can capture **86.6%** of all potential conversions. This allows for a drastic reduction in call volume while maintaining the majority of revenue.

The final ranked list was exported to `ranked_leads.csv` for immediate use.
