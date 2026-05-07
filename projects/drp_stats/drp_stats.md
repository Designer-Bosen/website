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
For each node in each iteration, $X_i$ is mapped from a probablistic function $\sigma$ using the score vector $S$ and $S_i=Z_i\theta + \beta m_i=\text{Individual Feature} + \beta \times \text{Neighbor Influence}$ where individual effect $k_i = Z_i \theta$ is determined by the node-specific features $Z_i$ with weights $\theta$; neighbor influence $\beta  m_i$ is computed from the current states of neighboring nodes; $m_i=\frac{1}{d_i}\sum_{j=1}^N A_{ij}X_j$ is the averaged neighbor influence with strength of peer effect $\beta$. Finally, the node specific conditional probability of the following form can be achieved through the modeling process:

$$\mathbb{P}(X_i = 1 \mid X_{-i}, \mathbf{Z})=\sigma(2(Z_i\theta + \beta m_i))$$

**Note:** there is a trade-off between individual features and neighbor influence, and the outcome depends on which effect is stronger. For example, If a node’s own features strongly favor 1, but neighbors are mostly -1, this means the node's own feature dominates. If a node has most of its neighbors being 1, the probability for itself being one increases.

#### Pseudo Code:

**STEP 1: Create:**
- Adjacency matrix $A \in \mathbb{R}^{N\times N}_\text{sym}$ (network frame)
- Covariate matrix $Z \in \mathbb{R}^{N\times p}$ (node features)
- Parameter vector $\theta \in \mathbb{R}^{p}$ (feature effect)
- Parameter scalar $\beta \in \mathbb{R}$ (peer effect)
- Number of iterations `num_iter`
- Burn-in length `burn_in`

**STEP 2: Initialize:** 
Randomly Initialize node states, e.g. $\mathbb{P}(X_i^{(0)} = +1)=p$, $\mathbb{P}(X_i^{(0)}=-1)=1-p$

$$
X^{(0)} = (X_1^{(0)},\dots,X_N^{(0)}) \in \{-1,+1\}^N
$$


**STEP 3: Gibbs Process:**  
For t = 1 to `num_iter`:  
For each node i:  
- Compute Neighbor Influence: $m_i^{(t)} = \sum_j A_{ij} X_j^{(t-1)}$  
- Compute Score: $S_i^{(t)} = \theta^\top Z_i + \beta m_i^{(t)}$
- Convert to Probability: $p_i^{(t)} = \text{sigmoid}(2S_i^{(t)})$ 
- Sample New State: 
$$
X_i^{(t)}=
\begin{cases}
+1, & \text{with probability } p_i^{(t)},\\
-1, & \text{with probability } 1-p_i^{(t)}.
\end{cases}
$$ 
- If t > `burn_in`: Record $X^{(t)}$ 


---

## Phase 2: Set up Simple Bayesian Logistic Regression Model

Without peer effects, $X_1, \dots, X_N$ given $Z_1, \dots, Z_N$ are conditional independent, The conditional probablity of $X_i$ given $\mathbf{Z}$ can be written as

$$\mathbb{P}(X_i \mid \mathbf{Z})=\frac{e^{X_i\mathbf{\theta}^T\mathbf{Z}_i}}{e^{\mathbf{\theta}^T\mathbf{Z}_i} + e^{-\mathbf{\theta}^T\mathbf{Z}_i}}$$

where $\sigma(x)=\frac{1}{1 + e^{-x}}$. The joint distribution (Likelihood) and log-likelihood function are

$$L(\theta; X, \mathbf{Z})
= \prod_{i=1}^N \frac{e^{X_i \theta^T \mathbf{Z}_i}}{e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}} 
= \frac{1}{\mathcal{Z}_N(\theta, \mathbf{Z})} \exp\left(\sum_{i=1}^N X_i (\theta^T \mathbf{Z}_i)\right)$$

$$\ell(\theta)= \log L(\theta; X, \mathbf{Z})= \sum_{i=1}^N \big[ X_i\theta^T \mathbf{Z}_i-\log(e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}) \big]$$

where $\mathcal{Z}_N(\theta, \mathbf{Z}) = \prod_{i=1}^N \big(e^{\theta^T \mathbf{Z}_i} + e^{-\theta^T \mathbf{Z}_i}\big)$ is the normalizing constant. A general choice is standard Gaussian prior is $\theta \sim N(\vec{0}, \tau^2I_p)$ for some $\tau > 0$. Then 

$$P(\theta)=\frac{1}{(2\pi \tau^2)^\frac{d}{2}}\exp(-\frac{1}{2\tau^2}\|\theta\|_2^2) \quad \text{and} \quad \log P(\theta)=-\frac{1}{2\tau^2}\|\theta\|_2^2+\text{const}$$

Finally, the objective function is given as the negative log-posterior such that 

$$
\begin{aligned}
-\log P(\theta \mid X, \mathbf{Z}) 
&\propto -\log (L(\theta; X, \mathbf{Z}) \cdot P(\theta) )\\
&= -\ell(\theta) - \log P(\theta) \\
&= -\sum_{i=1}^N \big[ X_i\theta^T \mathbf{Z}_i-\log(e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}) \big] +\frac{1}{2\tau^2}\|\theta\|_2^2+\text{const}
\end{aligned}
$$

and Maximum A Posteriori (MAP) is given as $\hat{\theta}_{MAP} = \arg\min_{\theta}-\log P(\theta \mid X, \mathbf{Z})$. Under standard assumptions, MAP estimator achieves rate: $\|\hat{\theta}_{MAP} - \theta\| = O\left(\frac{1}{\sqrt{N}}\right)$. In high dimensional setting, another induced condition is sparsity achieved by choosing priors $\theta_i \sim \text{Laplace}(\lambda)$, where $P(\theta)\propto \exp(-\lambda \|\theta\|_1)$. Then MAP estimator for L1-regularized logistic regression is given as

$$\hat\theta_{MAP}=\arg\min_\theta\{-\ell(\theta)+\lambda\|\theta\|_1\}$$


---

## Phase 3: Set up Bayesian Logistic Regression Model under Network Dependence

Let $S_i=\mathbf{\theta}^T\mathbf{Z}_i+\beta m_i(\mathbf{X})$ where $\theta \perp\!\!\!\perp \beta$, then the Ising Node-wise Conditional Probablity is 

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

$$-\ell(\theta, \beta)=-\sum_{i=1}^N \log \sigma(2X_iS_i)$$

### PRIOR CHOICE

**beta:**
- $\beta \sim N(0, \tau^2_\beta)$: Standard Gaussian Prior 

**sigma:**
- $\textbf{Laplace: }\theta_i \sim \text{Laplace}(\lambda)$: Turns into L1 penalty in MAP. Induces self-effect sparsity in high dimension
- $\textbf{Spike-and-Slab Prior: }\theta_i \sim (1-\pi)\delta_0+\pi N(0, \tau_\theta^2)$ where $\delta_0=1$ if $\theta_i=0$; $\delta_0=0$ otherwise: Encourages exact sparsity through a mixture of a point mass at zero and a Gaussian component.
- $\textbf{Horseshoe: }\theta_i \sim N(0, \lambda_i^2 \tau_\theta^2)$:  Strongly shrinks small coefficients toward zero while allowing large signals to remain less penalized.

---

## Phase 4: OPTIMIZATION

With $\beta \sim N(0, \tau^2_\beta)$ and $\theta_i \sim \text{Laplace}(\lambda)$, the MAP estimator is given as:

$$(\hat\theta, \hat\beta)_{MAP}=\arg\min_{\theta, \beta} \big\{ \mathcal{L(\theta, \beta)}\big\}$$

where

$$\mathcal{L(\theta, \beta)}=-\ell(\theta, \beta) + \lambda\|\theta\|_1 + \frac{\beta^2}{2\tau_\beta^2}$$

Then, the gradient and subgradient are given as 

$$\nabla_{\beta}\mathcal{L(\theta, \beta)}=-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)m_i(X)+\frac{\beta}{\tau_\beta^2}$$

$$\partial_\theta \mathcal{L(\theta, \beta)}=-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)Z_i+\lambda\partial \|\theta\|_1$$

where

$$
\partial \|\theta\|_1 =
\left\{
s \in \mathbb{R}^p :
\begin{cases}
s_j = \mathrm{sign}(\theta_j) & \text{if } \theta_j \neq 0 \\
s_j \in [-1,1] & \text{if } \theta_j = 0
\end{cases}
\right\}
$$





---

## Phase 5: Theory

**Hessian Matrix**

**Bias Analysis**

---

## Informal Reference Section

Bayesian Data Analysis Third Edition, Gelman

High-Dimensional Bayesian Regularized Regression with the bayesreg Package (arXiv:1611.06649)

https://johnwlambert.github.io/subgradient-methods/

https://www.bayesrulesbook.com/chapter-13

https://www2.stat.duke.edu/~banks/521-lectures.dir/lect10.pdf