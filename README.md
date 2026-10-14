# High-Resolution Compact Finite-Difference Schemes

A research-notebook collection and reproducible Python validation exploring **compact numerical differentiation**, **modified-wavenumber resolution**, and **tridiagonal, pentadiagonal and heptadiagonal implicit schemes**.

> **Status:** Reconstructed and organised from existing internship notebooks. The notebooks document exploratory research, not a finished publication or proof of a new numerical method. The Python validation was independently rerun and checked against its saved notebook outputs.

## Research question

How do compact finite-difference coefficients and coupling width affect the approximation to a derivative, particularly for high-wavenumber components where resolution can be challenging?

Unlike explicit finite differences, compact schemes couple unknown derivatives at neighbouring grid points. In the heptadiagonal form studied here, for interior point $i$ and grid spacing $h$:

$$
\begin{aligned}
d_i &+ \alpha(d_{i-1}+d_{i+1}) + \beta(d_{i-2}+d_{i+2}) \\
 &+ \gamma(d_{i-3}+d_{i+3}) \\
&= a\frac{f_{i+1}-f_{i-1}}{2h} + b\frac{f_{i+2}-f_{i-2}}{4h} + c\frac{f_{i+3}-f_{i-3}}{6h}.
\end{aligned}
$$

Here $d_i$ approximates $f'(x_i)$. Setting $\gamma=0$ removes third-neighbour derivative coupling (pentadiagonal left-hand side), while setting both $\beta=\gamma=0$ gives a tridiagonal left-hand side.

## What the source notebooks investigate

1. **Parameterised coefficient optimisation:** Mathematica notebooks set up consistency equations and optimise coefficients over a range of dimensionless wavenumbers, controlled by a parameter $r$.
2. **Modified-wavenumber behaviour:** The notes compare $k$ with a rational expression $k_{\mathrm{mod}}(k)$ formed from the stencil coefficients, to study how accurately different wavelengths are represented.
3. **Matrix-based numerical differentiation:** A Mathematica tridiagonal experiment solves a finite-dimensional derivative system and compares it with a known derivative.
4. **Heptadiagonal Python validation:** A separate Python notebook uses a dense linear solve and compares the result with an analytical derivative at three grid resolutions.

For the heptadiagonal stencil, the modified wavenumber in the notebook is

$$
 k_{\mathrm{mod}}(k) =
 \frac{a\sin k + (b/2)\sin(2k) + (c/3)\sin(3k)}
 {1+2\alpha\cos k+2\beta\cos(2k)+2\gamma\cos(3k)}.
$$

The Mathematica notebooks contain exploratory symbolic calculations and several parameter variants; their original outputs have **not** been recomputed in Mathematica in this repository.

## Reproduced numerical results

The Python test evaluates the derivative of

$$f(x)=e^{-20x^2}\sin(50x), \qquad 0\le x\le1.$$

The following values were **reproduced from the provided Python implementation**, using the same rounded coefficients and analytically supplied ghost derivatives:

| Grid intervals ($n$) | Maximum absolute error | RMS error |
| ---: | ---: | ---: |
| 20 | 1.684771 | 0.986659 |
| 25 | 0.016251 | 0.005552 |
| 50 | 0.003128 | 0.000729 |

![Derivative approximation and error by resolution](results/heptadiagonal_validation.png)

This dataset shows lower reported errors at the three sampled resolutions, but **does not establish a formal convergence order**. The coarse-grid error is much larger, consistent with the difficulty of resolving the oscillatory test function at low resolution.

### Reconstructed modified-wavenumber figure

![Reconstructed modified-wavenumber comparison](results/modified_wavenumber_reconstructed.png)

The above curves were **replotted in Python using coefficients recorded in the Mathematica notebook outputs**, not re-optimised. The panels represent a pentadiagonal candidate at $r=0.865$ and a heptadiagonal candidate at $r=0.856$. They are visualisations of specific recorded coefficient sets, **not a controlled head-to-head performance comparison or proof of superiority**.

## Repository layout

```text
compact-finite-difference-schemes/
├── README.md
├── METHODS_AND_LIMITATIONS.md
├── requirements.txt
├── src/
│   ├── heptadiagonal.py
│   └── reproduce_results.py
├── notebooks/
│   ├── mathematica/
│   │   ├── 01_tridiagonal_pentadiagonal_r_study.nb
│   │   ├── 02_pentadiagonal_modified_wavenumber.nb
│   │   ├── 03_heptadiagonal_optimisation.nb
│   │   ├── 04_heptadiagonal_modified_wavenumber.nb
│   │   └── 05_tridiagonal_numerical_test.nb
│   └── python/
│       └── heptadiagonal_validation.ipynb
├── results/
│   ├── heptadiagonal_errors.csv
│   ├── heptadiagonal_validation.png
│   └── modified_wavenumber_reconstructed.png
└── archive/
    └── original / alternative exploratory notebooks
```

The `archive/` folder preserves the original Python experiment, an initial Mathematica study, an alternative heptadiagonal test, and a smaller exploratory Mathematica file. Renamed files in `notebooks/mathematica/` are otherwise **unchanged source notebooks**.

## Running the Python validation

From the root of the cloned repository:

```bash
python -m pip install -r requirements.txt
python -m src.reproduce_results
```

The script creates the PNG figure and CSV file from the three recorded grid resolutions. Alternatively, launch Jupyter Notebook and open `notebooks/python/heptadiagonal_validation.ipynb` from the repository root. Wolfram Mathematica is required to edit or recompute the `.nb` source notebooks.

## Tools

- **Wolfram Mathematica / Wolfram Language** — symbolic consistency relations, coefficient fitting, plots
- **Python, NumPy** — matrix assembly, dense linear solve, analytical comparison
- **Matplotlib / Jupyter** — numerical visualisation and experiment documentation

## Interpretation and limitations

- The heptadiagonal Python experiment uses **known analytical derivatives beyond the grid boundary** to account for ghost points. A deployable scheme would need independent boundary conditions or a consistent closure.
- The Python coefficient values were already hard-coded in the original notebook; the supplied files do not unambiguously document how that specific set was obtained from a particular Mathematica optimisation run.
- Multiple Mathematica notebooks are **experimental variants**, and some contain incomplete or unevaluated calculations. No claim of a globally optimal or novel method is made.
- Direct comparisons between tri-, penta- and heptadiagonal accuracy under *identical* boundary conditions and test setups remain future work.
- No standalone final internship report was available. See [METHODS_AND_LIMITATIONS.md](METHODS_AND_LIMITATIONS.md) for file-level provenance and caveats.

## Research context

These notebooks were developed during a computational/numerical-physics internship investigating high-resolution compact finite-difference schemes. They are preserved here as an annotated record of exploratory scientific programming and numerical analysis.
