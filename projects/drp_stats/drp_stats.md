[← Back](../index.html)

# Stats 199 - Directed Research

---

## Navigation Links

[Tentative Research Proposal](index.html?file=drp_stats_proposal.md)

[Anchor Paper Ideas](index.html?file=drp_stats_anchor_paper_ideas.md)

[Weekly Meeting Summary](index.html?file=drp_stats_meeting_sum.md)

---

## Project Description

This **Stats 199 - Directed Research** is a 1-quarter contract course at UCLA Statistics and Data Science conducted under the supervision of Professor George Michailidis. This individual directed research is built upon on the recent work [High Dimensional Logistic Regression Under Network Dependence](https://jmlr.org/papers/volume25/22-1040/22-1040.pdf) which addresses the presence of network dependence in observations via introducing Ising-type network interaction structure into the logistic regression model. The paper proosed a penalized maximum pseudo-likelihood (PMPL) approach to overcome computational burden of full likelihood. This approach enables efficient estimation in high-dimensional setting while achieving descent computational rate and approximation accuracy. In particular, under sparsity and weak dependence conditions, PMPL estimator achieves consistency comparable to classical logistic regression with independent data.

Motivated by my research interest in high dimensional Bayesian inference...



---
## My Direction

Given the above results from the anchor paper, I aims to conduct a simulation study of a logistic regression model under network dependence in Bayesian perspective. The goal is to empirically evaluate its performance under this regime, including posterior contraction behavior, estimation accuracy, and computational efficiency of inference methods such as MCMC. **Keywords**: High-dimensional Bayesian Inference, restricted strong convexity

**Bayesian Theorem:** $\mathbb{P}(A \mid B)=\frac{\mathbb{P}(B \mid A)\cdot \mathbb{P}(A)}{\mathbb{P}(B)}$

From Bayesian perspective, parameter $\theta$ is treated as a random variable and follows some distribution. If $Y$ represents the outcome data, the probability density function of $\theta$ give $Y$ can be represented as:

$$f(\theta \mid Y) = \frac{f(Y \mid \theta) \cdot f(\theta)}{f(Y)} = \frac{\text{Likelihood} \cdot \text{Prior}}{\text{Normalizing Constant}} \propto \text{Likelihood} \cdot \text{Prior}$$

---

## Phase 1: Model the network in Python

**Adjacency Matrix** $A \in \mathbb{R}^{N\times N}_\text{sym}$ represent the network struture. If node $i$ is connected to node $j$, then $A_{ij} = A_{ji} = 1$. Otherwise $A_{ij} = A_{ji} = 0$. Each non-diagonal entry is generated using a Bernoulli distribution with tunable parameter $s$ controlling sparsity.

**Covariate Matrix** $Z \in \mathbb{R}^{N\times p}$ represent the collection of node-specific features, where $Z_i$ corresponds to the covariate vector of node $i$. $Z$ captures the individual effects, and entries of Z are generated independent from a chosen distribution.

**Outcome Vector** $X \in \{-1,1\}^{N}$ where $X_i$ is generated through a iterative updating process inspired by Gibbs samppling.
For each node in each iteration, $X_i$ is sampled from a conditional probability distribution $\sigma$ using the score vector $S$ and $S_i=\theta^T\mathbf{Z}_i + \beta m_i=\text{Individual Feature} + \beta \times \text{Neighbor Influence}$ where individual effect $k_i = Z_i \theta$ is determined by the node-specific features $Z_i$ with weights $\theta$; neighbor influence $\beta  m_i$ is computed from the current states of neighboring nodes; $m_i=\sum_{j=1}^N A_{ij}X_j$ is the total neighbor influence with strength of peer effect $\beta$. Finally, the node specific conditional probability of the following form can be achieved through the modeling process:

$$\mathbb{P}(X_i = 1 \mid X_{-i}, \mathbf{Z})=\sigma(2(\theta^T\mathbf{Z}_i + \beta m_i))$$



### PSEUDO CODE:

**STEP 1: Create**
- Adjacency matrix $A \in \mathbb{R}^{N\times N}_\text{sym}$ (network frame)
- Covariate matrix $Z \in \mathbb{R}^{N\times p}$ (node features)
- Parameter vector $\theta \in \mathbb{R}^{p}$ (feature effect)
- Parameter scalar $\beta \geq 0$ (peer effect)
- Number of iterations (burn-in) `num_iter`

**STEP 2: Initialize** 
Randomly Initialize node states, e.g. $\mathbb{P}(X_i^{(0)} = +1)=p$, $\mathbb{P}(X_i^{(0)}=-1)=1-p$

$$
X^{(0)} = (X_1^{(0)},\dots,X_N^{(0)}) \in \{-1,+1\}^N
$$


**STEP 3: Gibbs Process**  
For t = 1 to `num_iter`:  
For each node i:  
- Compute Score: $S_i^{(t)} = \theta^T Z_i + \beta \sum_j A_{ij} X_j^{(t-1)}$
- Convert to Probability: $p_i^{(t)} = \text{sigmoid}(2S_i^{(t)})$ 
- Sample New State: 
$$
X_i^{(t)}=
\begin{cases}
+1, & \text{with probability } p_i^{(t)},\\
-1, & \text{with probability } 1-p_i^{(t)}.
\end{cases}
$$ 

Return  $X^{(t)}$, $A$, $Z$


---

## Phase 2: Set up Simple Bayesian Logistic Regression Model

Without peer effects, $X_1, \dots, X_N$ given $Z_1, \dots, Z_N$ are conditional independent, The conditional probablity of $X_i$ given $\mathbf{Z}$ can be written as

$$\mathbb{P}(X_i \mid \mathbf{Z})=\frac{e^{X_i\mathbf{\theta}^T\mathbf{Z}_i}}{e^{\mathbf{\theta}^T\mathbf{Z}_i} + e^{-\mathbf{\theta}^T\mathbf{Z}_i}}$$

where $\sigma(x)=\frac{1}{1 + e^{-x}}$. The joint distribution (Likelihood) and log-likelihood function are

$$L(\theta^T; X, \mathbf{Z})
= \prod_{i=1}^N \frac{e^{X_i \theta^T \mathbf{Z}_i}}{e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}} 
= \frac{1}{\mathcal{Z}_N(\theta^T, \mathbf{Z})} \exp\left(\sum_{i=1}^N X_i (\theta^T \mathbf{Z}_i)\right)$$

$$\ell(\theta^T)= \log L(\theta^T; X, \mathbf{Z})= \sum_{i=1}^N \big[ X_i\theta^T \mathbf{Z}_i-\log(e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}) \big]$$

where $\mathcal{Z}_N(\theta^T, \mathbf{Z}) = \prod_{i=1}^N \big(e^{\theta^T \mathbf{Z}_i} + e^{-\theta^T \mathbf{Z}_i}\big)$ is the normalizing constant. A general choice is standard Gaussian prior is $\theta^T \sim N(\vec{0}, \tau^2I_p)$ for some $\tau > 0$. Then 

$$P(\theta^T)=\frac{1}{(2\pi \tau^2)^\frac{d}{2}}\exp(-\frac{1}{2\tau^2}\|\theta^T\|_2^2) \quad \text{and} \quad \log P(\theta^T)=-\frac{1}{2\tau^2}\|\theta^T\|_2^2+\text{const}$$

Finally, the objective function is given as the negative log-posterior such that 

$$
\begin{aligned}
-\log P(\theta^T \mid X, \mathbf{Z}) 
&\propto -\log (L(\theta^T; X, \mathbf{Z}) \cdot P(\theta^T) )\\
&= -\ell(\theta^T) - \log P(\theta^T) \\
&= -\sum_{i=1}^N \big[ X_i\theta^T \mathbf{Z}_i-\log(e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}) \big] +\frac{1}{2\tau^2}\|\theta^T\|_2^2+\text{const}
\end{aligned}
$$

and Maximum A Posteriori (MAP) is given as $\hat{\theta^T}_{MAP} = \arg\min_{\theta^T}-\log P(\theta^T \mid X, \mathbf{Z})$. Under standard assumptions, MAP estimator achieves rate: $\|\hat{\theta^T}_{MAP} - \theta^T\| = O\left(\frac{1}{\sqrt{N}}\right)$. In high dimensional setting, another induced condition is sparsity achieved by choosing priors $\theta^T_i \sim \text{Laplace}(\lambda)$, where $P(\theta^T)\propto \exp(-\lambda \|\theta^T\|_1)$. Then MAP estimator for L1-regularized logistic regression is given as

$$\hat\theta^T_{MAP}=\arg\min_{\theta^T}\{-\ell(\theta^T)+\lambda\|\theta^T\|_1\}$$


---

## Phase 3: Set up Bayesian Logistic Regression Model under Network Dependence

Let $S_i=\theta^T\mathbf{Z}_i+\beta m_i(X)$ where $\theta^T \perp\!\!\!\perp \beta$, then the Ising Node-wise Conditional Probablity is 

$$\mathbb{P}(X_i \mid (X_j)_{j\neq i}, \mathbf{Z})=\frac{e^{X_iS_i}}{e^{S_i} + e^{-S_i}}$$

Since $X_i \in \{-1,1\}$, this can be equivalently written in logistic form as

$$
\mathbb{P}(X_i \mid \cdot) = \sigma(2X_iS_i)=
\begin{cases}
\displaystyle \frac{1}{1 + e^{-2S_i}}, & \text{if } X_i = +1 \\
\displaystyle \frac{1}{1 + e^{2S_i}}, & \text{if } X_i = -1
\end{cases}
$$

Its negative pseudo log-likelihood is given as

$$-\ell(\theta^T, \beta)=-\sum_{i=1}^N \log \sigma(2X_iS_i)$$

### PRIOR CHOICE

**beta:**
- $\beta \sim N(0, \tau^2_\beta)$: Standard Gaussian Prior 

**sigma:**
- $\textbf{Laplace: }\theta^T_i \sim \text{Laplace}(\lambda)$: Turns into L1 penalty in MAP. Induces self-effect sparsity in high dimension
- $\textbf{Spike-and-Slab Prior: }\theta^T_i \sim (1-\pi)\delta_0+\pi N(0, \tau_{\theta^T}^2)$ where $\delta_0=1$ if $\theta^T_i=0$; $\delta_0=0$ otherwise: Encourages exact sparsity through a mixture of a point mass at zero and a Gaussian component.
- $\textbf{Horseshoe: }\theta^T_i \sim N(0, \lambda_i^2 \tau_{\theta^T}^2)$:  Strongly shrinks small coefficients toward zero while allowing large signals to remain less penalized.

---

## Phase 4: OPTIMIZATION

With $\beta \sim N(0, \tau^2_\beta)$ and $\theta^T_i \sim \text{Laplace}(\lambda)$, the MAP estimator is given as:

$$(\hat\theta^T, \hat\beta)_{MAP}=\arg\min_{\theta^T, \beta} \big\{ \mathcal{L}(\theta^T, \beta)\big\}$$

where

$$\mathcal{L}(\theta^T, \beta)=-\ell(\theta^T, \beta) + \lambda\|\theta^T\|_1 + \frac{\beta^2}{2\tau_\beta^2}$$

### GRADIENT (smooth-terms)

$$\nabla_{\beta}\mathcal{L}(\theta^T, \beta)=-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)m_i(X)+\frac{\beta}{\tau_\beta^2}$$

$$\nabla_{\theta^T} \big\{-\ell(\theta^T, \beta) \big\}=
-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)\mathbf{Z}_i$$

### HESSIAN MATRIX

$$\partial_{\theta^T}^2\mathcal{L}(\theta^T, \beta)=4\sum_{i=1}^N w_i \mathbf{Z}_i\mathbf{Z}_i^T$$

$$\nabla_{\beta}^2\mathcal{L}(\theta^T, \beta)=4\sum_{i=1}^N w_i m_i(X)^2+\frac{1}{\tau_\beta^2}$$

$$\partial_{\theta^T} \nabla_\beta\mathcal{L(\theta^T, \beta)}= 4\sum_{i=1}^N w_i \mathbf{Z}_im_i(X)$$

where $w_i=\sigma(2X_iS_i) \sigma(-2X_iS_i)$. The Hessian matrix $H$ for smooth points (i.e. $\theta^T_j\neq 0$), the gradient is given as 

$$
\begin{aligned}
H&=
\begin{pmatrix}
4\sum_{i=1}^N w_i \mathbf{Z}_i\mathbf{Z}_i^T & 4\sum_{i=1}^N w_i \mathbf{Z}_im_i(X) \\
4\sum_{i=1}^N w_i m_i(X)Z_i^T & 4\sum_{i=1}^N w_i m_i(X)^2+\frac{1}{\tau_\beta^2} 
\end{pmatrix} \\
&= 4\sum_{i=1}^N w_i u_iu_i^T + 
\begin{pmatrix}
0 & 0 \\
0 & \frac{1}{\tau_\beta^2}
\end{pmatrix}
\quad
\text{where } u_i=
\begin{pmatrix}
\mathbf{Z}_i \\
m_i(X)
\end{pmatrix}
\end{aligned}
$$

**$H$ is positive definite:** since $\sigma(x)>0$, then $w_i>0$. For any vector non-trivial $v\in \mathbb{R}^{p+1}$, $v^THv=4\sum_{i=1}^N w_i v^Tu_iu_i^Tv + \frac{v_{p+1}^2}{\tau_\beta^2}=4\sum_{i=1}^N w_i (u_i^Tv)^2 + \frac{v_{p+1}^2}{\tau_\beta^2}\geq 0$. Thus $H \succeq 0$. 

Therefore, the objective function $\mathcal{L}(\theta^T, \beta)$ is convex.

### OPTIMIZATION PSEUDO CODE

**STEP 1: Define Parameters**
- $\lambda$: Laplace prior penalty coefficient
- $\tau^2$: Gaussian prior variance
- `tol`: Stopping tolerance
- `shrink`: Backtracking step size shrinkage factor
- `eps`: Backtracking decreasing factor
- `step_beta_0`: GD w.r.t $\beta$ initial step size
- `step_theta_0`: GD w.r.t $\theta^T$ initial step size

**STEP 2: Define Functions**
- `fcn(theta, beta)`: $\mathcal{L}(\theta^T, \beta)=-\ell(\theta^T, \beta) + \lambda\|\theta^T\|_1 + \frac{\beta^2}{2\tau_\beta^2}$
- `grad_beta(theta, beta)`: $\nabla_{\beta}\mathcal{L}(\theta^T, \beta)=-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)m_i(X)+\frac{\beta}{\tau_\beta^2}$
- `grad_theta(theta, beta)`: $\nabla_{\theta^T} \big\{-\ell(\theta^T, \beta) \big\}=-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)\mathbf{Z}_i$

**STEP 3: Setups**
- Get dimensions: `N, p = Z.shape`
- Initialize `beta_old = 0`, `theta_old = np.zeros(p)`, 
- Initialize `beta_new = beta_old`, `theta_new = theta_old`

**STEP 4 Optimize**

Input $X$, $A$, $Z$

while True:
- `g_beta = grad_beta(theta_old, beta_old)`
- `g_theta = grad_beta(theta_old, beta_old)`
- `f_old = fcn(theta_old, beta_old)`
- 


---

## Phase 5: Theory

**Bias Analysis**



---

## Informal Reference Section

Bayesian Data Analysis Third Edition, Gelman

High-Dimensional Bayesian Regularized Regression with the bayesreg Package (arXiv:1611.06649)

https://johnwlambert.github.io/subgradient-methods/

https://www.bayesrulesbook.com/chapter-13

https://www2.stat.duke.edu/~banks/521-lectures.dir/lect10.pdf