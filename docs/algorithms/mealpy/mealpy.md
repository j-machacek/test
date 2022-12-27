---
    layout: default
    title: MEALPY
    nav_order: 1
    parent: Optimization algorithms
    has_children: true
    permalink: /docs/mealpy/mealpy
---
# MEALPY

[MEALPY](https://github.com/thieu1995/mealpy.git) [1] is a Python library for the most of cutting-edge population *meta-heuristic* algorithms - a field which provides an fast and efficient way to find the global optimal point of mathematical optimization problems.

The MEALPY library has been integrated into **numgeo-ACT** in such a way that it is very easy to access different algorithms for optimization via a simple interface. To access this interface, the MEALPY module must first be imported from **numgeo-ACT**:

```python
import ACT.mealpy as ACTmealpy
```

Then (after some more steps to read in the experimental data and choose the constitutive model and the weighting factors) the calibration of the constitutive model parameters can be started via the following interface:

```python
...
ACTmealpy.optimize(maxiter=300, n_cpu=32, method='DifferentialEvolution')
```

The interface has five optional arguments:
* `maxiter`: Maximum number of iterations to be performed before the optimization stops, default is `maxiter=200`.
* `exit_iter`: Early stoppage criterion. If the global best solution not better an epsilon after `exit_iter` iterations then stop the optimization. Default is `exit_iter=30`
* `popsize`: Population size defined as the next power to two of `N*popsize`, where `N` is the dimensionality of the problem (= number of parameters of the constitutive model to be optimized). Default is `popsize=15`.
* `method` : Optimization algorithm to be used. Currently the following algorithms are:
    * `DifferentialEvolution`, see [[differential_evolution]] Differential Evolution
    * `DifferentialEvolution-JADE`, see [[differential_evolution_jade]] Differential Evolution - JADE
    * `DifferentialEvolution-LSHADE`, see [[differential_evolution_lshade]] Differential Evolution - LSHADE

{:toc}

### References
[1] N. V. Thieu and S. Mirjalili, ‘MEALPY: a framework of the state-of-the-art meta-heuristic algorithms in python’. Zenodo, Jun. 2022. doi: [10.5281/zenodo.6684223](https://doi.org/10.5281/zenodo.6684223).