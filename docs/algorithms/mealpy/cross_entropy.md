---
    layout: default
    title: Cross-Entropy Method
    parent: MEALPY
    grand_parent: Optimization algorithms
---
## Cross-Entropy Method

The Cross-Entropy Method (CEM) is a probabilistic optimization belonging to the field of Stochastic Optimization. It was developed as an efficient estimation technique for rare-event probabilities in discrete event simulation systems by Rubinstein in 1997 [1] and was adapted for use in optimization. Detailed introduction in CEM is given in [2].

The information processing strategy of the algorithm is to sample the problem space and approximate the distribution of good solutions. This is achieved by assuming a distribution of the problem space (such as Gaussian), sampling the problem domain by generating candidate solutions using the distribution, and updating the distribution based on the better candidate solutions discovered. Samples are constructed stepwise (one component at a time) based on the summarized distribution of good solutions. As the algorithm progresses, the distribution becomes more refined until it focuses on the area or scope of optimal solutions in the domain.

### Usage
The Cross-Entropy Method algorithm is accessible via the mealpy library using the following command:
```python
ACTmealpy.optimize(maxiter=300, n_cpu=32, method='CrossEntropy')
```
### Example
Exemplarily, the results of a calibration of a hypoplastic model ($\phi_c$, $h_s$, $n$, $e_{c0}$, $e_{d0}$, $e_{i0}$, $\alpha$, $\beta$) for Karlsruhe Fine Sand (BMU-Sand) are shown by means of drained monotonic triaxial tests:

<img src="./cem/triaxCD.png" alt="triaxCD" width="66%"/>

and oedometric compression tests:

<img src="./cem/oedometer.png" alt="oedometer" width="33%"/>

The development of the global and local cost function is shown in the figure below. It can be seen that the value of the cost function stagnates over 15 iterations. This was defined as a termination criterion for the optimization.

<img src="./cem/fitness_function.png" alt="fitness_function" width="66%"/>

To get an (incomplete) impression about the reproducibility of the results, we repeat the calibration five times. The achieved values of the cost function as well as the required computing time per run (2x AMD Ryzen Threadripper PRO 3955WX 16-Cores, 3900 MHz, WSL2) are shown below.

<img src="./cem/statistics.png" alt="statistics" width="66%"/>

The influence of the scatter in the costfunction on the simulation outcome is shown below (from large variations in cost functions, large variations in simulation results are expected):

<img src="./cem/triaxCD_all.png" alt="triaxCD_all" width="66%"/>

### References
[1] R. Y. Rubinstein. Optimization of computer simulation models with rare events. European Journal of Operations Research, 99:89–112, 1997. [[[https://doi.org/10.1016/S0377-2217(96)00385-2](https://doi.org/10.1016/S0377-2217%2896%2900385-2 "Persistent link using digital object identifier")]]

[2] P. T. De Boer, D. P. Kroese, S. Mannor, and R. Y. Rubinstein. A tutorial on the cross-entropy method. Annals of Operations Research, 134(1):19–67, 2005. [[https://doi.org/10.1007/s10479-005-5724-z]]
