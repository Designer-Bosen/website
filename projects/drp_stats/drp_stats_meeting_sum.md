[← Back](index.html)

# Weekly Meeting Summary

## Week 2

- A simulation-based study is a feasible direction, given that theoretical derivations may be complex and time-consuming.

- curvature assumption such as RSC should be established on the loss function, not the prior. 

- Even with independent priors, the network dependence in the data leads to a complex posterior dependence structure.

- Sampling in a sequential order (e.g. Gibbs sampling / coordinate-wise MCMC)

    Sample $\beta_1 \mid \text{Other Parameters}$

    Sample $\beta_2 \mid \text{Other Parameters}$

    $\dots$

- MCMC could be computationally very expensive, so Variational Inference could be one choice to approximate posterior distribution.