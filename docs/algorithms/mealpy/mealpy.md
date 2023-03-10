---
    layout: default
    title: MEALPY
    nav_order: 1
    parent: Optimization algorithms
    has_children: true
    permalink: /mealpy/mealpy
    has_toc: false
---
# MEALPY
{: .no_toc }

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
    * `ArithmeticOptimization`, see [Arithmetic Optimization]({% link algorithms/mealpy/arithmetic_optimization.md%})
    * `CrossEntropy`, see [Cross-Entropy Method]({% link algorithms/mealpy/cross_entropy.md%})
    * `DifferentialEvolution`, see [Differential Evolution]({% link algorithms/mealpy/differential_evolution.md%})
    * `DifferentialEvolution-JADE`, see [Differential Evolution - JADE]({% link algorithms/mealpy/differential_evolution_jade.md%})
    * `DifferentialEvolution-LSHADE`, see [Differential Evolution - LSHADE]({% link algorithms/mealpy/differential_evolution_lshade.md%})
    * `GeneticAlgorithm`, see [Genetic Algorithm]({% link algorithms/mealpy/genetic_algorithm.md%})
    * `GradientBased`, see [Gradient Based Optimization]({% link algorithms/mealpy/gradient_based.md%})
    * `ParticleSwarmOptimization-CL`, see [Particle Swarm Optimization - Comprehensive Learning]({% link algorithms/mealpy/particle_swarm_optimization_cl.md%})
    * `ParticleSwarmOptimization-TVAC`, see [Particle Swarm Optimization Hierarchical Time-Varying Acceleration]({% link algorithms/mealpy/particle_swarm_optimization_tvac.md %})
    

### References
[1] N. V. Thieu and S. Mirjalili, ???MEALPY: a framework of the state-of-the-art meta-heuristic algorithms in python???. Zenodo, Jun. 2022. doi: [10.5281/zenodo.6684223](https://doi.org/10.5281/zenodo.6684223).