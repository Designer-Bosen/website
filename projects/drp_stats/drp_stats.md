[← Back](../index.html)

# Stats 199 - Directed Research

---

## Navigation Links

[Tentative Research Proposal](index.html?file=drp_stats_proposal.md)

[Anchor Paper Ideas](index.html?file=drp_stats_anchor_paper_ideas.md)


---

## Project Description

**Stats 199 - Directed Research** irected Research is a 1-quarter contract course at UCLA Statistics and Data Science conducted under the supervision of Professor George Michailidis. This individual directed research is built upon the recent work [High Dimensional Logistic Regression Under Network Dependence](https://jmlr.org/papers/volume25/22-1040/22-1040.pdf) [1] which addresses the presence of network dependence in observations via introducing Ising-typenetwork interaction structure into the logistic regression model. The paper proposed a penalized maximum pseudo-likelihood (PMPL) approach to over-come computational burden of full likelihood. This approach enables efficient estimation in high-dimensional setting while achieving desirable computational performance and approximation accuracy. In particular, under sparsity and weak dependence conditions, PMPL estimator achieves consistency comparable to classical logistic regression with independent data.

Motivated by interest in high-dimensional Bayesian modeling, this project aims to explore Bayesian approaches for logistic regression models under network dependence. The project is primarily simulation-based, focusing on the empirical behavior of Bayesian estimators under different network structures and sparsity regimes. In particular, a simulation study is conducted to investigate the impact of various prior specifications and hyperparameter choices on parameter recovery. Synthetic datasets were generated through Monte Carlo experiments under different levels of network dependence, sparsity conditions, and dimensionality settings. [2] [3] In addition, the project examined optimization and convexity properties of the corresponding objective functions, as well as the influence of different network construction mechanisms on estimation performance. The proposed Bayesian methods were compared against the PMPL approach developed in the original work. Performance was primarily assessed through parameter estimation accuracy and precision.

---
## Bayesian Perspective

**Bayesian Theorem:** $\mathbb{P}(A \mid B)=\frac{\mathbb{P}(B \mid A)\cdot \mathbb{P}(A)}{\mathbb{P}(B)}$

From Bayesian perspective, parameter $\theta$ is treated as a random variable and follows some distribution. If $X$ represents the outcome data, the probability density function of $\theta$ give $X$ can be represented as:

$$f(\theta \mid Y) = \frac{f(X \mid \theta) \cdot f(\theta)}{f(X)} \propto \text{Likelihood} \cdot \text{Prior}$$


---
## Modeling

### **Network Structure**

**Adjacency Matrix** $A \in \mathbb{R}^{N\times N}_\text{sym}$ represent the network structure. If node $i$ is connected to node $j$, then $A_{ij} = A_{ji} = 1$. Otherwise $A_{ij} = A_{ji} = 0$. Each non-diagonal entry is generated using a Bernoulli distribution with tunable parameter $s$ controlling sparsity.

**Covariate Matrix** $Z \in \mathbb{R}^{N\times p}$ represent the collection of node-specific features, where $Z_i$ corresponds to the covariate vector of node $i$. $Z$ captures the individual effects, and entries of Z are generated independent from a chosen distribution.

**Outcome Vector** $X \in \{-1,1\}^{N}$ where $X_i$ is generated through a iterative updating process inspired by Gibbs sampling.[4] [5] For each node in each iteration, $X_i$ is sampled from a conditional probability distribution $\sigma$ using the score vector $S$ and $S_i=\theta^T\mathbf{Z}_i + \beta m_i=\text{Individual Feature} + \beta \times \text{Neighbor Influence}$ where individual effect $k_i = Z_i \theta$ is determined by the node-specific features $Z_i$ with weights $\theta$; neighbor influence $\beta  m_i$ is computed from the current states of neighboring nodes; $m_i=\sum_{j=1}^N A_{ij}X_j$ is the total neighbor influence with strength of peer effect $\beta$. Finally, the node specific conditional probability of the following form can be achieved through the modeling process:

$$\mathbb{P}(X_i = 1 \mid X_{-i}, \mathbf{Z})=\sigma(2(\theta^T\mathbf{Z}_i + \beta m_i))$$


### Bayesian Logistic Regression Model

Without peer effects, $X_1, \dots, X_N$ given $Z_1, \dots, Z_N$ are conditional independent, the conditional probability of $X_i$ given $\mathbf{Z}$ can be written as

$$\mathbb{P}(X_i \mid \mathbf{Z})=\frac{e^{X_i\mathbf{\theta}^T\mathbf{Z}_i}}{e^{\mathbf{\theta}^T\mathbf{Z}_i} + e^{-\mathbf{\theta}^T\mathbf{Z}_i}}$$

where $\sigma(x)=\frac{1}{1 + e^{-x}}$. [6] [7] The joint distribution (Likelihood) and log-likelihood function are

$$L(\theta^T; X, \mathbf{Z})
= \prod_{i=1}^N \frac{e^{X_i \theta^T \mathbf{Z}_i}}{e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}} 
= \frac{1}{\mathcal{Z}_N(\theta^T, \mathbf{Z})} \exp\left(\sum_{i=1}^N X_i (\theta^T \mathbf{Z}_i)\right)$$

$$\ell(\theta^T)= \log L(\theta^T; X, \mathbf{Z})= \sum_{i=1}^N \big[ X_i\theta^T \mathbf{Z}_i-\log(e^{\theta^T \mathbf{Z}_i}+e^{-\theta^T \mathbf{Z}_i}) \big]$$

where $\mathcal{Z}_N(\theta^T, \mathbf{Z}) = \prod_{i=1}^N \big(e^{\theta^T \mathbf{Z}_i} + e^{-\theta^T \mathbf{Z}_i}\big)$ is the normalizing constant. In a high dimensional setting, an induced condition is sparsity achieved by choosing prior $\theta^T_i \sim \text{Laplace}(\lambda)$, where $P(\theta^T)\propto \exp(-\lambda \|\theta^T\|_1)$. Then the negative log-posterior and Maximum A Posteriori (MAP), which is equivalent to $\ell1$-regularized form given as the following 

$$-\log P(\theta^T \mid X, \mathbf{Z}) 
\propto -\log (L(\theta^T; X, \mathbf{Z}) \cdot P(\theta^T) )= -\ell(\theta^T) - \log P(\theta^T)$$

$$\hat\theta^T_{MAP}=\arg\min_{\theta^T}\{-\ell(\theta^T)+\lambda\|\theta^T\|_1\}$$

Under standard assumptions, MAP estimator achieves: $\|\hat{\theta^T}_{MAP} - \theta^T\| = O\left(\frac{1}{\sqrt{N}}\right)$.


### Bayesian Logistic Regression Model with Network Dependence

Let $S_i=\theta^T\mathbf{Z}_i+\beta m_i(X)$ where $\theta^T \perp\!\!\!\perp \beta$, then the Ising Node-wise Conditional Probability is 

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

### Gaussian and Laplace Prior

With Prior choices $\beta \sim N(0, \tau^2)$ and $\theta^T_i \sim \text{Laplace}(\lambda)$, [8] the MAP estimator is given as:

$$
\begin{aligned}
(\hat\theta^T, \hat\beta)_{MAP}&=\arg\min_{\theta^T, \beta} \big\{ \mathcal{L}(\theta^T, \beta)\big\} \\
&=\arg\min_{\theta^T, \beta} \big\{ -\ell(\theta^T, \beta) + \lambda\|\theta^T\|_1 + \frac{\beta^2}{2\tau^2} \big\} 
\end{aligned}
$$

To avoid gradient disappearance, take the negative pseudo log-likelihood to be the averaged form such that:

$$-\ell(\theta^T, \beta)=-\frac1N\sum_{i=1}^N \log \sigma(2X_iS_i)$$

 #### GRADIENT (smooth-terms)

$$\nabla_{\beta}\mathcal{L}(\theta^T, \beta)=-\frac2N\sum_{i=1}^N X_i \sigma(-2X_iS_i)m_i(X)+\frac{\beta}{\tau^2}$$

$$\nabla_{\theta^T} \mathcal{L}_{smooth}= -\frac2N\sum_{i=1}^N X_i \sigma(-2X_iS_i)\mathbf{Z}_i$$

#### HESSIAN MATRIX (smooth-terms)

The Hessian matrix $H$ for smooth points (i.e. $\theta^T_j\neq 0$), the gradient is given as 

$$
\begin{aligned}
H&=
\begin{pmatrix}
\nabla^2_{\theta^T} \mathcal{L}_{smooth} & \nabla_{\beta}\nabla_{\theta^T} \mathcal{L}_{smooth} \\
\nabla_{\theta^T}\nabla_{\beta} \mathcal{L}_{smooth} & \nabla_{\beta}^2\mathcal{L}(\theta^T, \beta)
\end{pmatrix} \\
&=
\begin{pmatrix}
\frac4N\sum_{i=1}^N w_i \mathbf{Z}_i\mathbf{Z}_i^T & \frac4N\sum_{i=1}^N w_i \mathbf{Z}_im_i(X) \\
\frac4N\sum_{i=1}^N w_i m_i(X)\mathbf{Z}_i^T & \frac4N\sum_{i=1}^N w_i m_i(X)^2+\frac{1}{\tau^2} 
\end{pmatrix} \\
&= \frac4N\sum_{i=1}^N w_i u_iu_i^T + 
\begin{pmatrix}
0 & 0 \\
0 & \frac{1}{\tau^2}
\end{pmatrix}
\end{aligned}
$$

where $u_i=\begin{pmatrix} \mathbf{Z}_i & m_i(X)\end{pmatrix}^T$ and $w_i=\sigma(2X_iS_i) \sigma(-2X_iS_i)$. Since $\sigma(x)>0$, then $w_i>0$. For any non-trivial vector $v\in \mathbb{R}^{p+1}$, $v^THv=4\sum_{i=1}^N w_i v^Tu_iu_i^Tv + \frac{v_{p+1}^2}{\tau^2}=4\sum_{i=1}^N w_i (u_i^Tv)^2 + \frac{v_{p+1}^2}{\tau^2}\geq 0$. Thus $H \succeq 0$. Therefore, the objective function $\mathcal{L}(\theta^T, \beta)$ is convex.

### Gaussian and Elastic Net Prior

With Prior choices $\beta \sim N(0, \tau^2)$ and $\theta^T_i \propto \lambda\big[\alpha\|\theta^T\|_1+\frac{1-\alpha}{2}\|\theta^T\|_2^2\big]$, \cite{elastic_net} the MAP estimator is given as:

$$
\begin{aligned}
(\hat\theta^T, \hat\beta)_{MAP}&=\arg\min_{\theta^T, \beta} \big\{ \mathcal{L}(\theta^T, \beta)\big\} \\
&=\arg\min_{\theta^T, \beta} \big\{ -\ell(\theta^T, \beta) + \lambda\big[\alpha\|\theta^T\|_1+\frac{1-\alpha}{2}\|\theta^T\|_2^2\big] + \frac{\beta^2}{2\tau^2} \big\} 
\end{aligned}
$$

#### GRADIENT (smooth-terms)

$$\nabla_{\beta}\mathcal{L}(\theta^T,\beta)
=-\frac{2}{N}\sum_{i=1}^{N} X_i\sigma(-2X_iS_i)m_i(X) +\frac{\beta}{\tau^2}$$

$$\nabla_{\theta^T}\mathcal{L}_{smooth}(\theta^T,\beta) =-\frac{2}{N}\sum_{i=1}^{N} X_i\sigma(-2X_iS_i)\mathbf Z_i+\lambda(1-\alpha)\theta^T$$


#### HESSIAN MATRIX (smooth-terms)

The Hessian matrix $H$ for smooth points (i.e. $\theta^T_j\neq 0$), is given by

$$
\begin{aligned}
H&=
\begin{pmatrix}
\nabla^2_{\theta^T}\mathcal L_{smooth}
&\nabla_{\beta}\nabla_{\theta^T}\mathcal L_{smooth}\\
\nabla_{\theta^T}\nabla_{\beta}\mathcal L_{smooth}
&\nabla_{\beta}^2\mathcal{L}(\theta^T, \beta)
\end{pmatrix}\\
&=
\begin{pmatrix}
\frac4N\sum_{i=1}^N w_i \mathbf Z_i\mathbf Z_i^T
+\lambda(1-\alpha)I_p
&\frac4N\sum_{i=1}^N w_i \mathbf Z_i m_i(X)\\
\frac4N\sum_{i=1}^N w_i m_i(X)\mathbf Z_i^T
&\frac4N\sum_{i=1}^N w_i m_i(X)^2
+\frac1{\tau^2}
\end{pmatrix}\\
&=
\frac4N\sum_{i=1}^N w_i u_i u_i^T+
\begin{pmatrix}
\lambda(1-\alpha)I_p & 0\\
0 & \frac1{\tau^2}
\end{pmatrix},
\end{aligned}
$$

where $u_i=\begin{pmatrix} \mathbf{Z}_i & m_i(X)\end{pmatrix}^T$ and $w_i=\sigma(2X_iS_i) \sigma(-2X_iS_i)$. Similarly to the previous part, $H \succeq 0$ implying $\mathcal L(\theta^T,\beta)$ is convex.

---

## Simulation Study

### Experiment Flow

- $N$: Number of observations.
- $p$: Number of covariates.
- $T$: Number of Monte Carlo experiments.
- $s$: Self-sparsity level; proportion of non-zero entries in $\theta$, where $s \in (0,1)$.
- $p_A$: Graph sparsity level; probability of an edge in $A$, where $p_A \in [0,1]$.
- $\beta^*$: True regression coefficient vector.
- $\theta^*$: True network interaction parameter vector.
- $X$: Response vector generated from the model.
- $Z$: Covariate matrix.
- $A$: Adjacency matrix generated according to $p_A$.

**For each of the following 3 dimensionality settings**

**Table 1: Experiment Settings**
| Setting | $N$ | $p$ | $s$ |
|----------|-----:|-----:|-----:|
| Low Dimensional | 100 | 20 | 0.50 |
| Moderate High Dimensional | 100 | 200 | 0.05 |
| Strong High Dimensional | 100 | 500 | 0.05 |

Generate $(\beta^*, \theta^*)$ for each setting. For each $p_A \in \{0.02, 0.05, 0.1\}$ with fixed $(\beta^*, \theta^*)$,
generate $T=100$ sets of $(X,A,Z)$ using Algorithm 1. Across the 3 settings and 3 values of $p_A$, this results in $3 \times 3 \times 100 = 900$ simulated datasets. For each group of 100 datasets, split the data into 20 datasets for hyperparameter tuning and 80 datasets for Monte Carlo evaluation.


**Prior Specifications**\
Before conducting the simulation procedure, one of the following prior specifications is selected to define the corresponding estimation framework:

- Prior 1: Gaussian and Laplace Prior.
- Prior 2: Gaussian and Elastic Net Prior.
- Prior 3: Laplace Prior (PMPL Estimator).

**Hyperparameter Tuning**\
For each setting and $p_A$, Algorithm 2 is applied to the 20 calibration datasets. For Prior 1 and 3, $\lambda$ is selected from a logarithmically spaced grid. For Prior 2, $(\lambda,\alpha)$ are selected from a two-dimensional grid, where $\lambda$ is chosen from a logarithmically spaced grid and $\alpha$ from a linearly spaced grid. For Prior 1, the variance hyperparameter $\tau^2$ is fixed across multiple independent experiments and is not included in the tuning procedure. The optimal hyperparameter configuration is defined as the one that minimizes the average estimation error of $\theta^T$ across the calibration datasets.

**Evaluation**\
For each setting and $p_A$, the optimal hyperparameter(s) selected from the 20 calibration datasets is fixed and applied to the remaining 80 datasets using Algorithm 2. The average parameter estimation error is then estimated in a Monte Carlo fashion by averaging the errors over these 80 evaluation datasets.

**Multiple Independent Experiment**\
To evaluate the impact of prior specification on estimation precision and accuracy, six independent experiments were conducted. These experiments consisted of the baseline Prior 3 (PMPL estimator), 4 Bayesian estimators using Prior 1 with pre-fixed variance hyperparameter $\tau^2 \in \{0.5,1,5,10\}$, one Bayesian estimator using Prior 2, and one Bayesian estimator using Prior 3. For Priors 1 and 3, $\lambda$ is selected from a logarithmically spaced grid of 10 values over $[10^{-3},10^{1}]$. For Prior 2, $\lambda$ is selected from the same grid, and $\alpha$ is selected from $\{0.05, 0.25, 0.5, 0.75, 0.95\}$. The Experiment Frameworks with specific hyperparameter configurations are summarized in Table 2

**Table 2: Independent Experiment Framework**
| Index | Prior Choice | Fixed Parameter | Tuning Parameter |
|------:|--------------|----------------|------------------|
| 1 | Prior 1 | $\tau^2 = 0.5$ | $\lambda$ |
| 2 | Prior 1 | $\tau^2 = 1$ | $\lambda$ |
| 3 | Prior 1 | $\tau^2 = 5$ | $\lambda$ |
| 4 | Prior 1 | $\tau^2 = 10$ | $\lambda$ |
| 5 | Prior 2 | $\tau^2 = 10$ | $(\lambda,\alpha)$ |
| 6 | Prior 3 (PMPL) | -- | $\lambda$ |

### Results

**Effect of Prior Variance $\tau^2$**\
The standard ridge penalty has the form $\lambda_{Ridge}\|\cdot\|_2^2$, whereas Prior 1 and 2 use equivalent parameterization $\frac{1}{2\tau^2}\|\cdot\|_2^2$. Thus $\tau^2 \propto \frac{1}{\lambda_{Ridge}}$, which means smaller $\tau^2$ corresponds to stronger shrinkage while larger $\tau^2$ corresponds to weaker shrinkage. As $\tau^2\rightarrow \infty$, the Gaussian penalty disappears and recovers PMPL estimator. By Comparing experiment $\{1,2,3,4,6\}$ outputs from the Strong High-Dimensional setting with $p
_A = 0.02$ in Table 3 (Similar trends were observed across all dimensional settings and graph sparsity levels.), increase of $\tau^2$ which weakens the Gaussian shrinkage effect generally reduces the estimation error of $\beta$. The reduction of shrinkage results in smaller average estimation error of $\beta$ but higher variability of $\hat\beta$. On the other hand, change of $\tau^2$ does not have a significant effect on estimation precision and accuracy on $\theta^T$.

**Table 3: Effect of $\tau^2$ on $\beta$ Estimation Error**
| Experiment | $\tau^2$ | Mean Beta Error | Variance |
|-----------:|----------:|-----------------:|---------:|
| 1 | 0.5 | 0.341 | $<10^{-4}$ |
| 2 | 1 | 0.340 | $<10^{-4}$ |
| 3 | 5 | 0.334 | 0.0001 |
| 4 | 10 | 0.326 | 0.0002 |
| 6 | $\infty$ | 0.272 | 0.0385 |

*Strong High-Dimensional Setting ($p_A = 0.02$)*

Effect of Elastic Net Parameter $\alpha$\
In experiment 5, the tuning procedure consistently selected $\alpha=0.95$ across all settings and sparsity levels. This strongly suggests the model with elastic net prior prefers an almost pure $\ell_1$ penalty. Consequently, the estimation performance of Experiment 5 is generally similar to that of Experiment 4. Moderate improvements in $\beta$ estimation error are observed in several moderate high-dimensional settings, whereas little difference is observed in the strong high-dimensional setting. This suggests that introducing an Elastic Net parameter provides limited benefit on estimation error of both parameters beyond the impact of $\ell_1$ regularization.

**Effect of $\ell_1$ Penalty Term $\lambda$, Dimensional Setting, and Sparsity Level**\
The results of experiment 2 shown in Table 4 demonstrate that the model with a Laplace prior on $\theta^T$ and a Gaussian prior on $\beta$ provides stable estimation performance across different dimensional and graph sparsity $p_A$. For the self-effect parameter $\theta^T$, all distributions are approximately unimodal and symmetric. The estimation error increases as the dimensionality of the problem increases. The average error rises from approximately 1.3–1.6 in the low-dimensional setting to around 3.6–3.7 in the strong high-dimensional setting. This trend is expected, since recovering self sparsity becomes more challenging as the parameter space expands. In addition, the average estimation error tends to increase slightly as $p_A$ increases, suggesting that graph sparsity also affects recovery of the self-effect estimates.
Despite the increase in average estimation error, the variances remain relatively stable across settings, thus the estimation procedure exhibits consistent performance across simulation replications. The selected values of $\lambda$ are generally small across all settings, indicating that only moderate regularization is required. Excessive penalization may lead to over-shrinkage and loss of important signals. For parameter $\beta$, the distributions are approximately unimodal but slightly skewed. The estimation error generally increases with problem dimensionality. Increasing $p_A$ substantially reduces the average estimation error in the low-dimensional setting, while its effect becomes less pronounced as dimensionality increases. This may be because, in higher-dimensional settings, the difficulty of estimating a larger number of parameters becomes the dominant source of error, thereby diminishing the relative impact of graph sparsity on estimation accuracy.

**Table 4: Optimal $\lambda$ and Estimation Performance**

| Setting | $p_A$ | $\lambda$ | Mean($\theta^T$) | Var($\theta^T$) | Mean($\beta$) | Var($\beta$) |
|----------|------:|----------:|-----------------:|----------------:|--------------:|-------------:|
| LD | 0.02 | 0.007743 | 1.350 | 0.0704 | 0.320 | 0.0026 |
| LD | 0.05 | 0.007743 | 1.473 | 0.0984 | 0.253 | 0.0029 |
| LD | 0.10 | 0.002783 | 1.615 | 0.0965 | 0.172 | 0.0019 |
| Moderate HD | 0.02 | 0.002783 | 1.935 | 0.0365 | 0.358 | $<10^{-4}$ |
| Moderate HD | 0.05 | 0.007743 | 1.987 | 0.0380 | 0.319 | 0.0004 |
| Moderate HD | 0.10 | 0.002783 | 2.193 | 0.0635 | 0.296 | 0.0020 |
| High HD | 0.02 | 0.001000 | 3.618 | 0.0610 | 0.340 | $<10^{-4}$ |
| High HD | 0.05 | 0.001000 | 3.618 | 0.0687 | 0.337 | $<10^{-4}$ |
| High HD | 0.10 | 0.002783 | 3.695 | 0.0520 | 0.295 | 0.0012 |

*Experiment 2 Output*


### Conclusion

Across all 6 simulations, PMPL generally achieves the lowest estimation error for both $\theta^T$ and $\beta$. Consistent with this finding, when estimation error is used as the tuning criterion, the hyperparameter in addition to $\lambda$ tends to correspond to a weaker regularization. Specifically, $\tau^2$ for $\beta$ prior variance is generally large, and $\alpha$ for the $\theta^T$ elastic net prior consistently chooses values close to 1. However, introducing regularization consistently reduces the variability of estimates, which reflects the bias-variance tradeoff.

Overall, these results suggest that the PMPL estimator achieves the highest estimation accuracy and exhibits strong robustness across different dimensionality settings and graph sparsity levels. Although additional Bayesian regularization can trade shrinkage bias to reduce the variability of the estimates, strong regularization sacrifices estimation accuracy while providing limited marginal gains in precision.

---

## References

1. Mukherjee, S., Niu, Z., Halder, S., Bhattacharya, B. B., & Michailidis, G. (2024). *Logistic regression under network dependence*. *Journal of Machine Learning Research*, 25(411), 1–62.

2. Makalic, E., & Schmidt, D. F. (2016). *High-dimensional Bayesian regularised regression with the BayesReg package*. arXiv preprint arXiv:1611.06649.

3. Gosho, M., Ohigashi, T., Nagashima, K., Ito, Y., & Maruo, K. (2023). *Bias in odds ratios from logistic regression methods with sparse data sets*. *Journal of Epidemiology*, 33(6), 265–275.

4. Yuan, B., Fan, J., Liang, J., Wibisono, A., & Chen, Y. (2023). *On a class of Gibbs sampling over networks*. In *Proceedings of the 36th Annual Conference on Learning Theory* (pp. 5754–5780). PMLR.

5. Piccioli, G., Troiani, E., & Zdeborova, L. (2024). *Gibbs sampling the posterior of neural networks*. *Journal of Physics A: Mathematical and Theoretical*, 57(12), 125002.

6. Johnson, A. A., Ott, M. Q., & Dogucu, M. (2021). *Chapter 13: Logistic Regression*. In *Bayes Rules! An Introduction to Applied Bayesian Modeling*. CRC Press.

7. Gelman, A., Carlin, J. B., Stern, H. S., Dunson, D. B., Vehtari, A., & Rubin, D. B. (2013). *Bayesian Data Analysis* (3rd ed.). CRC Press.

8. Algamal, Z. Y., & Lee, M. H. (2015). *Regularized logistic regression with adjusted adaptive elastic net for gene selection in high dimensional cancer classification*. *Computers in Biology and Medicine*, 67, 136–145.

---

## Appendix

**Algorithm 1: Gibbs Sampling Style Network Simulation**
<div style="border:2px solid black; padding:12px; margin:12px 0; border-radius:4px;">

**Input:** Sample size $N$, dimension $p$, graph sparsity $p_A$, true parameters $\beta^*$ and $\theta^*$, Gibbs iterations $K$.

1. Generate the adjacency matrix.

   For each pair $1 \le j < i \le N$:

   - Set $A_{ii}=0$
   - Sample $A_{ij}=A_{ji}\sim\mathrm{Bernoulli}(p_A)$

2. Generate covariates $Z_{ij}\overset{\text{i.i.d.}}{\sim}N(0,1), \qquad i=1,\ldots,N,\; j=1,\ldots,p.$

3. Initialize $X^{(0)}\in\{-1,+1\}^{N}$ uniformly at random.

4. For $k=1,\ldots,K$:
    For $i=1,\ldots,N$:

      - Compute $S_i^{(k)} = \theta^{*T}Z_i + \beta^*\sum_{j=1}^{N} A_{ij}X_j^{(k-1)}.$

      - Compute $p_i^{(k)} = \operatorname{sigmoid} \left(2S_i^{(k)}\right)$

      - Sample
        $$
        X_i^{(k)}
        =
        \begin{cases}
        +1, & \text{with probability } p_i^{(k)},\\
        -1, & \text{with probability } 1-p_i^{(k)}.
        \end{cases}
        $$

5. Return $(X^{(K)},A,Z)$
</div>


**Algorithm 2: Gibbs Sampling Style Network Simulation**
<div style="border:2px solid black; padding:12px; margin:12px 0; border-radius:4px;">

### Algorithm 2. Prox GD with Backtracking Line Search

**Prior Case:**

**Prior Choice 1 (Gaussian and Laplace Prior):**

$\mathcal L(\theta^T,\beta) = -\ell(\theta^T,\beta) + \lambda |\theta^T|_1 + \frac{\beta^2}{2\tau^2}$

$\operatorname{prox}*{\eta*\theta,g}(z)=\mathrm{sign}(z)\cdot\max(|z|-\eta_\theta\lambda,0)$

**Prior Choice 2 (Gaussian and Elastic Net Prior):**

$\mathcal L(\theta^T,\beta) = -\ell(\theta^T,\beta) + \lambda\left[\alpha|\theta^T|_1+\frac{1-\alpha}{2}|\theta^T|_2^2\right]+\frac{\beta^2}{2\tau^2}$

$\operatorname{prox}*{\eta*\theta,g}(z)=\mathrm{sign}(z)\cdot\max(|z|-\eta_\theta\lambda\alpha,0)$

**Prior Choice 3 (Laplace Prior):**

$\mathcal L(\theta^T,\beta) = -\ell(\theta^T,\beta)+\lambda|\theta^T|_1$

$\operatorname{prox}*{\eta*\theta,g}(z)=\mathrm{sign}(z)\cdot\max(|z|-\eta_\theta\lambda,0)$

**Input:** Data $(X,A,Z)$, tuning parameter $\lambda$, prior variance $\tau^2$, initial step sizes $\eta_{\beta0},\eta_{\theta0}$, shrinkage factor $\rho$, Armijo parameter $\epsilon$, tolerance $\mathrm{tol}$.

1. Define objective $\mathcal L(\theta^T,\beta)$ based on prior choice.

2. Initialize $\theta_{\mathrm{old}}=\mathbf 0$ and $\beta_{\mathrm{old}}=0$.

3. While True:

   - Compute $g_\theta=\nabla_\theta \mathcal L_{\mathrm{smooth}}(\theta_{\mathrm{old}},\beta_{\mathrm{old}})$.

   - Compute $g_\beta=\nabla_\beta \mathcal L(\theta_{\mathrm{old}},\beta_{\mathrm{old}})$.

   - Compute $f_{\mathrm{old}}=\mathcal L(\theta_{\mathrm{old}},\beta_{\mathrm{old}})$.

   - Initialize $\eta_\theta=\eta_{\theta0}$ and $\eta_\beta=\eta_{\beta0}$.

   - While True:

      - Set $\beta_{\mathrm{trial}}=\beta_{\mathrm{old}}-\eta_\beta g_\beta$, $\theta_{\mathrm{trial}}=\theta_{\mathrm{old}}-\eta_\theta g_\theta$.

      - Set $\theta_{\mathrm{trial}}=\operatorname{prox}*{\eta*\theta,g}(\theta_{\mathrm{trial}})$ based on prior choice.

      - Compute $f_{\mathrm{new}}=\mathcal L(\theta_{\mathrm{trial}},\beta_{\mathrm{trial}})$.

      - If $f_{\mathrm{new}}\le f_{\mathrm{old}}-\epsilon\left(\eta_\theta|g_\theta|*2^2+\eta*\beta g_\beta^2\right)$, break.

      - Set $\eta_\theta=\rho\eta_\theta$ and $\eta_\beta=\rho\eta_\beta$.

   - Set $\theta_{\mathrm{new}}=\theta_{\mathrm{trial}}$ and $\beta_{\mathrm{new}}=\beta_{\mathrm{trial}}$.

   - If $\dfrac{|\theta_{\mathrm{new}}-\theta_{\mathrm{old}}|*2+|\beta*{\mathrm{new}}-\beta_{\mathrm{old}}|}{|\theta_{\mathrm{old}}|*2+|\beta*{\mathrm{old}}|+10^{-8}} < \mathrm{tol}$, break.

   - Set $\theta_{\mathrm{old}}=\theta_{\mathrm{new}}$ and $\beta_{\mathrm{old}}=\beta_{\mathrm{new}}$.

4. Return $(\beta_{\mathrm{new}},\theta_{\mathrm{new}})$.

</div>


### Code and Error Plots

[Github Repository Portal](https://github.com/Designer-Bosen/Stats-199)