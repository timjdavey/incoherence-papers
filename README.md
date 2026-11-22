<div align="center">

# Incoherence papers

**Measuring complexity and self-organisation**

The code, simulations and notebooks for the papers.

[![incoherence package](https://img.shields.io/badge/package-incoherence-2EA44F?style=flat-square)](https://github.com/timjdavey/incoherence) [![Incoherence paper](https://img.shields.io/badge/Entropy_2024-Incoherence-8A2BE2?style=flat-square)](https://www.mdpi.com/1099-4300/26/8/683) [![Cohesion paper](https://img.shields.io/badge/Entropy_2023-Cohesion-008080?style=flat-square)](https://www.mdpi.com/1099-4300/25/12/1605) [![Primers](https://img.shields.io/badge/primers-timjdavey.com-FBFAF7?style=flat-square&labelColor=333)](https://timjdavey.com/incoherence/) [![Python](https://img.shields.io/badge/python-3-3776AB?style=flat-square&logo=python&logoColor=white)](#running-the-notebooks) [![MIT licence](https://img.shields.io/badge/licence-MIT-lightgrey?style=flat-square)](LICENSE)

</div>

<br>

<table>
  <tr>
    <td width="33%"><img src="notebooks/cohesion/low-inc.png" alt="Ten runs of a system whose distributions all lie on top of each other"></td>
    <td width="33%"><img src="notebooks/cohesion/low-coh.png" alt="Runs of a system whose distributions each sit in a different place"></td>
    <td width="33%"><img src="notebooks/cohesion/high-coh.png" alt="Runs of a system that split into two tight clusters"></td>
  </tr>
  <tr>
    <td align="center"><b>Low incoherence</b><br><sub>Every run looks the same, so the average is a fair summary of any one of them.</sub></td>
    <td align="center"><b>High incoherence, low cohesion</b><br><sub>Every run does its own thing, and the average describes none of them.</sub></td>
    <td align="center"><b>High incoherence, high cohesion</b><br><sub>The runs disagree, but they organise themselves into a few clear camps.</sub></td>
  </tr>
</table>

## Start here

The primers are the best way to understand the measures. They explain the big picture with no equations or jargon.

<table>
  <tr>
    <td width="50%" valign="top">
      <h3><a href="https://timjdavey.com/incoherence/">Complexity, and how to measure it</a></h3>
      <p>What makes a system complex, why entropy alone can't capture it, and how comparing an
      ensemble of runs leads to Incoherence. Includes practical, real and business examples.</p>
      <p>📖 <a href="https://timjdavey.com/incoherence/">Read the primer</a> · 📄 <a href="https://www.mdpi.com/1099-4300/26/8/683">Read the paper</a></p>
    </td>
    <td width="50%" valign="top">
      <h3><a href="https://timjdavey.com/cohesion/">Self-organisation, and how to measure it</a></h3>
      <p>What it means for a system to organise itself, how that changes the decisions you make
      about it (with climate change as the example), and how Cohesion is built.</p>
      <p>📖 <a href="https://timjdavey.com/cohesion/">Read the primer</a> · 📄 <a href="https://www.mdpi.com/1099-4300/25/12/1605">Read the paper</a></p>
    </td>
  </tr>
</table>

## What is this?

This repository is the experimental code behind both papers. It holds `ensemblepy`, the original
library that produced the published numbers, the simulations used to test the measures and the
notebooks that made every figure. It is kept so the papers can be reproduced. It is not the place to start if you want to measure your own data.

> [!IMPORTANT]
> **Want to calculate Incoherence or Cohesion? Use the
> [`incoherence`](https://github.com/timjdavey/incoherence) package.**
> It is a rebuild of the code here into something small, tested and fast. This repository used to
> be called `incoherence`. It was renamed to `incoherence-papers` when the package took that name.

## Repo structure

```text
ensemblepy/              the original library behind the papers
├── continuous.py        Continuous: ensembles of real-valued observations
├── discrete.py          Discrete: binned or categorical observations, plus chi² comparisons
├── collection.py        Collection: start from histograms rather than raw observations
├── correlation.py       Correlation: incoherence as a correlation coefficient for x, y data
├── stats.py             the Incoherence and Cohesion calculations themselves
├── entropy.py           Shannon, pooled and per-ensemble entropies
├── densityvar.py        density variance, the continuous analogue of entropy
├── divergences.py       Jensen–Shannon and the other divergences used along the way
├── bins.py, plots.py    binning and plotting helpers
└── tests/

simulations/             the systems used to test the measures (mostly built on Mesa)
├── daisy_world/         Daisyworld, the classic self-regulating planet
├── wealth/              the Boltzmann wealth model of agents trading money
├── automata/            one-dimensional cellular automata, by Wolfram rule number
├── graphs/              Erdős–Rényi random graphs and their tipping point
├── fab/                 a semiconductor factory line of parcels passing through noisy machines
├── snowflake/           generated snowflakes with tunable symmetry
└── ideal_gas/           particles bouncing around a box

notebooks/               the analysis and figures behind the papers
├── incoherence/         the Incoherence paper, numbered by draft section
├── cohesion/            the Cohesion paper
├── continuous_entropy/  density variance and its dimensional invariance
├── chi_squared/         comparisons with chi², ANOVA and Kruskal–Wallis
└── correlation_coeff/   Incoherence as a correlation measure
```

Some of `ensemblepy` was not carried over to the package. Plotting, the comparisons with other
statistics and entropy in more than one dimension live only here.
[Using `ensemblepy`](docs/ensemblepy.md) shows how to call it.

Each part of the Incoherence paper's results has a notebook behind it. The notebooks in
[`notebooks/incoherence/`](notebooks/incoherence/) carry the numbering from an earlier draft, so
the table below maps them onto the sections of the published paper.

| Paper | What it tests | Systems | Notebooks |
| :-- | :-- | :-- | :-- |
| **5.1** Baseline | How incoherence compares with chi², ANOVA and friends | 📊 synthetic data, 🌼 Daisyworld | `chi_squared/`, 5 |
| **5.2** Disorder and order | Whether complexity is distinct from plain randomness | ◼️ cellular automata, 🕸️ ER graphs | 4.1 |
| **5.3** Criticality | Whether it peaks at tipping points and is sensitive to initial conditions | 🕸️ ER graphs, ◼️ cellular automata, 🌼 Daisyworld | 4.3, 4.4 |
| **5.4** Perturbation | How a self-regulating system responds to shocks | 🌼 Daisyworld | 4.5 |
| **5.5** Diversity of diversity | What happens as the system gains variety | 🌼 Daisyworld | 4.6 |
| **6** Decision making | A digital twin of a semiconductor factory | 🏭 factory line | `FAB` |

The wealth model notebooks (4.1 and 4.2) and the movie reviews notebook are earlier explorations
that didn't make it into the final paper. The Cohesion notebooks in
[`notebooks/cohesion/`](notebooks/cohesion/) cover the Daisyworld example, ergodicity, language
detection and the illustrative figures from the paper.

[Reproducing the figures](docs/reproducing.md) maps every figure in both papers to the notebook
and file that made it. [Work that was not published](docs/unpublished.md) describes the rest.

## Corrections

There is one known mistake in the text of the Incoherence paper and none in the Cohesion paper.
Section 2.2 of the Incoherence paper gives the settings of the entropy estimator as `k = 100`,
but the figures were made with `k = 10`. No figure, value or conclusion changes.
[Corrections to the papers](docs/corrections.md) has the details and the corrected text.

## Running the notebooks

This section is for reproducing the papers. The code is plain Python with no build step. Install
the dependencies and open the notebooks from inside their folder, since each one adds the repo
root to its path through `nbsetup.py`.

```sh
pip install -r requirements.txt "mesa<3"
jupyter notebook notebooks/
```

The code is old and most notebooks no longer run from top to bottom. The outputs saved inside
them are the published runs. [Reproducing the figures](docs/reproducing.md) lists the versions
that work, the datasets you have to regenerate and the tests that still pass.

## Citing

If this work is useful to you, please cite the papers.

<details>
<summary><b>BibTeX</b></summary>

```bibtex
@article{davey2024incoherence,
  title   = {Incoherence: A Generalized Measure of Complexity to Quantify Ensemble Divergence in Multi-Trial Experiments and Simulations},
  author  = {Davey, Timothy},
  journal = {Entropy},
  volume  = {26},
  number  = {8},
  pages   = {683},
  year    = {2024},
  doi     = {10.3390/e26080683}
}

@article{davey2023cohesion,
  title   = {Cohesion: A Measure of Organisation and Epistemic Uncertainty of Incoherent Ensembles},
  author  = {Davey, Timothy},
  journal = {Entropy},
  volume  = {25},
  number  = {12},
  pages   = {1605},
  year    = {2023},
  doi     = {10.3390/e25121605}
}
```

</details>

## Thanks

Particular thanks go to Carlos Gershenson, who proposed the 2012 measure of complexity that this
work builds on, and who generously edited the special issue of *Entropy* in which the Incoherence
paper was published.

## Licence

[MIT](LICENSE) © 2021 Tim Davey
