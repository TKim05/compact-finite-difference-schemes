# Research reconstruction: methodology, provenance and limitations

This file distinguishes **observations from uploaded notebooks**, **verified reproduced results**, and **interpretations requiring further validation**.

## Source file inventory

| Portfolio file | Original filename | Main content observed |
|---|---|---|
| `notebooks/mathematica/01_tridiagonal_pentadiagonal_r_study.nb` | `internshup(modified r).nb` | Parameter-dependent symbolic coefficient solving for tridiagonal/pentadiagonal variants, modified-wavenumber plots, sin(x) residual tests and exploratory matrix construction |
| `notebooks/mathematica/02_pentadiagonal_modified_wavenumber.nb` | `penta - test.nb` | Coefficient solution and modified-wavenumber/relative-error plots for a pentadiagonal experiment, with parameter 0.865 |
| `notebooks/mathematica/03_heptadiagonal_optimisation.nb` | `Internship hepta.nb` | Sixth-order consistency equations, symbolic/numerical coefficient solution and a parameter/r-dependent metric and plot |
| `notebooks/mathematica/04_heptadiagonal_modified_wavenumber.nb` | `Internship hepta - test.nb` | Heptadiagonal coefficient solver and comparison of exact and numerical modified wavenumber, parameter 0.856 |
| `notebooks/mathematica/05_tridiagonal_numerical_test.nb` | `Internship 2.nb` | Matrix-based tridiagonal differentiation of a sine test function, plots against analytical derivative |
| `archive/initial_tridiagonal_pentadiagonal_study.nb` | `internshup.nb` | Earlier tri-/penta- scheme coefficient investigations and plots |
| `archive/alternative_heptadiagonal_test.nb` | `Internship hepta - test - Copy.nb` | Alternate heptadiagonal test and figures; not assumed to be identical to the primary heptadiagonal test |
| `archive/untitled_experiment.nb` | `Untitled-1.nb` | Small exploratory notebook with insufficient context to classify as a final algorithm |
| `archive/original_heptadiagonal_python_test.ipynb` | `internship.ipynb` | Hard-coded coefficients, heptadiagonal matrix construction, ghost-derivative correction, and max/RMS errors for n=20,25,50 |

**Excluded:** `Untitled.ipynb` contains no cells; the three `log*.ipynb` notebooks investigate gravitational integration with Euler and Velocity-Verlet and belong to a separate project. `HPC_coursework1.ipynb` concerns matrix multiplication and is unrelated to this research archive.

## Compact scheme and optimisation

The Mathematica code uses a family of implicit compact finite differences with neighbours at offsets 1, 2 and 3 and corresponding right-hand-side derivative stencils. Some notebooks derive consistency equations (labelled `second`, `fourth`, `sixth`, `eighth`, `tenth`) and use `Solve`/`NSolve` together with derivatives of an integrated error functional.

The optimisation ranges and weights are **not identical across all notebooks**: for example, one heptadiagonal notebook uses a weighted integral involving an exponential term, while another uses a power weight with n=0. These should not be silently treated as the same optimisation objective.

Several Mathematica notebooks include exploratory variables, evaluated outputs and/or apparent revisions. Their *latest saved outputs* do not establish that every cell will run cleanly from a fresh Mathematica kernel. These files have been preserved unchanged, and no new Wolfram execution is claimed.

## Python validation: what was verified

The original `internship.ipynb` defines a test function `exp(-20*x^2) * sin(50*x)` and its exact derivative. It builds a heptadiagonal matrix with main diagonal 1 and symmetric off-diagonals `alpha`, `beta`, `gamma`; the RHS is assembled from three centred finite-difference stencils.

The notebook supplies **exact derivative values at outside-grid points** to remove ghost derivative terms from the RHS. The system is solved with NumPy's dense `linalg.solve`. It prints maximum absolute and RMS error for `n = 20, 25, 50`.

Original hard-coded coefficients:

| a | b | c | alpha | beta | gamma |
|---:|---:|---:|---:|---:|---:|
| 1.16098 | 1.24087 | 0.199073 | 0.640794 | 0.151692 | 0.00797665 |

The coefficients approximately satisfy the low-order consistency equations in the heptadiagonal Mathematica study, within numerical rounding. Their exact optimisation source **could not be confidently traced** from the supplied files.

A cleaned Python implementation in `src/heptadiagonal.py` was independently run, and its errors were compared with saved outputs from the original notebook at all three resolutions. Agreement was verified at numerical precision. The test only covers the specific input function and supplied boundary values.

## Reconstructed coefficient plot

The comparison in `results/modified_wavenumber_reconstructed.png` uses these coefficient sets copied from **recorded Mathematica output cells** (not new coefficient fits):

- Pentadiagonal (`penta - test.nb`, r = 0.865): `(1.323482375223871, 0.9443942429827366, 0.027596356379887714, 0.5664582855813375, 0.08127820171190747)`.
- Heptadiagonal (`Internship hepta - test.nb`, r = 0.856): `(1.1977701578575464, 1.1736185404078332, 0.1603031375268261, 0.6239181082499834, 0.1357545937720012, 0.006173215874118228)`.

The panel plots the recorded candidate dispersion relations and exact wavenumber. It is **not** an optimised cross-method comparison.

## What cannot currently be concluded

- No benchmark justifies claiming a new numerical scheme or an accuracy improvement over published methods.
- No complete controlled comparison with standard noncompact or tridiagonal alternatives has been verified.
- No asymptotic convergence order can be inferred reliably from only three grid resolutions and analytically supplied ghost values.
- No final research report, peer-reviewed results, or definitive selection of the 'final' Mathematica variant was supplied.
- The original `.nb` files have not been evaluated with Wolfram Mathematica in this environment.

## Potential follow-up experiments

1. Implement physically appropriate one-sided/compact boundary closures without exact derivatives.
2. Compare schemes at matched grids and boundary conditions.
3. Systematically study convergence and roundoff limits over many resolutions.
4. Evaluate modified-wavenumber error over identical bandwidths with fixed metrics.
5. Consolidate and rerun the Mathematica optimisation steps from a clean session.
