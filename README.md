# Game of Thrones Survival Prediction

**One-liner:** EDA, feature engineering and model selection for a binary classification task — predict whether a Game of Thrones character survives.

---

## Repository structure

<div style="border:1px solid #ddd; padding:12px; border-radius:6px; background:#f8f8f8;">
<pre>
README.md
notebook.ipynb
requirements.txt
dicts.json         # grouping variants for 'culture' and 'title'
submission.csv     # predictions for the test set (accuracy: 0.7557840616966581)
</pre>
</div>

---

## What this project does

This repository contains a runnable Jupyter notebook that implements a complete supervised learning pipeline for predicting whether a *Game of Thrones* character is alive in the final book. The notebook performs:

- Data loading and cleaning (handling missing values and inconsistent date formats).
- Exploratory data analysis (visual summaries, missing-value analysis, class balance checks).
- Flexible, reproducible feature engineering driven by `load_config(...)`:
- Categorical grouping via `dicts.json` (title / culture mapping variants are injected into the pipeline).
- Encoding options (one-hot or target encoding) and configurable pipelines for preprocessing.
- Model selection and hyperparameter tuning (see section below).
- Final training on chosen configuration and creation of `submission.csv`.

The notebook is organized to be configurable (via `load_config`) and reproducible: train / validation split is stratified, experiments are recorded in a results table, and best estimators are saved.

---

## Feature engineering highlight

Key feature-engineering steps implemented in the notebook:

- Processed dates/ages and added a missing-data flag.
- Grouped titles and cultures using mapping variants from `dicts.json`.
- Built relational (family) features, book-presence indicators and a popularity proxy.
- Configurable preprocessing pipelines for scaling and encoding (one-hot / target).

---

## Model selection & training (accurate)

Model selection and tuning approach actually implemented in the notebook:

- Hyperparameter tuning via `RandomizedSearchCV` wrapped in `train_models_randomsearch(...)` with a stratified hold-out split.
- Comparison and tuning of several classifiers: LogisticRegression, RandomForestClassifier, DecisionTreeClassifier, AdaBoostClassifier, GaussianProcessClassifier, GaussianNB, KNeighborsClassifier and SVC.
- Classifiers are compared and results (best params + validation accuracy) are recorded.
- Best estimator is trained and used to generate `submission.csv` (test accuracy reported: **0.7557840616966581**).

---

## Notes about `dicts.json` (explicit variants)

`dicts.json` stores alternative mapping dictionaries for `culture` and `title`. Grouping can be chosen via `load_config(...)`. Typical grouping axes used in experiments include **region/continent**, **societal type or culture family**, **functional role or title function**, and **rank/level**.

Switching between these mapping variants is part of the feature-engineering experiments.

---

## How to run

Clone the repository:

```bash
git clone <repo-url>
cd <repo-name>
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Open the notebook:

```bash
jupyter lab   # or `jupyter notebook`
# then open notebook.ipynb
```

---

## Results

- **Final submission file:** `submission.csv`  
- **Test accuracy achieved:** **0.7557840616966581**
