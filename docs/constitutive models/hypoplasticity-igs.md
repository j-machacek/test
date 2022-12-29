---
    layout: default
    title: Hypoplasticity + Intergranular Strain (Hypo+IGS)
    parent: Constitutive models
    katex: true
---
# Hypoplasticity + Intergranular Strain (Hypo+IGS)
{: .no_toc }

Hypoplastic model for sands, von Wolffersdorff version [1] with InterGranular Strain (IGS) extension by Niemunis and Herle [2]. For details on the implementation see [3].

## Table of contents
{: .no_toc .text-delta }
1. TOC
{:toc}

## Parameters
**Hypoplasticity**
* $\nu$ - key: `nu` - if omitted the basic hypoplastic model of *von Wolffersdorff* is recovered
    > The Poisson's ratio $\nu$ is a modification of the original formulation introduced by A. Niemunis [4] to have more flexibility in fitting the inclination of the first loading cycle in $p-q$ space in cyclic triaxial tests. By setting $\nu=0$, the original von Wolffersdorff version is recovered.
* $\varphi_c$ - key: `phic` - provided in rad.
* $h_s$ - key: `hs` - provided in GPa
* $n$ - key: `n` 
* $\alpha$ - key: `alpha` 
* $\beta$ - key: `beta` 
* $e_{c0}$ - key: `ec` 
* $e_{d0}$ - key: `ed`
* $f_{ei} = e_{i0}/e_{c0}$  - key: `ei` - typically in the range of 1.15 - 1.2

> The reference values $\varphi_{c0}, e_{c00}$ and $e_{d00}$ are usually chosen as the angle of repose, the maximum void ratio $e_{max}$ and minimum void ratio $e_{min}$ obtained from laboratory tests, respectively.

**Intergranular Strain (IGS)**
* $m_R$ - key `mR`
* $m_T$ - key `mT`
* $\beta_R$ - key `betaR`
* $R$ - key `R`
* $\chi$ - key `chi`
    > Note: by setting $m_R=m_T=1$ the IGS extension is deactivated and the original hypoplastic model recovered.

## Bounds
The default values for the bounds are:

**Hypoplasticity**
* $0.0 \leq \nu \leq 0.4$ 
* $0.9 \varphi_{c0} \leq \varphi_c \leq 1.1\varphi_{c0}$, where $\varphi_{c0}$ is a value submitted by the user
* $10^{-4} \leq h_s \leq 75$ - provided in GPa
* $0.1 \leq n \leq 1$
* $0 \leq \alpha \leq 1$
* $0 \leq \beta \leq 5$
* $0.9 e_{c00} \leq e_{c0} \leq 1.1e_{c00}$, where $e_{c00}$ is a value submitted by the user
* $0.9 e_{d00} \leq e_{d0} \leq 1.1e_{d00}$, where $e_{d00}$ is a value submitted by the user
* $f_{ei} = e_{i0}/e_{c0}$ - typically in the range of 1.15 - 1.2

**Intergranular Strain**
* $1 \leq m_R \leq 15$
* $1 \leq m_T \leq 15$
* $10^{-5} \leq R \leq 5\cdot10^{-4}$
* $0 \leq \beta_R \leq 10$
* $0.1 \leq \chi \leq 15$

## Constraints
If either $m_T$ or $m_R$ or both are active during optimization, we enforce $m_T \leq m_R$ using a simple penalty approach.
 
## Usage

To access this interface, the `hypoplasticity` module must first be imported from **numgeo-ACT**:

```python
from ACT.hypoplasticity import hypoplasticity
```
Then (after some more steps to read in the experimental data and choose the constitutive model and the weighting factors) the `hypoplasticity` class can be initialized:

```python
model = hypoplasticity()
```

In a next step we initialize some of the parameters of the hypoplastic model:
```python
model.set(ec=1.054, ed=0.677, ei=1.15, phic=33.1/180*np.pi, R=1e-4, mT=1., mR=1.)
```
We initialize $\varphi_{c0}, e_{c00}$ and $e_{d00}$ as the angle of repose, the maximum void ratio $e_{max}$ and minimum void ratio $e_{min}$ obtained from laboratory tests, respectively. 

In the last step we specify which parameters we want to vary during optimization. In the present case we want to optimize all parameters of the hypoplastic model. By excluding the parameters of the IGS extension from this list and at the same time initializing $m_R=m_T=1$ we deactivate the IGS extension.

```python
to_optimize = \['phic','hs','n','alpha','beta','ed','ec'\]
```
We have now successfully set up the hypoplatic constitutive model for optimization by one of the implemented optimization algorithms $\rightarrow$ [Optimization algorithms]({% link docs/algorithms/algorithms.md %})

## Constitutive Equations

The basic hypoplastic model of von Wolfersdorff [1] interrelates the Jaumann stress rate $\boldsymbol{\mathring{\sigma}}$ with the strain rate $\boldsymbol{\dot{\varepsilon}}$:

$$
\boldsymbol{\mathring{\sigma}} = \mathsf{L}:\boldsymbol{\dot{\varepsilon}} + \boldsymbol{N} ~\|\boldsymbol{\dot{\varepsilon}\|} 
$$

 $\mathsf{L}$ is a fourth order tensor being linear in $\boldsymbol{\dot{\varepsilon}}$, whereas $\boldsymbol{N}$ is a second order tensor being non-linear in $\boldsymbol{\dot{\varepsilon}}$. Both stiffness tensors $\mathsf{L}$ and $\boldsymbol{N}$ are functions of stress and void ratio. One can rewrite above equation with a fourth-order stiffness tensor $\mathsf{M}$:

$$
\boldsymbol{\mathring{\sigma}} = \mathsf{M}(\boldsymbol{\sigma},e) : \boldsymbol{\dot{\varepsilon}} = \left(\mathsf{L} + \boldsymbol{N} \frac{\boldsymbol{\dot{\varepsilon}}}{\|\boldsymbol{\dot{\varepsilon}}\|} \right): \boldsymbol{\dot{\varepsilon}} \nonumber
$$

For a detailed presentation of the equations for $\mathsf{L}$ and $\boldsymbol{N}$ it is referred to [1]. 

To improve the performance of the hypoplastic model in the range of small strain cycles Niemunis & Herle [2] introduced a new tensorial state variable, the intergranular strain $\textbf{h}$, which memorizes the recent deformation history. With this extension the basic equation of the constitutive model reads:

$$
\boldsymbol{\mathring{\sigma}} = \mathsf{M}(\boldsymbol{\sigma},\textbf{h},e) : \boldsymbol{\dot{\varepsilon}} 
$$

where $\boldsymbol{\dot{\varepsilon}}$ is the strain rate. The stiffness tensor $\mathsf{M}$ is defined by [4].

$$
	\mathsf{M}=\Big(\rho^{\chi}m_{T} + (1-\rho^{\chi})m_{R}\Big)\mathsf{L} \\ + \Bigg\{ \begin{array}{ll} \rho^{\chi}(1-m_{T})\mathbf{\mathsf{L}}:\vec{\mathbf {h}}\otimes\vec{\mathbf{h}} + \rho^{\chi}\mathbf{N}\vec{\mathbf{h}} \\ \rho^{\chi}(m_{R}-m_{T})\mathsf{L}:\vec{\mathbf{h}}\otimes\vec{\mathbf{h}} \end{array} ~~~\textrm{for}~~~ \begin{array}{ll}\vec{\mathbf{h}}:\boldsymbol{\dot{\varepsilon}}>0 \\ \vec{\mathbf{h}}:\boldsymbol{\dot{\varepsilon}}\le 0\end{array}
$$

with the intergranular strain tensor $\textbf{h}$, its degree of mobilization $\rho$ and its direction $\vec{\mathbf{h}}$ defined as

$$
	\rho=\frac{\|\textbf{h}\|}{R}~~\textrm{and}~~\vec{\textbf{h}}=\frac{\mathbf{h}}{\|\textbf{h}\|}.
$$

$\chi$, $m_T$, $m_R$ and $R$ are material parameters controlling the influence of the intergranular strain. The evolution law of the intergranular strain $\textbf{h}$ is [2]:

$$
	\mathring{\textbf{h}} = \Bigg\{
	\begin{array}{ll}
		(\mathbf{\sf{I}}-\vec{\textbf{h}}\otimes\vec{\textbf{h}}\rho^{\beta_r}) : \boldsymbol{\dot{\varepsilon}}\\
		\boldsymbol{\dot{\varepsilon}}
	\end{array} ~~~\textrm{for}~~~
	\begin{array}{ll}\vec{\mathbf{h}}:\boldsymbol{\dot{\varepsilon}}>0 \\ \vec{\mathbf{h}}:\boldsymbol{\dot{\varepsilon}}\le 0\end{array}
$$

where $\beta_R$ is another material parameter and $\mathbf{\sf{I}}$ is the fourth-order identity tensor. The stiffness tensors $\mathsf{L}$ and $\mathbf{N}$  are given by the following equations [1]:

$$
\begin{aligned}
	\mathsf{L} & = \tilde{f}_{e_i} f_b f_e \frac{1}{\text{tr}(\hat{\boldsymbol{\sigma}}\cdot\hat{\boldsymbol{\sigma}})} (F^2\mathsf{I}+a^2\hat{\boldsymbol{\sigma}}\otimes\hat{\boldsymbol{\sigma}}) \\
	\textbf{N}&=\tilde{f}_{e_i} f_b f_e f_d \frac{Fa}{\text{tr}(\hat{\boldsymbol{\sigma}}\cdot\hat{\boldsymbol{\sigma}})} (\hat{\boldsymbol{\sigma}}+\hat{\boldsymbol{\sigma}}^*)
 \end{aligned}
$$

Therein $\hat{\boldsymbol{\sigma}}=\dfrac{\boldsymbol{\sigma}}{\text{tr}\boldsymbol{\sigma}}$  and $\hat{\boldsymbol{\sigma}}^{*}=\hat{\boldsymbol{\sigma}}-\frac{1}{3}\mathbf{I}$ are used. The scalar factors are defined by

$$
\begin{aligned}
	a=\frac{\sqrt{3}(3-\sin\varphi_c)}{2\sqrt{2}\sin\varphi_c}~,~
	f_d=\left(\frac{e-e_d}{e_c-e_d}\right)^\alpha~,~f_e=\left(\frac{e_c}{e}\right)^\beta
\end{aligned}
$$

and

$$
\begin{aligned}
	f_{b}=\frac{h_s}{n}\left(\frac{e_{i0}}{e_{c0}}\right)^{\beta}\frac{1+e_i}{e_i}\left(\frac{3p}{h_s}\right)^{1-n}\left[3 +a^2 -a\sqrt{3}\left(\frac{e_{i0}-e_{d0}}{e_{c0}-e_{d0}}\right)^{\alpha}\right]^{-1}.
\end{aligned}
$$

$\varphi_c$, $h_s$, $n$, $e_{i0}$, $e_{d0}$, $e_{c0}$, $\alpha$ and $\beta$ are parameters and $e$ is the actual void ratio. The pressure-dependent void ratios $e_i$, $e_c$ and $e_d$, describing the loosest, the critical and the densest state, are calculated using the following relation [5]:

$$
\begin{aligned}
	\frac{e_i}{e_{i0}}=\frac{e_c}{e_{c0}}=\frac{e_d}{e_{d0}}=\exp\left[-\left(\frac{3p}{h_s}	\right)^n	\right].
\end{aligned}
$$

$p$ is the mean effective stress. The scalar factor $F$ is given by

$$
\begin{aligned}
	F=\sqrt{\frac{1}{8}\tan(\psi)^2+\frac{2-\tan(\psi)^2}{2+\sqrt{2}\tan(\psi)\cos(3\theta)}}
	-\frac{1}{2\sqrt{2}}\tan(\psi),
\end{aligned}
$$

with $\tan(\psi)=\sqrt{3}\|\hat{\boldsymbol{\sigma}}^*\|$ and

$$
\begin{aligned}
	\cos(3\theta)=-\sqrt{6}\frac{\text{tr}(\hat{\boldsymbol{\sigma}}^*\cdot\hat{\boldsymbol{\sigma}}^*\cdot\hat{\boldsymbol{\sigma}}^*)}
	{\left[\text{tr}(\hat{\boldsymbol{\sigma}}^*\cdot\hat{\boldsymbol{\sigma}}^*)\right]^{3/2}}.
\end{aligned}
$$

The present implementation uses an adaptive explicit Euler scheme to consider the rate of convergence within the substepping scheme. For more details on the implementation reference to [3] is made.

## References
[1] P.-A. von Wolffersdorff, ‘A hypoplastic relation for granular materials with a predefined limit state surface’, Mechanics of Cohesive-frictional Materials, vol. 1, no. 3, pp. 251–271, 1996, doi: [10.1002/(SICI)1099-1484(199607)1:3&lt;251::AID-CFM13&gt;3.0.CO;2-3](https://doi.org/10.1002/%28SICI%291099-1484%28199607%291:3<251::AID-CFM13>3.0.CO;2-3).

[2] A. Niemunis and I. Herle, ‘Hypoplastic model for cohesionless soils with elastic strain range’, Mechanics of Cohesive-frictional Materials, vol. 2, no. 4, pp. 279–299, 1997, doi: [10.1002/(SICI)1099-1484(199710)2:4&lt;279::AID-CFM29&gt;3.0.CO;2-8](https://doi.org/10.1002/%28SICI%291099-1484%28199710%292:4<279::AID-CFM29>3.0.CO;2-8).

[3] J. Machaček, P. Staubach, M. Tafili, H. Zachert, and T. Wichtmann, ‘Investigation of three sophisticated constitutive soil models: From numerical formulations to element tests and the analysis of vibratory pile driving tests’, Computers and Geotechnics, vol. 138, p. 104276, Oct. 2021, doi: [10.1016/j.compgeo.2021.104276](https://doi.org/10.1016/j.compgeo.2021.104276).

[4] A. Niemunis, ‘Extended hypoplastic models for soils’, Habilitation, Ruhr-Universität Bochum, Bochum, 2003.

[5] E. Bauer, ‘Calibration of a Comprehensive Hypoplastic Model for Granular Materials’, Soils and Foundations, vol. 36, no. 1, pp. 13–26, Mar. 1996, doi: [10.3208/sandf.36.13](https://doi.org/10.3208/sandf.36.13).
