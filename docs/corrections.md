# Corrections to the papers

This page records every known mistake in the text of the two papers.

There is one in the Incoherence paper and none in the Cohesion paper.

Section 2.2 describes the continuous entropy estimator.
- It gives the settings used for cohesion `k=100`
- However, the incoherence paper used `k=10`

This is a setting which can be adjusted on the entropy estimator and is dependent on the spread of your observations. It changes the value of the entropy calculated slightly, but does not directly alter the incoherence value (only indirectly).

None of the conclusions change.


## What the paper says

Section 2.2, "Continuous Entropy", around equations 2 and 3:

- The density is worked out at `R = 100` reference points with the constant `k = 100`.
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
| Power applied to each density | None stated | None |
| `V0`, maximum entropy | 0.0047683913076489465 | 0.007006952515415692 |
| `V1`, minimum entropy | 0.009711690148675985 | 0.040028626993849875 |

The code asks for 500 steps and a helper function rounds that up to 501 points.

## Where the wrong values came from

They are the settings for the last step of cohesion, as described in the Cohesion paper
(*Entropy* 25(12), 1605). That step uses `k = 100` and takes the square root of each density,
because the pairwise divergences it measures are tightly bunched and need a sharper kernel.

The two constants printed in the Incoherence paper are exactly the ones the code stores for that
case. In `CACHED_VS` in `ensemblepy/densityvar.py`:

```python
# standard: power = 1
(10, (-0.5, 1.5), 500, 1, 1): (0.007006952515415692, 0.040028626993849875),
# for cohesion: power = 0.5, but need higher drop-off so k=100
(100, (-0.5, 1.5), 500, 0.5, 1): (0.0047683913076489465, 0.009711690148675985),
```

The description of the estimator was carried over from the Cohesion paper but somewhere between reviews the settings were not updated.

## What is and is not affected

- **Not affected: any figure, value or conclusion.** The paper reports what the code produced.
- **Not affected: the discrete results.** They use Shannon entropy and never touch `k`.
- **Affected: anyone who builds the estimator from the text.** They get a different measure. For
  example, they would get 0.17 or 0.23 where Figure 7A shows 0.45.
- **The Daisyworld figures (17 and 18) were not rerun**, because the saved datasets are no longer available. Their notebook calls the estimator with the same defaults as Figures 7 and 8.

## Corrected text

Section 2.2 should read, in substance:

> The density is calculated at R = 501 evenly spaced reference points over the range [−0.5, 1.5],
> after the observations have been normalised to [0, 1], with the constant set to k = 10.
>
> For these settings, V0 = 0.007006952515415692 is the variance of the densities for a maximum
> entropy distribution (observations spread uniformly over [0, 1]), and
> V1 = 0.040028626993849875 is the variance for a minimum entropy distribution (all observations
> at the same value).

Equations 2 and 3 themselves are correct as printed.

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

However a clarification on equation 2 of the Cohesion paper.

Section 4 can be read more than one way when two samples hold different numbers of observations. This section says which reading the figures used.

Equation 2 pools two samples, `a` and `b`, and weights them by `w`. Section 4 describes the pooled
sample as the observations of `a` and `b` combined, and also as the point-wise mean. Those are the
same when the samples are the same size. When they differ, combining the observations gives the
larger sample more say. The section offers `w = 1/2` or `w = n/N` and leaves the choice open.

**What the figures used.** The observations were combined, and `w = 1/2`.

**Where it shows.** Only in the bottom two panels of Figures 5 and 6. Each burst there is a
sample, and section 5.1 says the count of observations in each pattern was drawn at random. Those
bursts hold between 3 and 17 observations.

| Figures 5 and 6 | Published | As run: combined, `w = 1/2` | Point-wise mean, `w = 1/2` | Combined, `w = n/N` |
| :-- | :-- | :-- | :-- | :-- |
| Five patterns | 0.5 | 0.524 | 0.394 | 0.460 |
| No patterns | 0.1 | 0.141 | 0.133 | 0.157 |

The top two panels are the same on every reading, because both of their patterns hold 10
observations. The four panels stay in the same order on every reading, so the conclusion holds.

Every other figure in both papers that uses the continuous estimator has samples of equal size, so
the reading makes no difference to them. Figures 7 and 8 of the Incoherence paper use the same
number of observations in every trial of a run. The Daisyworld figures in both papers use one
temperature per grid cell. The notebook behind Figures 5 and 6 is
`notebooks/cohesion/Language detection.ipynb`, seeded with 44, 52 and 32.

_The wording quoted from the papers was read from their PubMed Central copies._
