---
    layout: default
    title: Arithmetic Optimization
    parent: MEALPY
    grand_parent: Optimization algorithms
    nav_order: 1
    katex: true
---
## Arithmetic Optimization

### Usage
The Arithmetic optimization algorithm is accessible via the mealpy library using the following command:
```python
ACTmealpy.optimize(maxiter=100, n_cpu=4, method='ArithmeticOptimization')
```

### Example
Exemplarily, the results of a calibration of a hypoplastic model ($\phi_c$, $h_s$, $n$, $e_{c0}$, $e_{d0}$, $e_{i0}$, $\alpha$, $\beta$) for Karlsruhe Fine Sand (BMU-Sand) are shown by means of drained monotonic triaxial tests:

<img src="./aoa/triaxCD.png" alt="triaxCD" width="66%"/>

and oedometric compression tests:

<img src="./aoa/oedometer.png" alt="oedometer" width="33%"/>

The development of the global and local cost function is shown in the figure below. It can be seen that the value of the cost function stagnates over 15 iterations. This was defined as a termination criterion for the optimization.

<img src="./aoa/fitness_function.png" alt="fitness_function" width="66%"/>

To get an (incomplete) impression about the reproducibility of the results, we repeat the calibration five times. The achieved values of the cost function as well as the required computing time per run (2x AMD Ryzen Threadripper PRO 3955WX 16-Cores, 3900 MHz, WSL2) are shown below.

<img src="./aoa/statistics.png" alt="statistics" width="66%"/>

The influence of the scatter in the costfunction on the simulation outcome is shown below (from large variations in cost functions, large variations in simulation results are expected):

<img src="./aoa/triaxCD_all.png" alt="triaxCD_all" width="66%"/>

### References
[1] 
