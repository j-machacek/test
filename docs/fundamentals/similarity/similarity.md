---
    layout: default
    title: Similarity measures
    parent: Fundamentals
    has_children: true
    katex: true
---
## Similarity measures
{: .no_toc }

The cost functions driving the optimization of the parameters are built by accounting for the discrepancy between the numerical predictions and the measured (experimental) data. Traditionally, this discrepancy is often quantified using a sum-of-square based cost function. However, in the present case this is not possible. One problem is a potentially different number of data points on the experimental curve compared to the numerical curve. Admittedly, this circumstance could be remedied by linear interpolation - provided there are enough data points to keep the introduced error low. However, in many cases there is no unique relationship between stress and strain - the material behavior is path dependent. Besides our own implementations we use the python tool similaritymeasures by Charles Jekel [11]. The latter gives access to the following measurements:


