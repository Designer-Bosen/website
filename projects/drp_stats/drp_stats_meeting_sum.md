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

## Week 3

- This week's goal is to construct the prior for Bayesian Logistic Regression Model. 

- To keep it simple, we just use Bernoulli distribution with some sparsity $s\in [0,1]$ to generate non-diag entries of adjacency matrix, and Gaussian distribution to generate covariates.

- For now, we assume $\beta$ (sparsity) is fixed, neighbors of a node is known. Therefore the conditional distribution for individual node

$$\mathbb{P}(X_i \mid (X_j)_{j\neq i}, \mathbf{Z})=\frac{e^{X_i\mathbf{\theta}^T\mathbf{Z}_i+\beta X_i m_i(\mathbf{X})}}{e^{\mathbf{\theta}^T\mathbf{Z}_i+\beta m_i(\mathbf{X})} + e^{-\mathbf{\theta}^T\mathbf{Z}_i-\beta m_i(\mathbf{X})}}$$

has neighbor effects $\beta m_i$ becomes a fixed term. And this is a logistic-type conditional model Setup.

- After finishing constructing the prior and setup the basic model correctly, we can then consider neighbor effects.

## Week 6

$$\textcolor{red}{\text{QUESTIONS}
}$$

- If we want to induce graph sparsity and learn the network structure, do we need to replace the scalar dependence parameter $\beta$ with an edge-specific interaction matrix $B=(\beta_{ij})$? I remember in this paper $\beta$ is just a scalar
- Can I consider MLP to recover $\beta$ and $\theta$, and how does this connect to the posterior contraction rate.
- Sub-regression of L1 term, sparsity, and sign function with $\{-1, 1\}$ encoding.
- Lecture Question: is 1 a supremum of AUC instead of maximum? Since in hypothesis testing, alpha + beta > 0
- Lecture QUestion: do people simulate the ratio of its biased down, if so, we can just add that back to recover more unbiased estimate

