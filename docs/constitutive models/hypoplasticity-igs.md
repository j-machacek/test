---
    layout: default
    title: Hypoplasticity + Intergranular Strain (Hypo+IGS)
    parent: Constitutive models
    katex: true
---
# Hypoplasticity + Intergranular Strain (Hypo+IGS)

Hypoplastic model for sands, von Wolffersdorff version [1] with InterGranular Strain (IGS) extension by Niemunis and Herle [2]. For details on the implementation see [3].

### Parameters
* $\nu$ - key: `nu` - if ommited the basic hypoplastic model of *von Wolffersdorff* is recovered
* $\varphi_c$ - key: `phic` - provided in rad.
* $h_s$ - key: `hs` - provided in GPa
* $n$ - key: `n` 
* $\alpha$ - key: `alpha` 
* $\beta$ - key: `beta` 
* $e_{c0}$ - key: `ec` 
* $e_{d0}$ - key: `ed`
* $f_{ei} = e_{i0}/e_{c0}$  - key: `ei` - typically in the range of 1.15 - 1.2

### Bounds
The default values for the bounds are:
* $0.0 \leq \nu \leq 0.4$ 
* $0.9 \varphi_{c0} \leq \varphi_c \leq 1.1\varphi_{c0}$, where $\varphi_{c0}$ is a value submitted by the user
* $10^{-4} \leq h_s \leq 75$ - provided in GPa
* $0.1 \leq n \leq 1$
* $0 \leq \alpha \leq 1$
* $0 \leq \beta \leq 5$
* $0.9 e_{c00} \leq e_{c0} \leq 1.1e_{c00}$, where $e_{c00}$ is a value submitted by the user
* $0.9 e_{d00} \leq e_{d0} \leq 1.1e_{d00}$, where $e_{d00}$ is a value submitted by the user
* $f_{ei} = e_{i0}/e_{c0}$ - typically in the range of 1.15 - 1.2

### Notes
* By setting $m_R=m_T=1$ the IGS extension is deactivated and the original hypoplastic model is recovered.
* The Poisson's ratio $\nu$ is a modification of the original formulation introduced by A. Niemunis [4] to have more flexibility in fitting the inclination of the first loading cycle in $p-q$ space in cyclic triaxial tests. By setting $\nu=0$, the original von Wolffersdorff version is recovered.
* The reference values $\varphi_{c0}, e_{c00}$ and $e_{d00}$ are usually chosen as the angle of repose, the maximum void ratio $e_{max}$ and minimum void ratio $e_{min}$ obtained from laboratory tests, respectively.
  
### Example
```python
import numpy as np
import sys,os
  
import ACT.globals as ACTglobals
import ACT.mealpy as ACTmealpy
import ACT.excel as ACTexcel
import ACT.weights as ACTweights

# Load python library for the calibration
from ACT.hypoplasticity import hypoplasticity

# Read in data
excelfile = './Database_BMU_Sand.xlsx'
exp_oedometer, exp_triaxCD, exp_triax_CUcyc = ACTexcel.collect(excelfile)

 # empty the cyclic triaxial tests, we only want to calibrate the "monotonic" parameters
exp_triax_CUcyc = []

# smoothen the data...
for oedo in exp_oedometer:
    oedo.interpolate(N=50)

for triax in exp_triaxCD:
    triax.interpolate(N=50)

# Start optimization
hypo = hypoplasticity()

# initialize some values
hypo.set(ec=1.054, ed=0.677, ei=1.15, phic=33.1/180*np.pi, alpha=0.14, beta=2.5, R=1e-4, mT=1., mR=1.)

# specifies which values should be updated/considered during calibration
to_optimize = ['hs','n']

ACTglobals.setup(
  Model = hypo,
  Free_parameter = to_optimize,
  oedometer = exp_oedometer,
  triaxCD = exp_triaxCD,
  triaxCUcyc = exp_triax_CUcyc,
  Similarity = 'frechet',
  path = os.getcwd())

ACTmealpy.optimize(maxiter=200, n_cpu=4, method='DifferentialEvolution')
```

### References
[1] P.-A. von Wolffersdorff, ‘A hypoplastic relation for granular materials with a predefined limit state surface’, _Mechanics of Cohesive-frictional Materials_, vol. 1, no. 3, pp. 251–271, 1996, doi: [10.1002/(SICI)1099-1484(199607)1:3&lt;251::AID-CFM13&gt;3.0.CO;2-3](https://doi.org/10.1002/%28SICI%291099-1484%28199607%291:3<251::AID-CFM13>3.0.CO;2-3).

[2] A. Niemunis and I. Herle, ‘Hypoplastic model for cohesionless soils with elastic strain range’, _Mechanics of Cohesive-frictional Materials_, vol. 2, no. 4, pp. 279–299, 1997, doi: [10.1002/(SICI)1099-1484(199710)2:4&lt;279::AID-CFM29&gt;3.0.CO;2-8](https://doi.org/10.1002/%28SICI%291099-1484%28199710%292:4<279::AID-CFM29>3.0.CO;2-8).

[3] J. Machaček, P. Staubach, M. Tafili, H. Zachert, and T. Wichtmann, ‘Investigation of three sophisticated constitutive soil models: From numerical formulations to element tests and the analysis of vibratory pile driving tests’, _Computers and Geotechnics_, vol. 138, p. 104276, Oct. 2021, doi: [10.1016/j.compgeo.2021.104276](https://doi.org/10.1016/j.compgeo.2021.104276).

[4] A. Niemunis, ‘Extended hypoplastic models for soils’, Habilitation, Ruhr-Universität Bochum, Bochum, 2003.
