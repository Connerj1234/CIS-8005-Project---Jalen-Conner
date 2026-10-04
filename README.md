# CIS 8005: Reservation cancellation analysis

This project asks whether reservation attributes in a **synthetic Kaggle dataset** can rank cancellation risk well enough to justify testing a targeted pre-arrival confirmation policy. [Graduate_Cancellation_Presentation.ipynb](Graduate_Cancellation_Presentation.ipynb) is the partner's original, preserved as a reference. [Graduate_Cancellation_Presentation_Explained.ipynb](Graduate_Cancellation_Presentation_Explained.ipynb) is the current presentation revision.

The source is Kaggle's [Binary Classification with a Tabular Reservation Cancellation Dataset](https://www.kaggle.com/competitions/playground-series-s3e7/data), Playground Series Season 3 Episode 7. Kaggle says its train and test files were generated from a model trained on an earlier reservation dataset. Findings here describe this synthetic competition file, not a specific hotel's bookings or the effect of contacting guests. The data page lists the files under a CC BY 4.0 license.

## Repository map

- `Graduate_Cancellation_Presentation.ipynb`: unchanged partner reference. It recreates the data preparation from raw training data, describes lead-time patterns, compares models, evaluates a fixed policy, checks capacity and arrival-year sensitivity, and proposes a prospective pilot.
- `Graduate_Cancellation_Presentation_Explained.ipynb`: current presentation revision with the same model design, shorter commented code cells, accessible explanations, and a simpler lead-time visual. It also includes exploratory grouped-split and zero-price sensitivity checks plus a proposed scoring schedule. Its latest full saved run used scikit-learn 1.4.2, so its selected cutoff and exact metrics differ slightly from the original notebook's 1.9.1 run. Use numbers from one run consistently in presentation material.
- `data/raw/train.csv`: 42,100 labeled reservations used for the analysis and evaluation.
- `data/raw/test.csv` and `data/raw/sample_submission.csv`: unlabeled competition artifacts; they are not used for the project's reported outcomes or model selection.
- `archive/00_data_preparation.ipynb` and `archive/01_data_audit_and_eda.ipynb`: earlier preparation and broader descriptive exploration, retained as project history. Notebook 01 examines market segment, special requests, and price in more detail than the presentation notebook.
- `reports/analysis_handoff.md`: original audit and descriptive findings, followed by the current modeling results and remaining limits.
- `reports/figures/`: figures from the archived EDA notebook.

The old modeling placeholder and duplicate completion workflow were removed because their substantive work is now in the presentation notebook. The archived notebooks remain readable but are not required to run the main analysis.

## Run the notebooks

Install the packages in `requirements.txt` with `python -m pip install -r requirements.txt`. Run the explained notebook from the repository root; it reads `data/raw/train.csv` directly and does not require `data/processed/`. The original partner notebook retains its Windows-specific `ROOT` path, which must be changed to the local project folder before rerunning elsewhere. Both notebooks save tables and figures to ignored `graduate_results/`; their embedded outputs can be read without execution.

The pinned data and modeling packages match the original notebook's saved run, which reports Python 3.14.6, NumPy 2.5.3, pandas 3.0.6, and scikit-learn 1.9.1. The explained notebook's saved run used scikit-learn 1.4.2 and selected 0.18 rather than the original run's 0.21. Exact fitted probabilities and the validation-selected cutoff can change with package versions.

The raw CSVs were downloaded from the competition on September 28, 2026, and kept unchanged. The SHA-256 digest for `train.csv` is `617098a2dcefa187e717f2fba56bb24e0b29f60949a88189ce390087343912ad`.

## What the analysis found

The labeled file has 42,100 rows and a 39.2% cancellation rate. The presentation compares a dummy baseline, logistic regression, decision tree, and random forest using three-fold cross-validation within the fitting partition. Random forest had the highest mean average precision (0.830). On the previously explored 8,420-row random holdout, its ROC AUC was 0.888 and average precision was 0.835.

An illustrative 5:1 penalty for missed cancellations versus false alerts selected a 0.21 cutoff on validation data. On the holdout, that rule found 94.0% of cancellations while flagging 61.6% of reservations. These penalties are assumptions, not observed costs or savings. A 2017-to-2018 arrival-year stress test had lower ROC AUC (0.770), so the random holdout result should not be treated as deployment performance.

The source has a duplicate pattern relevant to the competition: there are **no exact duplicate labeled rows after removing `id`**, but there are **562 pairs with identical predictor values and opposite outcomes** after removing both `id` and `booking_status`. We preserve all rows in the project analysis. The Kaggle competition's duplicate-based leaderboard tactics address a different goal and do not establish a usable hotel policy.

Before an operational claim, verify which fields exist at the scoring time, measure contact capacity and costs, test performance on genuinely new bookings, and estimate the effect of contacting guests. See the notebook's assumption register and recommendation section.
