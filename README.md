# PoD: Microbial Growth

This repository is for a PoD project on microbial growth. The repository currently contains no source code, data, or analysis files, so the specific model and results are not yet documented.
This project models microbial cell growth and division using single-cell lineage measurements collected under different carbon-source conditions. It combines a stochastic growth-and-division model with Bayesian parameter estimation, and includes exploratory survival/hazard analyses and saved MCMC diagnostic plots.

## Project status
## Model

Project details will be added as the implementation takes shape. In particular, this README needs the following information:
Between divisions, cell mass grows exponentially. For a cell born with mass \(m_0\) and growth rate \(\alpha\), the increase in mass is represented as

- the research question and microbial growth model;
- data sources, units, and any preprocessing;
- software and package requirements;
- instructions for running the analysis or simulations;
- a description of generated results and how to interpret them.
\[
P(t) = m_0\left(e^{\alpha t}-1\right),
\qquad
m(t) = m_0 e^{\alpha t}.
\]

The probability that a cell has not divided by time \(t\) is modeled with a generalized-logistic survival function:

\[
S(t) = \left[1 + \left(\frac{P(t)}{\tau}\right)^k\right]^{-\alpha_S}.
\]

The corresponding division-time density is calculated from the hazard and survival functions. Growth rates are modeled with a Gamma distribution, and division partition ratios with a Beta distribution. At division, the partition ratio determines the daughter cell's starting mass. The fitting code estimates the model and distribution parameters from observed generation times, growth rates, division ratios, and birth masses.

The real-data fitting notebook uses `emcee` to sample the parameter posterior. It produces parameter-chain plots and corner plots for inspecting sampler behavior and posterior distributions.

## Repository contents

No project files are currently present.
### Code and notebooks

## Setup and usage
| Path | Purpose |
|---|---|
| `func.py` | Growth, survival, hazard, likelihood, prior, posterior, and synthetic-lineage functions used by the fitting notebook. |
| `Emcee_fit_real_data_v8.ipynb` | Main real-data workflow: load and filter a selected substrate, initialize parameter estimates, run ensemble MCMC, and inspect/save results. |
| `survival_function_fit.ipynb` | Exploratory analysis of empirical survival and hazard curves, generalized-logistic fits, and synthetic-data workflows. Some cells retain earlier module/path references; see the run notes below. |
| `.ipynb_checkpoints/` | Jupyter checkpoint copies and intermediate notebook artifacts. |

Setup and run instructions are not available until the implementation and its dependencies are added.
### Data

## License
The `data/` directory contains substrate-specific CSV files and combined tables:

No license is currently provided. All rights are reserved by default; contact the repository owner before reusing or redistributing project materials.
- `UMIK_Ace.csv`, `UMIK_DMa.csv`, `UMIK_Glu.csv`, `UMIK_Gly.csv`, `UMIK_Lac.csv`, `UMIK_Pyr.csv`, `UMIK_Rha.csv`, `UMIK_Suc.csv`, and `UMIK_Xyl.csv` contain measurements for individual substrate conditions.
- `UMIK_allSources.csv` combines the substrate conditions and includes a `source` column.
- `df_UMIK_allSources.csv` is an additional combined data table.

The tables include measurements such as `generationtime`, `growth_rate`, `division_ratio`, `length_birth`, `length_final`, `lineage_ID`, `generation`, and, in the combined table, `source`.

### Existing figures

- `chain plots/` contains saved MCMC trace plots by substrate and parameter.
- `corner plots/` contains saved posterior corner plots by substrate.

## Setup

The primary fitting notebook uses Python 3, Jupyter, NumPy, pandas, SciPy, Matplotlib, `emcee`, and `corner`.

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install numpy pandas scipy matplotlib emcee corner jupyter
jupyter lab
```

Open `emcee_real_data_fit.ipynb` from the repository directory and run the cells in order. The notebook currently sets `path_to_directory = 'data2/'`, but the checked-in datasets are under `data/`. Change that notebook setting to `data/` before running its data-loading cell. The notebook's MCMC section uses multiple processes and can take a long time; its current settings use 20 walkers and 30,000 production steps after burn-in.

The notebook selects one substrate through the `sugar` variable (currently `Ace`). Set it to one of the substrate labels listed above to fit another condition.

## Notes on interpretation and reproducibility

- MCMC estimates depend on the selected substrate, filtering, starting values, priors, and sampler settings. Inspect trace plots and convergence diagnostics before interpreting posterior summaries.
- The notebook seeds NumPy's global random generator for some operations, but parallel sampling and changes to execution order or package versions can still affect results.
- `survival_function_fit.ipynb` is an exploratory notebook. It imports `functions` and `CmdStanModel`, while the current top-level code provides `func.py` and no Stan model file. It may require adapting those references before all cells run in a clean environment.
- No `LICENSE` file is included. Contact the repository owner before redistributing the data or reusing project materials.
