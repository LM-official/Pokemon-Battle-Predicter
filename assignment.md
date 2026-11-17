# Pokémon Battle Prediction

## FDS Kaggle Competition

Fundamentals/Foundations of Data Science
Sapienza University of Rome
I Semester 2026-2027

---

## Why Pokémon?

Have you ever watched a competitive match and tried to guess the winner? That's what we're going to do.

Pokémon hides complex battle mechanics and strategies behind cute characters; sometimes predicting the outcome can be surprisingly difficult!

Your task is to train a machine learning model that can predict the final outcome of a Pokémon battle from an incomplete observation of the match.

---

## Pokémon 101

To complete the task, you need to understand the "features" of a battle. This challenge uses battles in the Pokémon Gen 2 OU format.

Each Pokémon has six base stats, namely:

- **HP:** how much damage a Pokémon can endure.
- **Attack and Defense:** damage dealt or received when using a physical move.
- **Special Attack and Special Defense:** damage dealt or received when using a special move.
- **Speed:** determines who attacks first.

---

## Additional mechanics

Types, moves, and held items provide additional information. Every Pokémon has at least one type, and each type has some sort of (dis)advantage over another.

For example, a Water attack is "super effective" on a Fire Pokémon (i.e., it deals 2x damage). We'll provide you the type chart.

Moves don't just hurt the opponent, as they may change the status of the Pokémon (i.e., make it sleepy, poisoned, burned…).

---

## Okay…but what is the challenge?

Formally, this is a supervised binary classification challenge in which you'll predict the boolean target, ***player_won***.

The target is present in the training data and absent from the test data.

---

## Input: what you will have

- Input data includes battles last between 30 and 80 total turns.
- **You receive only the first 30 active turns of each battle**; any later turns are hidden.
- You are not told the total length of an individual battle.
- The target describes the final result, rather than who appears to be ahead at turn 30; remember, the underdogs can win.

---

## Output: what we want

Submit one binary prediction for every test battle:

- `1` → Player 1 wins
- `0` → Player 1 loses

Use the provided **sample_submission.csv** format and keep each **battle_id** paired with its prediction.

---

## Battle structure

The main fields are:

- `battle_id`: the battle identifier.
- `p1_team_details`: details of all six Pokémon on Player 1's team, including their base stats, types, and held items.
- `p2_lead_details`: details of the Pokémon with which Player 2 starts the battle.
- `battle_timeline`: the first 30 active turns, including recorded Pokémon states and move information.

---

## Getting started

First of all, find your teams and join the competition via this link using your institutional email.

You'll find a student starter notebook in the "Discussion" tab to customize with your solution.

The starter provides a working reference of how you should format your solution. Improve it through feature engineering, model comparison, and careful validation.

---

## Leakage

Training accuracy tells you how well your model fits data it has already seen. It does not provide a reliable measure of performance on new battles.

You can't use the test set to select features or tune your model, as this would "leak" info and make your scores unreliable.

So, how do you know if your model is doing good? How do you select features and tune hyperparameters?

---

## Validation

The solution is to create your own validation subset from *train.jsonl*. You use this validation set to compare models and make all your decisions.

**Use and trust your local validation to make decisions.** Treat the public leaderboard feedback as an occasional *sanity check,* rather than a replacement for your validation process.

A little hint…*k-fold*. ;)

---

## A few rules

- Only classical machine learning algorithms are allowed. Neural networks are excluded.
- Use the competition data and the supplied Generation 2 type chart. External databases, additional replay datasets, usage statistics, and manually collected tables are prohibited.
- Models pretrained on external data for this task are also prohibited.

---

## …and some more rules

- Follow the format in `sample_submission.csv` and respect the published submission limits (3 per day).
- Discussing ideas and strategies on the competition is encouraged. Private sharing of code between teams is not permitted.
- Remember to select 3 submissions before the competition closes for scoring.

---

## Evaluation

The grade for the challenge is pass or fail. Passing the challenge gives you a chance to complete the final project (more details later).

We evaluate:

1. **Performance:** Your final performance on the private leaderboard will be a key factor in your evaluation. A higher Accuracy score will contribute positively towards your final grade.
2. **Methodology:** A Report detailing your complete methodology. This is your chance to showcase the breadth and depth of your work.

---

## More on the report

The report should focus on the highlights of your work, telling us which experiments you ran.

It should cover areas such as:

- Feature Engineering & Selection
- Model Experimentation & Refinement
- Validation Strategy

**Code Reproducibility is mandatory.** After the deadline, you must submit your code. It must be able to reproduce the results of your final submissions. Well-commented code is appreciated.

---

## Timeline and deadlines

- **Competition starts:** 23/10/2026
- **Competition ends:** 05/11/2026, 23:59

You must **send us your materials**, including:

- a **public** Kaggle notebook
- report

**by 12/11/2026**, using this form.

---

## Good practices (and more rules)

The Kaggle notebook is submitted in the form, remember that:

- You must make it public in the competition after the end of the competition.
- It must run and generate your selected 3 final submissions.
- It must clone your GitHub repo (no copy-pasting code).
- Please add a brief markdown cell before each submission to explain the approach.

---

## Submission resources

**How to Create Modules:** For those who'd like a guide on structured code or a refresher on Python modules, here is the official documentation.

**Demo Notebook:** This example notebook shows you exactly how to load, path, and use a GitHub repo inside Kaggle.

---

## TL;DR?

### FDS Kaggle Challenge: Pokemon Battle Prediction

*Gen 2 OU / Quick Reference*

**Battle length and observation:** 30 to 80 total turns

| First 30 shown | Later turns hidden |
|---|---|

Every included battle lasts 30 to 80 turns. You receive only its first 30 active turns.

**Input: Team context and opening turns**
Player 1's team, Player 2's lead, and the first 30 active turns of the battle.

**Predict: Final result for Player 1**

- `1` Player 1 wins
- `0` Player 1 loses

**Train:** 10,000 labeled battles
**Test:** 5,000 unlabeled battles
**Submission:** One prediction per test battle. Keep battle_id. Use sample_submission.csv

---

## Our contacts

- Siria Sannino
  - sannino.2001580@studenti.uniroma1.it
- Riccardo Bastiani
  - bastiani.2197391@studenti.uniroma1.it

---

## Good luck!

"Me, give up? No way!" — Ash Ketchum
