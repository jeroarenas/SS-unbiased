# Known input covariance and least squares: closed-form analysis

This repository contains the code that reproduces the experiments of the
paper

> **When Does Known Input Covariance Help Least Squares? A Closed-Form
> Analysis**
> J. Arenas-García, L. K. Hansen, and J. Larsen
> *IEEE Signal Processing Letters*, submitted.

The paper analyses the linear unbiased estimation problem under a Gaussian
linear model when, in addition to a finite set of labelled samples, the
covariance matrix of the input regressors is known (equivalently, an
unlimited amount of unlabelled data is available). Three estimators are
considered:

* the ordinary least-squares (LS) estimator,
* a semi-supervised (SS) estimator that exploits the known input
  covariance,
* the affine combination of both.

Closed-form, finite-sample expressions are derived for the excess
mean-square error (EMSE) of each estimator and for the optimal mixing
parameter. The combined estimator is shown to systematically improve on
both individual estimators, with the largest gains at low SNR and high
relative dimensionality. A leave-one-out (LOO) procedure is proposed that
attains the theoretical optimum without requiring any oracle knowledge
of the noise variance or the unknown parameter vector.

## Requirements

* Python ≥ 3.9
* NumPy
* Matplotlib

A minimal environment can be installed with

```bash
pip install numpy matplotlib
```

## Files

| File | Purpose |
|------|---------|
| `make_figures.py` | Reproduces the two figures of the experimental section. |

## Quick start

The figures of the paper are produced by a single entry point,
`make_figures`, exposed by `make_figures.py`. Running the script with no
arguments,

```bash
python make_figures.py
```

generates the figures for two representative scenarios (white regressors
with a generic unknown vector, and AR(1)-coloured regressors with the
unknown vector aligned to the low-eigenvalue subspace of the input
covariance).

## Reproducing a single scenario

`make_figures` is fully parametrised. The relevant inputs are

| Parameter | Description |
|-----------|-------------|
| `M` | Dimension of the unknown vector. |
| `N_max` | Largest training-set size shown in the figures. |
| `n_runs` | Number of Monte Carlo training sets. |
| `w_norm` | Euclidean norm of the unknown vector. |
| `noise_power` | Observation-noise variance σ²<sub>ε</sub>. |
| `colouring` | `"white"` or `("ar1", a)`. In the latter case the input regressors are the output of the IIR filter H(z) = √(1−a²)/(1−a z⁻¹) driven by white noise, giving a Toeplitz covariance Σᵢⱼ = a<sup>\|i−j\|</sup> with trace(Σ)/M = 1. |
| `alignment` | `"random"`, `"low"` or `"high"`: alignment of the unknown vector with the eigenvectors of Σ. |
| `k` | Number of eigenvectors used when `alignment` is `"low"` or `"high"`. |
| `marker_step` | Spacing in N between Monte Carlo markers. |
| `seed` | Random-number generator seed. |
| `show_title` | If `True`, an informative title is added to each figure. |
| `fig1_path`, `fig2_path` | Output paths for the EMSE figure and the mixing-parameter figure. |

Example:

```python
from make_figures import make_figures

make_figures(
    M=96, N_max=360, n_runs=400,
    w_norm=1.0, noise_power=0.1,
    colouring=("ar1", 0.8),
    alignment="low", k=8,
    marker_step=10,
    fig1_path="emse.pdf",
    fig2_path="lambda.pdf",
)
```

## What the figures show

* **EMSE figure.** Closed-form curves (solid) for the EMSE of the LS
  estimator, the SS estimator, the cross-EMSE between them, and the
  optimal combination. Monte Carlo estimates are overlaid as markers.
  For the LS, SS and cross-EMSE curves, the markers validate their own
  curve; for the combination, the markers correspond to the realised
  estimator whose mixing parameter is selected by LOO (compared against
  the theoretical optimum).
* **Mixing-parameter figure.** Optimal mixing parameter λ\* (solid line),
  together with the mean of the LOO estimate (markers) and a shaded
  band of ±1 standard deviation across Monte Carlo runs.

## Citation

```bibtex
@article{arenas2026known,
  title   = {When Does Known Input Covariance Help Least Squares?
             A Closed-Form Analysis},
  author  = {Arenas-Garc\'\i{}a, Jer\'onimo and Hansen, Lars Kai
             and Larsen, Jan},
  journal = {IEEE Signal Processing Letters},
  year    = {2026},
  note    = {Submitted}
}
```

## License

MIT.
