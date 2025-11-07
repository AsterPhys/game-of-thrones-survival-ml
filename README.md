# Game of Thrones Survival Prediction

**One-liner:** EDA, feature engineering and model selection for a binary classification task — predict whether a Game of Thrones character survives.

---

## Repository structure

- **README.md**  
  Project README (this file).

- **notebook.ipynb**  
  Jupyter notebook with the full pipeline:
  - EDA
  - preprocessing / feature engineering
  - model selection / training
  - evaluation
  - submission generation

- **requirements.txt**  
  Python packages used to run the notebook.

- **dicts.json**  
  Contains alternative groupings/mappings for `culture` and `title` features used in experiments.

- **submission.csv**  
  Final predictions for the test set. Measured accuracy on the test set: **0.7557840616966581**.

---

## What this project does

This is a binary classification project that answers the question: **Will a Game of Thrones character survive?**

**Main steps performed in the notebook:**

- Exploratory Data Analysis (visuals, missing value analysis, class balance)  
- Feature engineering (title/culture grouping, relational features, date parsing, boolean flags)  
- Model selection and hyperparameter search (compare several classifiers, use randomized search where applicable)  
- Train best model on training data and evaluate on validation/test sets using accuracy  
- Produce `submission.csv` with predictions for the test dataset

**Goal / target metric:** achieve **accuracy > 0.75** on the test dataset.

---

## Data description

Each row corresponds to a character.

### Columns (short description)

| Column | Description |
|---|---|
| `name` | Character name |
| `Title` | Social rank / title of the character (e.g. "Lord", "Queen") |
| `House` | Noble house to which the character belongs |
| `Culture` | Cultural/social group of the character |
| `book1`, `book2`, `book3`, `book4`, `book5` | Indicators (presence) of the character in the respective books |
| `Is noble` | Boolean flag indicating nobility derived from `Title` |
| `Age` | Age relative to reference (305 AC) |
| `male` | Boolean (male / female) |
| `dateOfBirth` | Date of birth (as available) |
| `Spouse` | Name of the spouse (if any) |
| `Father` | Name of the father (if any) |
| `Mother` | Name of the mother (if any) |
| `Heir` | Name of the heir (if any) |
| `Is married` | Whether the character is married (boolean) |
| `Is spouse alive` | Whether the character's spouse is alive (boolean) |
| `Is mother alive` | Whether the character's mother is alive (boolean) |
| `Is heir alive` | Whether the character's heir is alive (boolean) |
| `Is father alive` | Whether the character's father is alive (boolean) |
| `Number dead relations` | Number of dead relatives (relations that have died) |
| `Popularity score` | Count of internal incoming + outgoing links to the character page on *A Wiki of Ice and Fire* — used as a proxy for character prominence |
| `isAlive` | **Target variable** — whether the character is alive in the (final) book (binary label used for supervised learning) |

---

## Feature engineering highlights

- **Title / Culture grouping:** use `dicts.json` to map many sparse categorical values into broader categories. Multiple mapping variants are tested as part of experiments.  
- **Relational features:** flags and counts derived from family/spouse/heir/father/mother columns (e.g., number of dead relatives, whether heir is alive).  
- **Date parsing:** parse `dateOfBirth` to derive age where possible and standardize missing/approximate dates.  
- **Boolean flags:** `Is noble`, `Is married`, `male`, presence indicators for books, etc.  
- **Popularity:** use internal link counts as a proxy for character prominence.

---

## Model selection & training

- Compare several classifiers (e.g., logistic regression, tree-based models, ensemble methods).  
- Use randomized search (or similar) for hyperparameter tuning where applicable.  
- Evaluate models on validation/test splits using **accuracy**; track experiments to select best model.  
- Train chosen model on training data and produce final predictions for the test set.

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

## Notes about `dicts.json`

`dicts.json` contains predefined grouping dictionaries used by the notebook for:

- mapping different `culture` values into broader groups, and  
- mapping various `title` strings into canonical title categories.

Switching between mapping variants (there can be multiple grouping strategies in the file) is part of the feature-engineering experiments and may materially affect model performance.

---

## Results

- **Final submission file:** `submission.csv`  
- **Test accuracy achieved:** **0.7557840616966581**

---

## Files of interest

- `notebook.ipynb` — full, runnable pipeline (recommended starting point)  
- `requirements.txt` — environment reproducibility  
- `dicts.json` — grouping variants used during feature engineering  
- `submission.csv` — final model predictions (accuracy reported above)

---

Если нужно, могу добавить таблицу с метриками по моделям / примеры кода фрагментов препроцессинга или экспортировать README в `README.md`-файл готовом для репозитория.
