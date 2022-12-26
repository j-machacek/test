---
    layout: default
    title: Differential Evolution - JADE
    parent: MEALPY
    grand_parent: Optimization algorithms
---
## Differential Evolution - JADE
A modification of the classical differential evolution (DE) algorithm. JADE, was proposed by Zhang et al. [1] to improve optimization performance by implementing a new mutation strategy with optional external archive and updating control parameters in an adaptive manner. The parameter adaptation automatically updates the control parameters to appropriate values and avoids a user's prior knowledge of the relationship between the parameter settings and the characteristics of optimization problems. 

### Usage
The Differential Evolution optimization algorithm is accessible via the mealpy library using the following command:
```python
ACTmealpy.optimize(maxiter=100, n_cpu=32, method='DifferentialEvolution-JADE')
```

### Example

Exemplarily, the results of a calibration of a hypoplastic model ($\varphi_c$, $h_s$, $n$, $e_{c0}$, $e_{d0}$, $e_{i0}$, $\alpha$, $\beta$) for Karlsruhe Fine Sand (BMU-Sand) are shown by means of drained monotonic triaxial tests:

<img src="./jade/triaxCD.png" alt="triaxCD" width="66%"/>

and oedometric compression tests:

<img src="./jade/oedometer.png" alt="oedometer" width="33%"/>

The development of the global and local cost function is shown in the figure below. It can be seen that the value of the cost function stagnates over 15 iterations. This was defined as a termination criterion for the optimization.

<img src="./jade/fitness_function.png" alt="fitness_function" width="66%"/>

To get an (incomplete) impression about the reproducibility of the results, we repeat the calibration five times. The achieved values of the cost function as well as the required computing time per run (2x AMD Ryzen Threadripper PRO 3955WX 16-Cores, 3900 MHz, WSL2) are shown below.

<img src="./jade/statistics.png" alt="statistics" width="66%"/>

The influence of the scatter in the costfunction on the simulation outcome is shown below (from large variations in cost functions, large variations in simulation results are expected):

<img src="./jade/triaxCD_all.png" alt="triaxCD_all" width="66%"/>


### References
[1] J. Zhang and A. C. Sanderson, "JADE: Adaptive Differential Evolution With Optional External Archive," in _IEEE Transactions on Evolutionary Computation_, vol. 13, no. 5, pp. 945-958, Oct. 2009, doi:10.1109/TEVC.2009.2014613. [[https://ieeexplore.ieee.org/document/5208221]]
