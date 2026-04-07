[← Back](../index.html)

# Stats 199 - Directed Research
**Mentored by Professor Michailadis**: 
[Anchor paper](https://jmlr.org/papers/volume25/22-1040/22-1040.pdf)

**Tentative Research Proposal**: 
[Research Proposal](index.html?file=drp_stats_proposal.md)


---
## Anchor Paper Ideas:

### Logistic Regression:

Given predictor variables: $\mathbf{Z}_1, \dots, \mathbf{Z}_N \in \mathbb{R}^d$ and independent response variables: $X_1, \dots, X_N \in \{-1,1\}$, the logistic regression model for hte probability of a positive outcome is given by:

$$\mathbb{P}(X_i=1 \mid \mathbf{Z}_i)=\frac{e^{\mathbf{\theta}^T \mathbf{Z}_i}}{e^{\mathbf{\theta}^T \mathbf{Z}_i}+e^{-\mathbf{\theta}^T \mathbf{Z}_i}}$$

for $1 \leq i \leq N$ and $\mathbf{\theta}=(\theta_1 ,\dots, \theta_d)^T \in \mathbb{R}^d$. Assuming conditional independence of $X_i$ given $\mathbf{Z}_i$, the joint distribution can be written as:

$$\mathbb{P}(X \mid \mathbf{Z})=\prod_{i=1}^N \frac{e^{X_i \theta^T \mathbf{Z}_i}}{e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}}=\frac{1}{\mathcal{Z}_N(\theta, \mathbf{Z})} \exp{(\sum_{i=1}^N X_i (\theta^T \mathbf{Z}_i))}$$

where $\mathcal{Z}_N(\theta, \mathbf{Z})=\prod_{i=1}^N{\frac{1}{e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}}}$ is the normalizing constant.

**Property:** Under standard assumptions (e.g. fixed dimension), the maximum likelihood estimator (MLE) achieves rate: $\|\hat{\theta}_{MLE} - \theta\| = O\left(\frac{1}{\sqrt{N}}\right)$

### Gradient and Hessian:

Log likelihood function for logistic regression is given by:

$$\ell(\theta)=\log L(\theta \mid X, \mathbf{Z})=\log \prod_{i=1}^N{ \mathbb{P}(X_i \mid \mathbf{Z}_i)}$$

with the assumption $X_1, \dots, X_N$ ​are conditionally independent given $\mathbf{Z}_1, \dots, \mathbf{Z}_N$. If we want to optimize $\ell(\theta)$, we need to consider its Gradient and Hessian, which are given by:

$$\nabla \ell(\theta)=\sum_{i=1}^N \mathbf{Z}_i (X_i-\tanh(\theta^T \mathbf{Z}_i)) \quad \text{and} \quad \nabla^2 \ell(\theta)=-\sum_{i=1}^N \mathbf{Z}_i \mathbf{Z}_i^T \cdot \text{sech}^2(\theta^T\mathbf{Z}_i)$$

However, in the presence of network dependence, the assumption that $X_1, \dots, X_N$ are conditionally independent given $\mathbf{Z}_1, \dots, \mathbf{Z}_N$ no longer holds. For example, Ising-type logistic models is

$$P(X \mid \mathbf{Z}, \theta) = \frac{1}{Z(\theta)} \exp{(\sum_{i=1}^N X_i \theta^T \mathbf{Z}_i+\sum_{i,j} A_{ij} X_i X_j)}$$

So the log-likelihood is:

$$\ell(\theta)=\sum_{i=1}^N X_i \theta^T \mathbf{Z}_i+\sum_{i,j} A_{ij} X_i X_j-\log Z(\theta)$$

For Large $N$, the presence of the partition function makes it extreme computational expensive to compute the sum of over $2^N$ configurations. In addition, gradient formulation requires taking difference of observed and implied sufficient statistics, but expectation of sufficient statistics involves full-joint distribution.

### PMPL Estimator

**Pseudo-Likelihood:** 

The conditional distribution of $X_i$ given $(X_j)_{j\neq i}$ is:

$$\mathbb{P}(X_i \mid (X_j)_{j\neq i}, \mathbf{Z})=\frac{e^{X_i\mathbf{\theta}^T\mathbf{Z}_i+\beta X_i m_i(\mathbf{X})}}{e^{\mathbf{\theta}^T\mathbf{Z}_i+\beta m_i(\mathbf{X})} + e^{-\mathbf{\theta}^T\mathbf{Z}_i-\beta m_i(\mathbf{X})}}$$

The negative Log Pseudo-Likelihood (LPL) is given as 

$$L_N(\beta,\mathbf{\theta})=-\frac{1}{N}\sum_{i=1}^N \log \mathbb{P}(X_i \mid (X_j)_{j\neq i}, \mathbf{Z})=-\frac{1}{N}\sum_{i=1}^N \{X_i(\mathbf{\theta}^T\mathbf{Z}_i+\beta m_i(\mathbf{X})) - \log \cosh(\mathbf{\theta}^T\mathbf{Z}_i+\beta m_i(\mathbf{X}))\} + \log(2)$$

The proposed Penalized Maximum Pseudo-likelihood (PMPL) in the paper is given as the estimator of $(\beta, \mathbf{\theta}^T)$ with a L1 regularization (tuning) parameter $\lambda$ as follows:

$$(\hat{\beta}, \hat{\mathbf{\theta}}^T):=\text{arg}\min_{(\beta,\mathbf{\theta})}=\{L_N(\beta,\mathbf{\theta})+\lambda\|\theta\|_1\}$$

under various regularization assumptions, Theorem 1 states if $\lambda$ is chosen proportional to $\sqrt{\log(\frac{d}{n})}$



### Weakly Dependent Data and Dobrushin’s Condition

### Restricted Strong Convexity (RSC)

### Main Contribution of the Anchored Paper

This motivates the question of whether similar gradient concentration and curvature properties (e.g., restricted strong convexity) can still be established under weak dependence, particularly when using pseudo-likelihood or Bayesian formulations.


---
## My Direction:

Given above results from the anchor paper, I want to formulate a Bayesian logistic regression model under network dependence. Then analyze its properties under this regime e.g. posterior contraction rates, consistency, computational approaches such as MCMC.

Keywords: High-dimensional Bayesian Inference, restricted strong convexity

### Prior Knowledge on Bayesian Inference:

**Bayesian Theorem:** $\mathbb{P}(A \mid B)=\frac{\mathbb{P}(B \mid A)\cdot \mathbb{P}(A)}{\mathbb{P}(B)}$

From Bayesian perspective, parameter $\theta$ is treated as a random variable and follows some distribution. If $Y$ represents the data, the probability density function of $\theta$ give $Y$ can be represented as:

$$f(\theta \mid Y) = \frac{f(Y \mid \theta) \cdot f(\theta)}{f(Y)} = \frac{\text{Likelihood} \cdot \text{Prior}}{\text{Normalizing Constant}} \propto \text{Likelihood} \cdot \text{Prior}$$

**Bayesian Logistic Regression Model:**



## Informal Reference

Bayesian Data Analysis Third Edition, Gelman

High-Dimensional Bayesian Regularized Regression with the bayesreg Package (arXiv:1611.06649)


---