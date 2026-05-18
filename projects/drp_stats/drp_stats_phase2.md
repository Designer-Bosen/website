[← Back](index.html)

# Phase 2: Bayesian Logistic Regression Model Setup

## Simple Bayesian Logistic Regression

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

## Bayesian Logistic Regression with Network Dependence

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