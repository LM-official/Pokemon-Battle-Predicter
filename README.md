<div align="center">

# ⚔️ Pokémon Battle Predictor
**A machine-learning project for predicting Pokémon battle outcomes.** Using the first 30 active turns of Generation II OU battles, the notebook engineers team and battle-state features, validates several classifiers, and generates Kaggle submissions.

![Language](https://img.shields.io/badge/Language-Python-blue?style=flat-square)
![Task](https://img.shields.io/badge/Task-Binary%20classification-orange?style=flat-square)
![Models](https://img.shields.io/badge/Models-Scikit--learn%20%7C%20LightGBM%20%7C%20XGBoost%20%7C%20CatBoost-informational?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

</div>

---

## ⚔️ What it does

The project predicts whether Player 1 ultimately wins a battle, using the supplied battle record and its observed opening turns. It is a supervised binary classification task: `player_won = 1` means Player 1 wins; `player_won = 0` means Player 1 loses.

The notebook builds features from:

- **Team strength and matchups** — base-stat summaries, type diversity, shared weaknesses, and type effectiveness using the supplied Gen 2 chart.
- **Battle state** — HP loss over turn intervals, remaining HP, statuses, stat boosts, effects, and knockout evidence.
- **Team resources** — held-item evidence, deployed Pokémon and levels, and estimated remaining effective HP.
- **Tactics and tempo** — moves, switches, effective move power, Spikes, phazing, and Rest/Sleep Talk use.

Player 1's full team is available, while only Player 2's lead and battle observations are supplied. Some Player 2 team and item features are therefore inferred from evidence in the timeline.

---

## 🗂️ Project structure

```text
.
├── README.md
├── assignment.md                 # Challenge description and rules
├── requirements.txt
├── dataset/
│   ├── train.jsonl               # Labeled battles (download separately)
│   ├── test.jsonl                # Unlabeled battles (download separately)
│   ├── type_chart_gen2.csv       # Generation II type chart
│   └── sample_submission.csv     # Required submission format
├── src/
│   ├── notebook.ipynb            # Feature engineering, validation, and final models
│   └── starter-notebook.ipynb    # Competition starter notebook
└── report/                       # Project report sources and PDF
```

The dataset directory is excluded from version control. Obtain the competition files and place them in `dataset/` with the filenames shown above. The notebook expects all four files there.

```sh
import kagglehub

# Download latest version
path = kagglehub.competition_download('fds-pokemon-battles-prediction-2026')

print("Path to competition files:", path)
```

---

## 🧰 Setup

Use Python and install the pinned dependencies from the project root:

```sh
python3 -m venv .venv
source .venv/bin/activate       # Windows: .venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Open `src/notebook.ipynb` and run its cells from top to bottom.

---

## 📊 Models and validation

The notebook evaluates three configured submissions:

| Configuration | Model | Feature set | Output |
| --- | --- | --- | --- |
| `lr_dani` | Logistic regression | `FEATURES_DANI` | `submission_lr_dani.csv` |
| `lr_fil` | Logistic regression | `FEATURES_FIL` | `submission_lr_fil.csv` |
| `ens_lore` | Equal-weight soft-voting ensemble: LightGBM, XGBoost, CatBoost, and logistic regression | `FEATURES_LORE` | `submission_ens_lore.csv` |

Each configuration is evaluated with **3 repeats of stratified 5-fold cross-validation** (seed `42`). Preprocessing is fitted inside each fold. The notebook reports accuracy, precision, recall, F1, and ROC-AUC, then fits each configured model on all training battles. Test probabilities are converted to labels at a `0.5` threshold.

---

## 📤 Submission files

The final notebook cell writes one CSV per configuration to the notebook's current working directory. Each file contains the required `battle_id` and `player_won` columns. Predictions are matched back to the sample submission by `battle_id`, preserving its row order.

The Kaggle competition permits classical machine-learning methods only; neural networks and external battle or usage data are excluded. See [assignment.md](assignment.md) for the challenge rules and [report/README.md](report/README.md) for report guidance.

---

## 👥 Authors
Pokemon-Battle-Predicter was designed and build by:
- **[LM-official](https://github.com/LM-official)**
- **[Pierba](https://github.com/Pierba)**
- **[Danile-Zu](https://github.com/Daniele-Zu)**
- **[giamm025](https://github.com/giamm025)**

## 📄 License

Released under the MIT License. See [LICENSE](LICENSE).