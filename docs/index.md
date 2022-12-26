---
title: Home
layout: home
nav_order: 1
---
![numgeoACT_logo_text.jpg](./images/numgeoACT_logo_text.jpg "numgeoACT_logo_text.png")

## Automatic Calibration Tool (ACT) based on the fem-program **numgeo** (Machaček & Staubach, [www.numgeo.de])

Copyright (C) 2021-2022 Jan Machaček 



## Status of implementation and roadmap for the future
- [ ] **Constitutive models**
    - [x] Hypoplasticity
    - [ ] Intergranular strain
    - [x] Sanisand (monotonic)
    - [ ] Hypo + ISA
    - [x] HCA - Sand (note that the current numgeo version is required)
    - [ ] HCA - Clay
    - [ ] HCA - AVISA
    - [x] ISA-Sand
- [ ] **Laboratory tests in database:**
    - [x] Oedometric compression tests
    - [x] Drained monotonic triaxial tests
    - [ ] Undrained monotonic triaxial tests
    - [x] Undrained cyclic triaxial tests
    - [x] Undrained cyclic triaxial tests for HCA
- [ ] **Possibility to read all experimental data from one single excel file**
    - [x] Oedometric compression tests
    - [x] Drained monotonic triaxial tests
    - [ ] Undrained monotonic triaxial tests
    - [x] Undrained cyclic triaxial tests - strain controlled
    - [ ] Undrained cyclic triaxial tests - stress controlled
    - [ ] Oedometric compression tests - strain rate jumps
- [ ] **Similarity measures, see [Cost function]**
    - [x] Dynamic time warping (DTW)
    - [x] Partial Curve Mapping
    - [x] Frechet Distance
    - [x] Area between two curves
    - [x] Curve Length Measure
    - [x] Least square
    - [x] Modified least square after [12]
    - [ ] Soil mechanics motivated similarity measures
- [ ] **Output**
    - [ ] Summary as PDF
