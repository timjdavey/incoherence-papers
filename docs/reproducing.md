# Reproducing the figures

This page says which notebook and saved file made each figure in the two papers, what you need to
run them and what no longer runs.

Each figure was matched by reading its caption against the notebook cell that draws it and the
file that cell saves. Nothing was regenerated for this page. Paths are relative to `notebooks/`.

## The Incoherence paper

> Davey, T. (2024). *Entropy*, 26(8), 683. https://doi.org/10.3390/e26080683

| Figure | What it shows | Notebook | Saved file | Seed |
| :-- | :-- | :-- | :-- | :-- |
| 1 | `[(1−x), x]` against `[1, 0]` | `chi_squared/Discrete comparison` | `chi_squared/discrete-a.eps` | Not random |
| 2 | An identity matrix of size `x` | Same | `chi_squared/discrete-c.eps` | Not random |
| 3 | One `[0.5, 0.5]` among `x` many `[1, 0]` | Same | `chi_squared/discrete-b.eps` | Not random |
| 4 | `x` many duplicated distributions | Same | `chi_squared/discrete-d.eps` | Not random |
| 5 | Lowest against highest entropy, over `x` states | Same | `chi_squared/discrete-e.eps` | Not random |
| 6 | Two lowest entropy distributions, over `x` states | Same | `chi_squared/discrete-f.eps` | Not random |
| 7 | A shift in mean, a shift in spread and the full range | `chi_squared/Continuous comparison` | `chi_squared/cont-progs.eps` | None set |
| 8 | Few points, an identity matrix and duplicates | Same | `chi_squared/cont-flat.eps` | None set |
| 9 | Cellular automaton rule 218 | `incoherence/4.1. Disorder - Cellular Automata` | `incoherence/figures/4.1-ca-218.eps` | 4020 |
| 10 | Rule 0 | Same | `incoherence/figures/4.1-ca-0.eps` | 4020 |
| 11 | Rule 30 | Same | `incoherence/figures/4.1-ca-30.eps` | 4020 |
| 12 | Random graphs at their tipping point, 20 and 100 nodes | `incoherence/4.3. Tipping points - ER graphs` | `incoherence/figures/ergraph-critical.eps` | None set |
| 13 | Standard against modified random graphs, 50 nodes | `incoherence/4.1. Disorder - ER graphs` | `incoherence/figures/ergraph.eps` | None set |
| 14 | Rule 226 at `p = 0.50` | `incoherence/4.3. Tipping points - Cellular Automata` | `incoherence/figures/4.3-ca-p50.eps` | 4020 |
| 15 | Rule 226 at `p = 0.73` | Same | `incoherence/figures/4.3-ca-p73.eps` | 4020 |
| 16 | Rules 226, 0 and 30 across every `p` | Same | `incoherence/figures/4.3-ca-tipping.eps` | 4020 |
| 17 | Daisyworld across luminosities | `cohesion/Cohesion Daisyworld example` | `cohesion/4.4-daisy-scan.eps` | None set |
| 18 | Each Daisyworld trial at luminosity 0.83 | Same | `cohesion/seven-nine.eps` | None set |
| 19 | The factory example | `cohesion/Illustrative graphs` | `cohesion/incoherence-example.eps` | None set |

Some rows need a note.

- **Figures 1 to 6.** The letters in the file names follow the order of the cases in the
  notebook, not the order of the figures. That is why Figure 2 is `c` and Figure 3 is `b`.
- **Figure 13.** The notebook saves `ergraph.eps` three times. The last save, with 50 nodes and
  100 trials, is the one that matches the caption.
- **Figures 17 and 18.** These were drawn by the Cohesion paper's Daisyworld notebook, not by
  `incoherence/4.4 Self Organised Criticality - Daisyworld`. They read
  `datasets/daisy_adjusted.pickle`, which is not in the repository.
- **Figure 19.** The notebook also saves a wide version, `cohesion/incoherence-example-horz.eps`.
  The distributions are drawn from normal distributions. `incoherence/FAB` holds the factory
  simulation itself and saves no figure.
- **Sections 5.4 and 5.5** have no figure of their own. Their results come from
  `incoherence/4.5. Perturbation - Daisyworld` and `incoherence/4.6. Diversity - Daisyworld`. The
  `4.5-daisy-*.eps` and `4.6-daisy-*.eps` files those notebooks saved are not in the paper.

## The Cohesion paper

> Davey, T. (2023). *Entropy*, 25(12), 1605. https://doi.org/10.3390/e25121605

| Figure | What it shows | Notebook | Saved file | Seed |
| :-- | :-- | :-- | :-- | :-- |
| 1 | The kinds of uncertainty, as a swarm plot | `cohesion/Illustrative graphs` | `cohesion/aleatoric.eps` | None set |
| 2 | The factory example, with cohesion | Same | `cohesion/cohesion-example.eps` | None set |
| 3 | The entropy estimator on values spread over `[0, x]` | `continuous_entropy/Dimensional invariance` | `continuous_entropy/block-compare.eps` | None set |
| 4 | The same, for a spike at 0.5 mixed with a uniform spread | Same | `continuous_entropy/perfect-compare.eps` | None set |
| 5 | Four messages made of burst patterns | `cohesion/Language detection` | `cohesion/burstv2.eps` | 44, 52 and 32 |
| 6 | The same four, coloured by pattern | Same | `cohesion/starburstv2.eps` | 44, 52 and 32 |
| 7 | Daisyworld temperatures across luminosities | `cohesion/Cohesion Daisyworld example` | `cohesion/temperature.eps` | None set |
| 8 | Daisyworld species across luminosities | Same | `cohesion/plants.eps` | None set |

Figures 7 and 8 also read `datasets/daisy_adjusted.pickle`.

## What you need

**Python and packages.** The figures were made on Python 3.9, which you can see in the paths
saved in the notebooks' output. `requirements.txt` pins nothing. The example in
[Using `ensemblepy`](ensemblepy.md) and the passing tests run on Python 3.11 with this set:

| Package | Version |
| :-- | :-- |
| Mesa | 2.4.0 |
| numpy | 2.4.6 |
| scipy | 1.17.1 |
| pandas | 3.1.1 |
| seaborn | 0.13.2 |
| matplotlib | 3.11.2 |
| networkx | 3.6.1 |
| cellpylib | 2.4.0 |

Mesa has to stay below 3. The simulations use `mesa.time.RandomActivation`, which Mesa 3 removed.

```sh
pip install -r requirements.txt "mesa<3"
```

**Datasets.** Five notebooks read files from `datasets/`, which is gitignored. None of the
original files survive.

| File | How to get it | Read by |
| :-- | :-- | :-- |
| `daisy_adjusted.pickle` | `python simulations/daisy_world/generate_adjusted.py` | `cohesion/Cohesion Daisyworld example`, `incoherence/5. Daisyworld chi2 anova` |
| `daisy_standard.pickle` | `python simulations/daisy_world/generate_standard.py` | `incoherence/4.4 Self Organised Criticality - Daisyworld` |
| `datasaurus.csv` | The notebook tries a public URL first | `correlation_coeff/Results` |
| `rotten_tomatoes_movies.csv` | Not recorded | `incoherence/Movie reviews` |

Run the two scripts from the repo root, and make the `datasets/` folder first. Both use every
core and set no seed.

| Script | Luminosities | Trials at each | Steps |
| :-- | :-- | :-- | :-- |
| `generate_adjusted.py` | 201, from 0.45 to 1.45 | 30 | 400 |
| `generate_standard.py` | 101, from 0.48 to 1.35 | 20 | 400 |

## What to expect

The notebooks are a record of what was run, not a pipeline you can replay. The outputs saved
inside each one are the published runs.

- **Most notebooks do not run from top to bottom on the current code.** A check in November 2026
  ran 13 of the 28 on the versions above, and one finished. The 12 in `incoherence/` and three
  others were not tried.
- **Without a seed you get the same shape, not the same decimals.** Only the cellular automata
  and burst pattern figures set one.
- **The Daisyworld figures cannot be matched exactly.** The data behind them is gone and the
  scripts that make it are unseeded.

The notebooks behind published figures stopped for these reasons.

| Notebook | Figures | Why it stops |
| :-- | :-- | :-- |
| `chi_squared/Discrete comparison` | Incoherence 1 to 6 | A cell calls `create_data`, and the function is defined as `create_df` |
| `cohesion/Illustrative graphs` | Incoherence 19, Cohesion 1 and 2 | It reads `.cohesion`, which the current code no longer works out by default |
| `cohesion/Language detection` | Cohesion 5 and 6 | The same |
| `cohesion/Cohesion Daisyworld example` | Incoherence 17 and 18, Cohesion 7 and 8 | The dataset is missing |
| `continuous_entropy/Dimensional invariance` | Cohesion 3 and 4 | A `TypeError` when it formats a string |

[Using `ensemblepy`](ensemblepy.md) explains the change to cohesion and names the last commit
before it.

`incoherence/4.3. Tipping points - Cellular Automata` was not tried, but expect it to stop too.
It calls the automata model's `analyse()`, which fails in the tests below. That notebook made
Incoherence Figures 14 to 16.

## The tests

`python -m unittest` finds 17 tests and 5 pass. The rest predate later refactors.

| Test | Result | Why |
| :-- | :-- | :-- |
| `test_bins.test_binint`, `test_bins.test_ergs` | Pass | |
| `automata.test_creation`, `wealth.test_basic`, `daisy_world.test_netlogo` | Pass | |
| `test_base`, `test_series` | Error | They import `ensemblepy.base`, which was removed |
| `test_scan` | Error | It imports `ensemblepy.series`, which was removed |
| `test_densityvar.test_constants`, `test_densities` | Error | They use the constants `MIN_VARIANCE` and `STEP_COUNT`, which were removed |
| `test_entropy.test_weights` | Error | It calls `observation_weights`, which was removed |
| `test_stats.test_measures` | Error | It passes `with_meta`, which `measures` no longer takes |
| `test_correlation.test_specific` | Error | A `TypeError`, from subscripting a method |
| `automata.test_r2e` | Error | It passes `units`, which the entropy function no longer takes |
| `automata.test_run_analysis` | Error | The automata model calls `_incoherence` without its `maxent` argument |
| `test_bins.test_binspace` | Fail | Log spaced bins give 0.240 where the test expects 0.316 |
| `test_correlation.test_thresholds` | Fail | Incoherence comes back as `nan` when every `y` value is the same |

## Still to confirm

These are places where the caption and the notebook do not line up, and the notebook was not
rerun to settle it.

- **Incoherence Figures 17 and 18.** The captions say 20 trials at each luminosity. The script
  that makes `daisy_adjusted.pickle` runs 30.
- **Cohesion Figure 7.** Two cells save `temperature.eps`. The caption describes three panels
  with grey dotted lines at 0.53 and 1.28. One cell draws five panels with those lines and the
  other draws three panels without them.
