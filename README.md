# Eco-evolutionary dynamics of sexually selected infanticide facilitate species coexistence

Python analysis code accompanying the manuscript **“Eco-evolutionary dynamics of sexually selected infanticide facilitate species coexistence”**.

This study examines how sexually selected male infanticide affects population dynamics and coexistence between species competing for a shared resource. A stage-structured model links the evolution of infanticidal male strategies to density-dependent demographic costs. Infanticide can strengthen self-limitation in a competitively superior species, allowing a competitor to persist under conditions that would otherwise lead to competitive exclusion.

## Repository contents

| File | Contents |
| --- | --- |
| [main/analysis_full_ecoevo_1sp.ipynb](main/analysis_full_ecoevo_1sp.ipynb) | Full seven-dimensional one-species model: time series, equilibrium searches, local stability, rare-strategy invasion thresholds, and bifurcation diagrams. |
| [main/analysis_2sp_QSS.ipynb](main/analysis_2sp_QSS.ipynb) | Two-dimensional ecological reduction under a quasi-steady-state (QSS) approximation, with fixed male-strategy frequencies: nullclines, phase portraits, and the outcome diagram on the frequency plane. |
| [main/analysis_full_ecoevo_2sp.ipynb](main/analysis_full_ecoevo_2sp.ipynb) | Full 14-dimensional two-species eco-evolutionary model: equilibrium and stability analyses, comparison with a model without infanticide, and representative demographic and evolutionary trajectories. |
| [supplementary/nodens_2sp_QSS.ipynb](supplementary/nodens_2sp_QSS.ipynb) | QSS ecological model with density-independent reproductive initiation and infanticide: phase portraits and fixed-frequency outcome diagram (Fig. S3). |
| [supplementary/nodens_2sp.ipynb](supplementary/nodens_2sp.ipynb) | Full eco-evolutionary model with density-independent reproductive initiation and infanticide: equilibrium enumeration, classification of stable equilibrium combinations, and representative trajectories (Fig. S4). |
| [supplementary/alldens_2sp_QSS.ipynb](supplementary/alldens_2sp_QSS.ipynb) | QSS ecological model with density-dependent reproductive initiation and infanticide: phase portraits, coexistence, and boundary bistability (Fig. S5). |
| [supplementary/alldens_2sp.ipynb](supplementary/alldens_2sp.ipynb) | Full eco-evolutionary model with density-dependent reproductive initiation and infanticide: parameter scans, comparison with the no-infanticide reference, multistability, and representative trajectories (Fig. S6). |
| [main/outcome_plane_p1_p2.svg](main/outcome_plane_p1_p2.svg) | Saved outcome-plane figure from the main QSS analysis. |
| [requirements.txt](requirements.txt) | Direct Python dependencies pinned to the versions in the current local environment. |
| [LICENCE](LICENCE) | MIT license for the code. |

The seven notebooks cover the main model and both alternative formulations in Supplementary Section 3. Figures S1–S2 concern the main model and are covered by the representative-trajectory analysis in `main/analysis_full_ecoevo_2sp.ipynb`. Figure correspondences identify the relevant analyses; some panels require changing parameters or formatting as described below.

## Model overview

Each species has five female classes and two male classes. The state vector for species $i$ is

$$
\mathbf{x}_i = (F_{f,i}, C_{1,N,i}, C_{2,N,i}, C_{1,I,i}, C_{2,I,i}, M_{N,i}, M_{I,i})^\top.
$$

Here, $F_{f,i}$ denotes free females; $C_{1,N,i}$ and $C_{2,N,i}$ denote females in two successive care stages with broods sired by non-infanticidal males; $C_{1,I,i}$ and $C_{2,I,i}$ are the corresponding classes for infanticidal sires; and $M_{N,i}$ and $M_{I,i}$ are male densities. Total adult density is $N_i$, the sum of these seven classes, and the frequency of infanticidal males is

$$
p_i = \frac{M_{I,i}}{M_{N,i}+M_{I,i}}.
$$

In the main model:

- Free females initiate reproduction at a constant per-capita rate when males are present, with sire strategy determined by current male frequencies.
- Infanticide occurs through encounters with conspecific infanticidal males, at hazard $\gamma_i M_{I,i}$ per caring female. It removes the current brood and resets care to the first stage with a new brood sired by the infanticidal male.
- Successful reproduction requires completion of both care stages. Sons inherit their father's strategy, and equal numbers of adult daughters and sons are recruited.
- Infanticidal sires incur a fecundity cost, $b_{I,i}=(1-s_i)b_{N,i}$.
- Density-dependent mortality is $d_i(N_i+\rho_{ij}N_j)$. Species interact through resource competition; reproduction and infanticide occur within species.

The three QSS notebooks fix $p_1$ and $p_2$, assume an equal adult sex ratio, and solve for stationary **fractions** of females in each reproductive state. They reduce the system to population densities $(N_1,N_2)$ and isolate ecological effects from strategy evolution. These QSS reductions are distinct from the eight-dimensional no-infanticide reference models used in the main and all-density-dependent full-model notebooks.

### Alternative assumptions about density dependence

| Model | Reproductive-initiation rate per free female | Infanticide hazard per caring female |
| --- | --- | --- |
| Main model (`main/`) | $\alpha_i$ when males are present | $\gamma_i M_{I,i}$ |
| Density-independent model (`nodens_*`) | $\alpha_i$ when males are present | $\gamma_i p_i$ |
| Density-dependent model (`alldens_*`) | $\alpha_i M_i$, where $M_i=M_{N,i}+M_{I,i}$ | $\gamma_i M_{I,i}$ |

### Main parameters

| Symbol | Code key | Meaning |
| --- | --- | --- |
| $\alpha_i$ | `alpha`, `alpha1`, `alpha2` | Per-capita rate of reproductive initiation. |
| $\beta_i$ | `beta`, `beta1`, `beta2` | Rate of progression through each care stage. |
| $\gamma_i$ | `gamma`, `gamma1`, `gamma2` | Coefficient of density-dependent infanticidal encounters. |
| $d_i$ | `d`, `d1`, `d2` | Coefficient of density-dependent mortality. |
| $b_{N,i}$ | `b_N`, `b_N1`, `b_N2` | Adult daughters, and separately adult sons, recruited per completed non-infanticidal brood. |
| $s_i$ | `s`, `s1`, `s2` | Proportional fecundity cost of the infanticidal strategy. |
| $\rho_{ij}$ | `rho12`, `rho21` | Competitive effect of species $j$ on species $i$. |
| $p_i$ | `p1`, `p2` in the QSS notebook | Fixed infanticidal-male frequency in the ecological reduction; an evolving quantity in the full model. |

The supplementary QSS notebooks use `r_N1` and `r_N2` for the reproductive-output parameters denoted by `b_N1` and `b_N2` in the full-model notebooks. They calculate `r_I=(1-s)*r_N`. Also, `alpha` is a mass-action coefficient in the `alldens_*` models, and `gamma` is a density-independent hazard coefficient in the `nodens_*` models; numerical values should be interpreted within the corresponding formulation.

See the manuscript and supplement for the complete equations and analytical invasion criteria.

## Environment

The analysis environment is described below. Dependency versions are listed in the table and pinned in [requirements.txt](requirements.txt).

| Component | Version | Purpose |
| --- | --- | --- |
| Python | 3.13.7 | Python interpreter. |
| NumPy | 2.4.4 | Numerical arrays and linear algebra. |
| SciPy | 1.17.1 | ODE integration, root finding, and numerical utilities. |
| Matplotlib | 3.10.9 | Figures and phase diagrams. |
| tqdm | 4.67.1 | Progress indicators for parameter scans. |
| mpmath | 1.3.0 | High-precision refinement of no-infanticide equilibrium roots in `alldens_2sp.ipynb`. |
| JupyterLab | 4.4.7 | Notebook interface. |
| ipykernel | 6.30.1 | Python kernel for Jupyter and VS Code. |
| Jupyter Notebook | 7.4.5 | Alternative notebook interface. |

Package versions are recorded in [requirements.txt](requirements.txt).

## Running the analyses

Each notebook is self-contained and does not import code or results from the other notebooks. No external observational dataset is required: results are generated from the model equations, parameters, and initial conditions defined in the cells.

1. Open a notebook in `main/` or `supplementary/` using the intended Conda environment's kernel.
2. Check the `params` dictionary and the relevant scan and plotting settings against the figure you want to reproduce.
3. Restart the kernel and run cells from top to bottom. Later plotting cells depend on functions and results created earlier in the same notebook.
4. After changing parameters, rerun the setup and all dependent analysis cells. Merely changing a plotting cell does not recompute equilibria or scans.

### Correspondence with manuscript figures

| Figure | Notebook and analysis | Settings needed |
| --- | --- | --- |
| Fig. 1 | Conceptual life-history diagram. | This schematic is not generated by the notebooks. |
| Fig. 2A–B | One-species notebook, `One-parameter bifurcation diagrams along s`. | Scan $s\in[0,1]$ with `gamma=0.01`. |
| Fig. 2C–D | One-species notebook, `One-parameter bifurcation diagrams along gamma`. | Scan $\gamma\in[0,0.02]$ with `s=0.6`. |
| Fig. 3 | One-species notebook, `Dynamics`. | Run separately with `s=0.0`, `0.6`, and `0.8`. |
| Fig. 4A | QSS notebook, `Outcome diagram on the (p1,p2) plane`. | Use the Fig. 4 parameters below and scan both frequencies from 0 to 1. |
| Fig. 4B–E | QSS notebook, `Phase plane and nullclines`. | Run separately at `(p1,p2)=(0.2,0.2)`, `(0.2,0.8)`, `(0.8,0.2)`, and `(0.8,0.8)`. |
| Fig. 5 | Full two-species notebook, full-model scan, no-infanticide reference scan, and `Publication figures`. | Scan `s1=s2` and `rho12=rho21` from 0 to 1. |
| Figs. S1–S2 | Full two-species notebook, `Dynamics at representative parameter values in each region`. | The current cell selects a representative point in each outcome region automatically; use the caption values below to reproduce the specified panels. |
| Fig. S3 | `supplementary/nodens_2sp_QSS.ipynb`, initial phase portrait and frequency-plane scan. | Default model parameters match the caption; set each phase-portrait frequency pair separately. |
| Fig. S4 | `supplementary/nodens_2sp.ipynb`, full-model scan and stable-equilibrium classification plots. | Scan common cost and competition using the default demographic parameters. |
| Fig. S5 | `supplementary/alldens_2sp_QSS.ipynb`, initial phase portrait and frequency-plane scan. | Default model parameters match the caption; set each phase-portrait frequency pair separately. |
| Fig. S6 | `supplementary/alldens_2sp.ipynb`, full-model scan, no-infanticide reference, and ecological/evolutionary plots. | Scan common cost and competition using the default demographic parameters. |

For the phase portraits in Fig. 4B–E, change only `p1` and `p2` from their defaults `(0.0,0.8)` to the pairs listed in the table above. The outcome-plane scan for Fig. 4A varies both frequencies automatically.

For **Fig. 5 and Figs. S1–S2**, the shared parameters are `alpha1=alpha2=1.0`, `beta1=beta2=1.0`, `gamma1=gamma2=0.01`, `d1=d2=0.001`, `b_N1=2.0`, and `b_N2=1.0`. The supplementary captions specify these parameter pairs:

| Panel | `s1=s2` | `rho12=rho21` |
| --- | --- | --- |
| A | 0.9 | 0.7 |
| B | 0.7 | 0.9 |
| C | 0.1 | 0.9 |
| D | 0.9 | 0.4 |
| E | 0.2 | 0.9 |
| F | 0.2 | 0.3 |
| G | 0.3 | 0.8 |
| H | 0.6 | 0.9 |
| I | 0.6 | 0.2 |

These supplementary trajectories start from `N1=N2=10` and `p1=p2=0.5`, with equal total male and female densities and equal densities across the five female classes.

### Supplementary alternative-model settings

All four supplementary notebooks currently use `alpha1=alpha2=1.0`, `beta1=beta2=1.0`, and `d1=d2=0.001`. Their remaining defaults are:

| Notebook | Non-infanticidal reproductive output, species 1 / 2 | `gamma1=gamma2` | `s1=s2` | `rho12=rho21` | Fixed `(p1,p2)` |
| --- | --- | --- | --- | --- | --- |
| `nodens_2sp_QSS.ipynb` | `r_N1=1.2`, `r_N2=1.0` | 2.0 | 0.1 | 1.0 | `(0.8,0.2)` |
| `nodens_2sp.ipynb` | `b_N1=2.0`, `b_N2=1.0` | 1.0 | 0.1 | 1.0 | Evolving |
| `alldens_2sp_QSS.ipynb` | `r_N1=1.2`, `r_N2=1.0` | 0.01 | 0.1 | 1.0 | `(0.8,0.2)` |
| `alldens_2sp.ipynb` | `b_N1=2.0`, `b_N2=1.0` | 0.01 | 0.1 | 1.0 | Evolving |

## License

The code is distributed under the [MIT License](LICENCE).
