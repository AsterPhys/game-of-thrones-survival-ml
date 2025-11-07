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
  - parse and standardize `dateOfBirth` / `age` (creates `age_value`, `age_no_data`, `dateOfBirth_value`);
  - create boolean flags for family relations and presence in books (`book1`..`book5`);
  - compute relational counts (e.g., number of dead relatives) and presence flags (`is_married`, `is_spouse_alive`, `is_heir_alive`, `is_father_alive`, `is_mother_alive`);
  - use precomputed **popularity** / link-count as a proxy for character prominence.
- Categorical grouping via `dicts.json` (title / culture mapping variants are injected into the pipeline).
- Encoding options (one-hot or target encoding) and configurable pipelines for preprocessing.
- Model selection and hyperparameter tuning (see section below).
- Final training on chosen configuration and creation of `submission.csv`.

The notebook is organized to be configurable (via `load_config`) and reproducible: train / validation split is stratified, experiments are recorded in a results table, and best estimators are saved.

---

## Feature engineering highlights (accurate)

Key feature-engineering steps implemented in the notebook:

- **Age / date processing**
  - `age_value` — numeric age (filled with 0 where missing);
  - `dateOfBirth_value` — normalized date value placeholder;
  - `age_no_data` — binary flag indicating missing age/date information.
- **Title & Culture grouping**
  - Titles and cultures are mapped using variant dictionaries loaded from `dicts.json` (see Notes below).
  - Grouping variants are pluggable through `load_config(...)`.
- **Relational & family features**
  - Flags for `Is married`, `Is spouse alive`, `Is mother alive`, `Is father alive`, `Is heir alive`.
  - `Number dead relations` — aggregated count of dead relatives.
- **Book-presence indicators**
  - `book1` .. `book5` retained as presence indicators (useful signals for survival likelihood).
- **Popularity**
  - Precomputed link-count (incoming + outgoing internal wiki links) used as `Popularity score` feature.
- **Preprocessing pipeline**
  - The notebook contains helper `pipeline_transform(...)` and optional pipelines applied before model training. Numeric scaling (e.g., `StandardScaler`) and encoding (one-hot / target) are available and used where appropriate.

---

## Model selection & training (accurate)

Model selection and tuning approach actually implemented in the notebook:

- A function `train_models_randomsearch(...)` performs:
  - stratified `train_test_split` (test_size=0.2),
  - optional application of a preprocessing pipeline wrapping the estimator,
  - `RandomizedSearchCV` over provided parameter distributions (`n_iter` configurable),
  - evaluation on the hold-out validation split using **accuracy**.
- Models compared in the experiments include (but are not limited to):
  - `LogisticRegression`, `RandomForestClassifier`, `DecisionTreeClassifier`, `AdaBoostClassifier`,
  - `GaussianProcessClassifier`, `GaussianNB`, `KNeighborsClassifier`, `SVC`.
- Results from each run are collected into a DataFrame with model name, best parameters and validation accuracy; best estimator objects are kept for later use.
- The notebook trains the selected best model and produces `submission.csv` with test-set predictions. The reported test accuracy is **0.7557840616966581**.

---

## Notes about `dicts.json` (explicit variants)

`dicts.json` in this project holds a set of predefined grouping dictionaries used by the notebook. The notebook accepts which mapping to use via `load_config(..., cultures_grouped=..., titles_grouped=...)`.

**Culture grouping variants present in the project:**
- `cultures_grouped`
- `cultures_grouped_by_continent`
- `cultures_grouped_by_societal_type`
- `cultures_variant_region_fine`
- `cultures_variant_by_society`
- `cultures_variant_binary_continent_plus_type`

**Title grouping / normalization variants present in the project:**
- `titles_grouped_by_rank`
- `titles_grouped_by_function`
- `titles_normalized`
- `titles_variant_functional_strict`
- `titles_variant_level_based`
- `titles_variant_binary_flags_for_ml`

Switching between these mapping variants is part of the feature-engineering experiments: different mappings can materially affect model input dimensionality and downstream performance.

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

---

Если нужно, могу добавить таблицу с метриками по моделям / примеры кода фрагментов препроцессинга или экспортировать README в `README.md`-файл готовом для репозитория.
