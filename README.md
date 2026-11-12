# Can't Stop: optimal policy and heuristics

A study of the dice game *Can't Stop* (Sid Sackson). We compute an optimal strategy with dynamic programming. We then compare it with simple heuristics, and we tune some of them with genetic algorithms. A small web app lets you play and compare strategies.

This is an IMI project, done by a team of five students.

<!-- TODO: add the course, the year and the supervisor if you want. -->

## The game

In *Can't Stop*, you roll four dice and split them into two pairs. Each pair sum (2 to 12) is a column. You move forward in the columns you choose. After each roll, you can **stop** and keep your progress, or **keep rolling**. If no pair is playable, you **bust** and lose the progress of the turn.

We study a one-player version: **complete 3 columns in as few turns as possible.**

## What we did

1. **Optimal policy.** For 3 open columns, we compute the best "stop or continue" decision by dynamic programming (with `numba`). The results are saved in `data/` (`V.npy` and `policy.npy`).
2. **Heuristics.** We implement several strategies and benchmark each one over 5000 simulations. They include two naive baselines, "minimize the number of openings", a probabilistic score, "minimize the bust probability", the *Rule of 28*, constant weights, and a heuristic based on the dynamic programming policy.
3. **Genetic algorithms.** We tune the weights of the heuristics. One notebook follows the *Linear Weights* genome of Glenn and Aloi.
4. **Web app.** An interface to play solo and to compare strategies (see below).

![Distribution of the number of turns to finish, 5000 simulations per strategy](benchmark_heuristics.png)

*Number of turns needed to complete 3 columns, for each strategy. Lower is better.*

![Optimal stop or continue policy from dynamic programming](dp3col_policy.png)

*Optimal stop-or-continue policy from dynamic programming, for columns 6, 7 and 8 (progress in column 8 fixed at 3, 6 and 9).*

## Notebooks

The notebooks and the app are partly in French.

| Notebook | Content |
| --- | --- |
| `three_columns_numba.ipynb` | Dynamic programming for 3 open columns, minimum number of rolls, simulations and Monte Carlo. |
| `heuristics_opening.ipynb` | Implementation and benchmark of the heuristics, result tables, distributions and duels between heuristics. |
| `optimized_rule_of_28.ipynb` | Genetic algorithm that tunes the parameters of the constant-weights heuristic. |
| `paper_faithful_linear_weights_cant_stop_ga.ipynb` | Genetic algorithm for the *Linear Weights* genome, following Glenn and Aloi. |
| `simulateur_jeu_complet_1_joueur.ipynb` | One-player simulator of the full game (all columns). |
| `simulateur_toutes_colonnes_1_joueur.ipynb` | A short earlier draft of the full-game simulator. |

### Run the notebooks

You need Python 3.

```bash
pip install -r requirements.txt
jupyter lab
```

The notebooks read and write files relative to the root of the repository. Keep them there, and start Jupyter from the root.

## Web app

The app is in `app/`. It is plain HTML and JavaScript, so there is nothing to build. Open `app/index.html` in a browser.

It has five views: home, solo game, duel against a heuristic, two players, and heuristic against heuristic.

To run the tests of the game rules, you need Node.js:

```bash
node app/rules.test.js
```

`avec_H6/` is a variant of the app with a different set of heuristics.

## Repository structure

```text
app/                  Web app (HTML, CSS, JavaScript) and its tests
avec_H6/              Variant of the web app
data/                 Dynamic programming results (V.npy, policy.npy)
heuristics_opening/   Simulation results of the heuristics (.npy) and figures
*.ipynb               Notebooks (see the table above)
*.png, *.npy          Figures and results written by the notebooks
```

The branch `one-dimensional-approximation` explores a one-dimensional approximation of the problem.

## Authors

- Camille Bernard
- Tristan Bonotte
- Alexandre Fiolle
- Maissa Frikha
- Léo Garnaud
- Maxime Tran
 

<!-- TODO: add each person's role. From the commit history, the main contributions look like this:
- Camille: first 3-column code, one-player simulator, genetic optimization of the Rule of 28, web app.
- Léo: simulation of the optimal policy, comparison with heuristics, weights, minimum number of rolls.
- Maissa: roll computations, dynamic heuristic, duels between heuristics, heuristics in the app.
- Maxime: Monte Carlo and storage of V, opening heuristics, DP-3-columns heuristic, plots and final tables.
- Alexandre: one-dimensional dynamic programming and heuristic (branch one-dimensional-approximation). -->

## References

- Glenn and Aloi, *A Generalized Heuristic for Can't Stop*.
