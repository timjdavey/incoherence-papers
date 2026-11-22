# Using `ensemblepy`

This page shows how to use `ensemblepy`, the library that produced the numbers in both papers.

Use it when you need a published number exactly. For anything else, use the
[`incoherence`](https://github.com/timjdavey/incoherence) package, which is smaller, tested and
faster.

## Setting up

`ensemblepy` is a folder in this repository, not an installed package. Install the dependencies
and run Python from the repo root so that `import ensemblepy` finds it.

```sh
pip install -r requirements.txt
```

The notebooks sit two folders down, so each one starts with `import nbsetup`, which adds the repo
root to the path.

## An example

You pass a list of trials, where each trial is the list of observations from one run of your
system. The example below builds three systems of ten runs each. In the first every run behaves
the same. The second splits into two camps. In the third every run wanders off on its own.

```python
import numpy as np
import ensemblepy as ep

rng = np.random.default_rng(1)

same    = [rng.normal(0, 1, 500) for _ in range(10)]
camps   = [rng.normal(3 if i % 2 else -3, 1, 500) for i in range(10)]
scatter = [rng.normal(rng.uniform(-4, 4), rng.uniform(0.3, 2), 500) for _ in range(10)]

for name, runs in [("same", same), ("camps", camps), ("scatter", scatter)]:
    m = ep.Continuous(runs, metrics=("incoherence", "cohesion"))
    print(f"{name:8} incoherence={m.incoherence:.2f}  cohesion={m.cohesion:.2f}")
```

```text
same     incoherence=0.03  cohesion=0.95
camps    incoherence=0.57  cohesion=0.80
scatter  incoherence=0.45  cohesion=0.30
```

The identical runs are coherent. The two camps are incoherent but highly cohesive, because the
runs disagree in an organised way. The scattered runs are incoherent with little structure to
lean on.

## What you can call

| Call | What you pass | Use it for |
| :-- | :-- | :-- |
| `ep.Continuous(observations)` | A list of trials, each a list of real numbers | Real-valued data. Trials may differ in length. |
| `ep.Discrete(observations, bins)` | The same list, plus the bin edges | Categorical data, or real values you want binned. |
| `ep.Collection(histograms)` | One row of counts or probabilities per trial | Data that is already a set of distributions. |
| `ep.Correlation(x, y)` | Two lists of the same length | Whether `y` depends on `x`. See [Incoherence as a correlation measure](unpublished.md#incoherence-as-a-correlation-measure). |
| `ep.incoherence(discrete, ...)` | `True` or `False`, then the arguments above | One number, with no object to hold. |
| `ep.cohesion(discrete, ...)` | The same | One number. Real-valued data only. |

Each object holds its results as attributes.

| Attribute | What it is |
| :-- | :-- |
| `.incoherence` | The measure, from 0 to 1 |
| `.cohesion` | The measure, from 0 to 1. Only there if you asked for it. |
| `.entropies` | The entropy of each trial |
| `.pooled` | The entropy of all the trials pooled together |
| `.maxent` | The largest entropy the data could have |
| `.divergences` | The divergence between each pair of trials. Comes with cohesion. |
| `.measures` | All of the above in one dictionary |

`.comparison()` returns incoherence beside the other statistics the papers test it against. For
real-valued data those are Kruskal-Wallis, ANOVA, the standard deviation of the means and of the
standard deviations, Jensen-Shannon divergence and the earth mover's distance. For binned data
they are chi², Kullback-Leibler, Hellinger, total variation, Jensen-Shannon and the earth mover's
distance. The real-valued version expects every trial to hold the same number of observations.

## What will trip you up

- **Cohesion is off unless you ask for it.** Pass `metrics=("incoherence", "cohesion")`. Without
  it, reading `.cohesion` raises an `AttributeError`.
- **Cohesion does not work for binned data.** `ep.Discrete` and `ep.Collection` ignore the
  `metrics` argument, and `ep.cohesion(True, ...)` raises a `KeyError`. The calculation is still
  there, so you can call it yourself:

  ```python
  from ensemblepy.stats import _cohesion

  d = ep.Discrete(camps, ep.binspace(-7, 7, 20))
  cohesion, divergences = _cohesion(d.histograms, discrete=True)
  ```

- **The Cohesion notebooks expect the older behaviour.** Cohesion was always worked out until
  commit `8265a14` in June 2024, which made it something you ask for. The notebooks in
  `notebooks/cohesion/` read `.cohesion` without asking, so they stop at that line on the current
  code. `25c085c` is the last commit before the change.
- **Every trial counts the same by default.** The docstrings say `weights=None`, but the default
  is `weights=False`, which gives each trial equal weight. Pass `weights=None` to weight each
  trial by its number of observations. For real-valued data the pooled entropy is always taken
  from all the observations joined into one list, so a longer trial still has more say there.
- **Real-valued data is rescaled to its own range.** `ep.Continuous` scales the observations to 0
  to 1 using the smallest and largest value it is given. To compare several sets of trials on one
  scale, pass the same `normalise=(low, high)` to each. The Daisyworld notebooks do this across
  luminosities.
- **Entropies are in bits.** Pass `base` to change that.

## The settings behind the published numbers

The entropy of real-valued data comes from an estimator in `ensemblepy/densityvar.py`. Its
defaults are the settings the papers' figures were made with.

| Setting | Incoherence, and the entropy of each trial | The last step of cohesion |
| :-- | :-- | :-- |
| Constant `k` | 10 | 100 |
| Reference points | 501, over −0.5 to 1.5 | 501, over −0.5 to 1.5 |
| Power applied to each density | 1 | 0.5 |

The text of the Incoherence paper gives the cohesion settings for both. [Corrections to the
papers](corrections.md) has the details.

The scaling constants for each combination are stored in `CACHED_VS`. If you change `k`, the
range or the number of steps to a combination that is not stored, pass `auto_calculate=True` to
`ep.density_variance` or it raises a `ValueError`.

## How it maps to the `incoherence` package

| `ensemblepy` | `incoherence` package |
| :-- | :-- |
| `ep.Continuous(trials)` | `ic.measure(trials)` |
| `ep.Collection(histograms)` | `ic.measure_counts(counts)` |
| `ep.Discrete(trials, bins)` | Bin the data yourself, then `ic.measure_counts(counts)` |
| `ep.incoherence(...)`, `ep.cohesion(...)` | `ic.incoherence(...)`, `ic.cohesion(...)` |
| `ep.Correlation(x, y)` | Not carried over |
| `.comparison()` and `.chi2()` | Not carried over |
| `ep.plot_series`, `ep.plot_discrete` | Not carried over |
| `ep.density_variance` in more than one dimension | Not carried over |

The two give slightly different numbers. On the ten trials in two camps from the package's
README, `ensemblepy` gives incoherence 0.57 and cohesion 0.79, and the package gives 0.57 and
0.82. [Improvements on the papers](https://github.com/timjdavey/incoherence/blob/main/docs/improvements.md)
lists each change and how far it moves the published figures.
