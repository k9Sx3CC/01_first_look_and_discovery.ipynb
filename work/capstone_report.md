# Capstone Report

Author: Khadeeja Khan
Lane: Machine Learning – Content Refresh Prioritization
Repo: Your GitHub Repository URL
Date: 2026-07-20

## 0. Abstract

This project investigates whether machine learning can improve the prioritization of website content for refresh compared with a rule-based baseline. The analysis uses an anonymized dataset of approximately 30,000 content pages containing historical search, engagement, and content-related features. Three machine learning models were evaluated using a client-holdout validation strategy, with Random Forest achieving the highest measured Precision@50 of 0.74, compared with 0.24 for the baseline method. The final output is a ranked refresh queue with explanation codes that supports editorial decision-making. The system is intended as a decision-support tool rather than an automated publishing system.

## 1. Problem Framing

The objective of this project is to help content editors decide which pages should be reviewed first for potential updates. The unit of analysis is an individual content page, and the output is a ranked refresh score for each page.

The recommended action is to review the highest-ranked pages before allocating editorial resources. A false positive may result in unnecessary review effort, while a false negative may delay updates to pages that are genuinely declining.

Machine learning is useful because it can combine multiple search visibility, engagement, and content freshness signals simultaneously, allowing pages to be prioritized more effectively than a simple rule-based approach.

## 2. Data Safety

The project uses the anonymized content refresh dataset provided for the FlyRank ML Internship. The dataset contains approximately 30,000 pages with historical search, engagement, and content characteristics.

The model uses features including:

* Search impressions
* Search clicks
* Click-through rate (CTR)
* Average search position
* Content age
* Days since last update
* Scroll rate
* Engagement rate
* Word count
* Character count
* Search volume

To prevent target leakage:

trend_direction was used only to create the target label.
trend_pct was excluded because it directly reflects the target.
Client IDs and content IDs were used only for grouping and reporting and were never used as model features.

No client-identifying information, URLs, or private search queries appear in the final project outputs.

## 3. Baseline

The baseline uses a transparent rule-based ranking strategy developed earlier in the internship. It provides a simple reference for evaluating whether machine learning improves prioritization.

Under the same evaluation setting:

|Method	|Precision@50|
|---|---|
|Baseline|	0.24|

The baseline offers a fair comparison because both methods are evaluated using the same dataset and client-holdout validation strategy.

## 4. Model / Analysis

Three supervised learning models were trained:

* Logistic Regression
* Decision Tree
* Random Forest

The prediction target is is_declining_label, derived from historical trend_direction.

The final feature set contains 52 engineered features, including:

* Search impressions
* Search clicks
* CTR
* Average search position
* Search volume
* Content age
* Days since last update
* Engagement rate
* Scroll rate
* Word count
* Character count
* Content type
* Search intent
* Position tier
* Freshness tier

Features directly derived from the target were deliberately excluded.

## 5. Evaluation

The models were evaluated using a client_holdout split so that pages from the same client never appeared in both training and testing sets.

The measured results were:

|Method	             | Precision@50	|  ROC AUC|
|---|---|---|
|Baseline             |	0.24	    |   0.627|
|Logistic Regression	 |  0.40	    |   0.700|
|Decision Tree	     |  0.58	    |   0.742|
|Random Forest	     |  0.74	    |   0.750|

The target positive rate (base rate) is 54.2%.

The Random Forest achieved the strongest measured performance under the evaluation strategy.

Most prediction errors occurred on pages with mixed engagement signals or moderate confidence scores, where distinguishing between stable and declining content was more difficult.

## 6. Interpretation

Feature importance analysis indicates that the model relied primarily on:

* Days with impressions
* Log impressions (90 days)
* Average search position
* Content age
* Character count
* Word count
* Log clicks
* CTR
* Scroll rate
* Days with sessions

These results suggest that historical search visibility, ranking performance, and content freshness provide useful signals for identifying potentially declining pages.

Some variables, such as search volume and competition, contributed relatively little compared with visibility and engagement features.

## 7. Recommendation

The final output is a ranked refresh queue ordered by predicted refresh priority.

Editors should begin reviewing pages at the top of the queue because these pages have the highest measured refresh scores.

Each recommendation includes explanation codes, including:

* declining_with_demand
* low_ctr_visible_page
* model_decline_risk
* visible_page
* refresh_and_review_ctr

These recommendations are intended to support editorial review rather than automatically updating website content.

The measured evaluation suggests that the model can improve prioritization within this dataset. However, editorial review remains necessary before publishing any changes.

## 8. Reproducibility

The project can be reproduced from a fresh repository clone using:

* python scripts/01_prepare_features.py
* python scripts/02_baseline_score.py
* python scripts/03_train_model.py
* python scripts/04_evaluate_and_export.py
* python scripts/05_build_pdf_report.py

The repository includes:

* Feature engineering scripts
* Model training scripts
* Evaluation scripts
* Export scripts
* Generated evaluation metrics
* Ranked refresh queue
* Model reports

Random seeds were fixed throughout the internship where applicable to improve reproducibility.

The evaluation outputs, including model_results.json, refresh_queue.csv, and supporting charts, are committed so that reported metrics can be independently verified.

## 9. Acknowledgments & Data Credit

This project was built using the FlyRank ML Internship dataset.

The dataset and internship materials were provided by FlyRank.
https://flyrank.ai