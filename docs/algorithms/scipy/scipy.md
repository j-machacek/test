---
    layout: default
    title: SciPy
    nav_order: 2
    parent: Optimization algorithms
    has_children: true
    permalink: /scipy/scipy
    has_toc: false
---
# SciPy

[SciPy](https://scipy.org/) [1] is a Python library providing algorithms for optimization, integration, interpolation, eigenvalue problems, algebraic equations, differential equations, statistics and many other classes of problems.

The SciPy library has been integrated into **numgeo-ACT** in such a way that it is very easy to the Differential Evolution algorithm. To access this interface, the SciPy module must first be imported from **numgeo-ACT**:

```python
import ACT.scipy as ACTscipy
```

Then (after some more steps to read in the experimental data and choose the constitutive model and the weighting factors) the calibration of the constitutive model parameters can be started via the following interface:

```python
...
ACTscipy.optimize(maxiter=300, n_cpu=32, method='DifferentialEvolution')
```

The interface has five optional arguments:
* `maxiter`: Maximum number of iterations to be performed before the optimization stops, default is `maxiter=200`.
* `popsize`: Population size defined as the next power to two of `N*popsize`, where `N` is the dimensionality of the problem (= number of parameters of the constitutive model to be optimized). Default is `popsize=15`.
* `method` : Optimization algorithm to be used. Currently the following algorithms are:
    * `'DifferentialEvolution'`: Differential Evolution


### References
[1] P. Virtanen et al., ‘SciPy 1.0: fundamental algorithms for scientific computing in Python’, Nat Methods, vol. 17, no. 3, pp. 261–272, Mar. 2020, doi: [10.1038/s41592-019-0686-2](https://doi.org/10.1038/s41592-019-0686-2).