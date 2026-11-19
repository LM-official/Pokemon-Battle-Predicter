# One-page report workspace

Edit `main.tex` and `chapters/`; keep `template/` as the original reference.
The supplied template requires English, two columns, and at most **one page
excluding references**. The existing chapter inputs and local style files
were correctly connected. The starter now has a real title, a compact author
placeholder, an abstract, four sections, and a small results table.

## Build and preview

From the repository root:

```sh
latexmk -pdf -interaction=nonstopmode -halt-on-error -cd Report/main.tex
```

Open `Report/main.pdf`. From inside `Report/`, build with:

```sh
latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
```

A TeX distribution with `pdflatex` and `latexmk` is required. Chapter files
include a TeX root directive for editors that support it, so building while
editing a chapter targets `main.tex`.

The PDF and LaTeX intermediate files are ignored by Git. The directory uses
the tracked spelling `Report/`, including the capital `R`, so the same build
command works on case-sensitive systems.

## Where to write

| File | Content | Approximate word budget |
| --- | --- | --- |
| `main.tex` | Authors/team, abstract, final approach and main result | 50–70 |
| `chapters/1_features.tex` | Ten feature categories, selection, evidence | 140–170 |
| `chapters/2_models.tex` | Six tested model families and refinement | 70–100 |
| `chapters/3_results.tex` | K-fold validation, metrics, score summaries, final performance | 180–220 |
| `references.bib` | Sources you actually cite | Excluded from page limit |

The prose is a factual starting point, not a completed report. Replace all
visible red `TODO`s, author placeholders, and table dashes. Keep only the most
informative experiments; rebuild as you write and check the page count.
The structure follows the requested feature categories, tested models,
validation/metrics, and final results for Dani, Lore, and Filippo. Compact
lists leave space for your interpretation within the one-page limit.

## Requested writing structure

- **Feature categories:** base stats; held items; type coverage, weaknesses,
  and type multipliers; team health; faint evidence; statuses; boosts;
  battle effects; deployment, team levels, tactical tempo, and hazards;
  30th-turn snapshots.
- **Models tested:** linear regression; random forest; LightGBM classifier;
  XGBoost; CatBoost; ensemble. The notebook currently implements
  `LogisticRegression`, which differs from linear regression. The draft
  includes a TODO to confirm the name or describe a separate linear
  regression experiment. Random forest and any earlier linear regression
  experiments need to be described from your actual experiment history.
- **Validation:** k-fold cross-validation, with the notebook's repeated
  stratified setup already specified.
- **Metrics:** accuracy, precision, recall, F1, and probability-based ROC-AUC,
  with definitions in the draft.
- **Score summaries:** training mean, validation mean, validation population
  standard deviation, and OOF score.
- **Final performance:** Dani (`lr_dani`), Lore (`ens_lore`), and Filippo
  (`lr_fil`), with an accuracy table and prompts for leaderboard results.

## Verified facts from `src/my-notebook.ipynb`

- `lr_dani`: logistic regression, `FEATURES_DANI`, 76 unique predictors.
- `lr_fil`: logistic regression, `FEATURES_FIL`, 84 unique predictors.
- `ens_lore`: equal-weight soft voting over LightGBM, XGBoost, CatBoost, and
  logistic regression, `FEATURES_LORE`, 119 unique predictors.
- Boosting: 150 iterations, depth 5, learning rate 0.04. Logistic regression:
  `C=1.0`, `max_iter=1000`.
- Validation: 3 repeats of 5 stratified folds, split seed 42, shared splits
  across configurations. Numeric imputation, encoding, and scaling are
  learned within folds. Predictions use threshold 0.5.
- Each model is refitted on all training data and writes its own submission
  CSV, matching predictions to the sample submission by battle ID.

The notebook currently has **no saved cell outputs**. It specifies fixed
features and model settings, but does not record how they were selected or
which earlier experiments succeeded. Supply that history from your real
experiments and use measured scores; the draft intentionally leaves those
parts open. It does not claim that any configuration won or improved accuracy.

## Fill the results table

After a successful notebook run, its `cv_outputs` dictionary contains one
summary per configuration. In a new notebook cell you can inspect:

```python
for label, (model_name, _, _) in MODEL_CONFIGS.items():
    summary = cv_outputs[label][2]
    scores = summary.loc[(model_name, "accuracy")]
    print(
        f"{label}: CV {scores['validation_mean']:.4f} "
        f"+/- {scores['validation_std']:.4f}; OOF {scores['oof']:.4f}"
    )
    print(f"  Training mean: {scores['train_mean']:.4f}")
```

The CV standard deviation is the population standard deviation across the
15 fold scores (`ddof=0`). OOF accuracy is computed after averaging each
battle's three out-of-fold probabilities and thresholding at 0.5; it is
different from the mean fold accuracy. These are local validation results,
not leaderboard scores. Read leaderboard results from Kaggle if available,
and state when a private score is unavailable. A submission CSV alone cannot
establish accuracy.

Explain feature selection and tuning honestly: comparing choices on these
folds can make their scores optimistic; it is not an independent final test.
Describe the asymmetry between the fully known Player 1 team and partially
observed Player 2 team, including the uncertainty in inferred items/knockouts.

## References and final checks

Add real entries to `references.bib`, insert `\cite{...}` where relevant, and
uncomment the bibliography block in `main.tex`. It is disabled initially to
avoid an empty References heading and BibTeX warnings. If references spill
beyond the text page, you may put `\clearpage` immediately before the block;
the report body itself must still fit on one page.

Before submission, replace the results caption with a descriptive final
caption, remove all TODOs/placeholders, verify that claimed experiments and
scores match the submitted code, and inspect the rendered PDF for overflow.
Keep the supplied 10 pt font, margins, and two-column layout.
