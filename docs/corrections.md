# Corrections to the papers

This page records every known mistake in the text of the two papers.

Section 2.2 of the Incoherence paper describes the continuous entropy estimator with the wrong
settings. The Cohesion paper needs no correction.

- **The constant `k`.** The text gives `k = 100`. The Incoherence paper used `k = 10`.
- **The square root.** Equation 3 takes the variance of the square root of each density,
  `Var̂(√G)`. The code took the variance of the densities themselves, `Var̂(G)`.
- **Both come from the Cohesion paper.** They are the settings for the last step of cohesion, and
  were carried over into the Incoherence paper by mistake.

None of the conclusions change, because the paper reports what the code produced. Someone who
builds the estimator from the text however will get a different result.


## What the paper says

Section 2.2, "Continuous Entropy", around equations 2 and 3:

- The density is worked out at `R = 100` reference points with the constant `k = 100`.
- Equation 3 is `H = 1 − (Var̂(√G) − V0) / (V1 − V0)`. It takes the square root of each density
  `g_j` in `G` before it takes the variance.
- The scaling constants are `V0 = 0.0047683913076489465` and `V1 = 0.009711690148675985`, cited
  to the Cohesion paper (reference 18).

## What was actually run

The figures were made with `ensemblepy` in this repository. Its defaults in
`ensemblepy/densityvar.py` have not changed since November 2022, and every notebook behind the
paper's continuous figures calls `ep.Continuous(...)` with those defaults.

| Setting | Paper text | Code that made the figures |
| :-- | :-- | :-- |
| Constant `k` | 100 | 10 |
| Reference points `R` | 100 | 501 |
| Range of the reference points | Not stated | −0.5 to 1.5, after the observations are scaled to 0 to 1 |
| Power applied to each density | Square root, in Equation 3 | None |
| `V0`, maximum entropy | 0.0047683913076489465 | 0.007006952515415692 |
| `V1`, minimum entropy | 0.009711690148675985 | 0.040028626993849875 |

The code asks for 500 steps and a helper function rounds that up to 501 points.

- **The figures confirm there was no square root.** Rebuilt without it, Figure 7A ends at 0.448,
  as published. With the square root it ends at 0.658.
- **So does the code's history.** `ensemblepy/densityvar.py` had no power setting at all before
  30 November 2022. That day it gained `DEFAULT_POWER = 1`, which means no square root, and it
  has not changed since.

## Where the wrong values came from

They are the settings for the last step of cohesion, as described in the Cohesion paper
(*Entropy* 25(12), 1605). That step uses `k = 100` and takes the square root of each density,
because the pairwise divergences it measures are tightly bunched and need a sharper kernel.

The square root in Equation 3 has the same source. Equation 4 of the Cohesion paper is the same
equation, printed for that last step. The code only ever asks for the square root there, in
`_cohesion` in `ensemblepy/stats.py`:

```python
return (1 - density_variance(divergences, power=0.5, k=100)) ** 2, divergences
```

The two constants printed in the Incoherence paper are exactly the ones the code stores for that
case. In `CACHED_VS` in `ensemblepy/densityvar.py`:

```python
# standard: power = 1
(10, (-0.5, 1.5), 500, 1, 1): (0.007006952515415692, 0.040028626993849875),
# for cohesion: power = 0.5, but need higher drop-off so k=100
(100, (-0.5, 1.5), 500, 0.5, 1): (0.0047683913076489465, 0.009711690148675985),
```

The description of the estimator was carried over from the Cohesion paper but somewhere between reviewer feedback the settings were not updated.

## The estimator as run is the better one

The figures show which settings the code used. They cannot show which settings are better,
because they were made with one of them. So we also scored both versions, with the `incoherence`
package, on tests that each have a right answer of their own.

| Test | Right answer | No square root, as run | Square root, as printed |
| :-- | --: | --: | --: |
| Rank agreement with Shannon entropy in 5 bins, 500 random shapes | 1 | 0.950 | 0.892 |
| Mean gap to that Shannon entropy | 0 | 0.066 | 0.146 |
| Widest gap between 1D and 2D, the shape in Cohesion Figure 3 | 0 | 0.033 | 0.082 |
| Widest gap between 1D and 2D, the shape in Cohesion Figure 4 | 0 | 0.013 | 0.057 |

- **The estimator as run sits about half as far from Shannon entropy.** Agreeing with Shannon
  entropy and giving the same answer in any number of dimensions are what the papers claim for
  the estimator.
- **Repairing the square root does not close the gap.** Its kernel reaches further, so the grid
  cuts its tail off. With `k = 20` or a wider grid it reaches 0.933 and 0.122 on the first two
  rows, and does worse on the last two.
- **The square root reads too much into stray observations.** It lifts low densities, so 1% of a
  trial straying moves its entropy about four times as far as it moves Shannon entropy.

## What is and is not affected

- **Not affected: any figure, value or conclusion.** The paper reports what the code produced.
- **Not affected: the discrete results.** They use Shannon entropy and never touch `k`.
- **Affected: anyone who builds the estimator from the text.** They get a different measure. For
  example, with every printed setting, the square root included, Figure 7A ends at about 0.17
  where the paper shows 0.45. That uses the 501 reference points the printed `V0` and `V1` were
  worked out for. Taking `R = 100` at its word gives 0.16.
- **The Daisyworld figures (17 and 18) were not rerun**, because the saved datasets are no longer available. Their notebook calls the estimator with the same defaults as Figures 7 and 8.

## Corrected text

Section 2.2 should read, in substance:

> The density is calculated at R = 501 evenly spaced reference points over the range [−0.5, 1.5],
> after the observations have been normalised to [0, 1], with the constant set to k = 10.
>
> Equation 3 takes the variance of the densities themselves, Var̂(G), not of their square roots:
> H = 1 − (Var̂(G) − V0) / (V1 − V0). The square root is only used in the last step of cohesion,
> as described in the Cohesion paper.
>
> For these settings, V0 = 0.007006952515415692 is the variance of the densities for a maximum
> entropy distribution (observations spread uniformly over [0, 1]), and
> V1 = 0.040028626993849875 is the variance for a minimum entropy distribution (all observations
> at the same value).

Equation 2 is correct as printed.

## Two smaller wording points

These are not part of the mistake above. They are places which could do with clarification.

- **Section 3.8** says that with one distribution `[1/2, 1/2]` among `N` distributions `[1, 0]`,
  incoherence falls "from a maximum value of I = 0.654". The value is 0.654 at `N = 1`, rises to
  0.710 at `N = 3`, and only then falls. It drops back below 0.654 at about `N = 10`. The claim
  that incoherence tends to 0 as `N` grows still holds.
- **Figure 5** describes incoherence as invariant to the number of states. It is steady to within
  0.01, at 0.654 for 2 states, 0.644 for 10 and 0.648 for 100.

## The Cohesion paper needs no correction

> Davey, T. (2023). *Entropy*, 25(12), 1605. https://doi.org/10.3390/e25121605

Its estimator, Equations 3 and 4, is printed with `k = 100` and the square root. That is right,
because section 4 brings it in for the last step of cohesion, the entropy of the divergences.
Two points could do with clarification.

### The settings behind Figures 3 and 4

- **The figures did not use the printed settings.** Figures 3 and 4 call their dashed line "the
  density variance method described above". It was made with `k = 10` and no square root, the
  defaults in `ensemblepy/densityvar.py`, by `notebooks/continuous_entropy/Dimensional invariance.ipynb`.
- **The entropy of each sample used the same defaults.** Section 4 leaves the estimator behind
  `H_a` in Equation 2 open, "defined on a system by system bases". For real-valued data, every
  figure used `k = 10` and no square root.

_The wording quoted from the papers was read from their PubMed Central copies._
