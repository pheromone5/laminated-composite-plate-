# Laminated Composite Plate Optimisation

Surrogate-assisted GA and PSO for maximising the fundamental natural frequency of a 16-ply symmetric carbon/epoxy laminate. Notebook: `Simple_GA_FSDT_CLT_(2).ipynb`.

## Problem
Choose 8 independent ply angles (a symmetric 16-ply layup, angles in [-90°, 90°]) to maximise the first natural frequency f1 of a simply supported (SSSS) square plate.

| Parameter | Value |
|---|---|
| Material | T300/5208-type carbon/epoxy |
| E1, E2, G12 | 181, 10.3, 7.17 GPa |
| ν12, ρ | 0.28, 1600 kg/m³ |
| Ply thickness | 0.25 mm |
| Plate | 1 m x 1 m, 10 x 10 mesh |

## Method
- **Reference model:** First-order shear deformation theory (FSDT / Mindlin) 4-node plate finite elements, with a dense eigen-solve for the frequencies.
- **Fast model:** Classical lamination theory (CLT) stiffness features (D11, D12, D22, D16, D26, D66, S11, S12, S22).
- **Surrogate:** MLP regressor (32-32, tanh, L-BFGS) trained on a 40-point design of experiments, then refined with 18 true-FEM infill points per cycle (20 cycles).
- **Optimisers:** hand-written GA (tournament selection, arithmetic crossover, Gaussian mutation, elitism) and PSO, each run on the surrogate.

## Results
Baseline f1 = 25.13 Hz.

| Method | Best f1 (Hz) | Gain |
|---|---|---|
| Direct GA (400 FEM calls) | 28.27 | +12.5% |
| Surrogate-assisted GA | 28.40 | +13.0% |
| Surrogate-assisted PSO | 28.36 | +12.9% |

*Check these figures against the notebook outputs before publishing; they come from a single seeded run.*

## Run it
```bash
pip install numpy scipy scikit-learn pandas matplotlib jupyter
jupyter notebook "Simple_GA_FSDT_CLT_(2).ipynb"
```
