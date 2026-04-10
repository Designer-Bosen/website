[← Back](../index.html)

# Stats 199 - Directed Research

---

## Navigation Links

[Tentative Research Proposal](index.html?file=drp_stats_proposal.md)

[Anchor Paper Ideas](index.html?file=drp_stats_anchor_paper_ideas.md)

[Weekly Meeting Summary](index.html?file=drp_stats_meeting_sum.md)

---

## Project Description

This **Stats 199 - Directed Research** is a 1-quarter contract course at UCLA Statistics and Data Science conducted under the supervision of Professor George Michailidis. The research is built upon on the recent work [High Dimensional Logistic Regression Under Network Dependence](https://jmlr.org/papers/volume25/22-1040/22-1040.pdf) which addresses the presence of network dependence in observations via introducing Ising-type network interaction structure into the logistic regression model. The paper proosed a penalized maximum pseudo-likelihood (PMPL) approach to overcome computational burden of full likelihood. This approach enables efficient estimation in high-dimensional setting while achieving descent computational rate and approximation accuracy. In particular, under sparsity and weak dependence conditions, PMPL estimator achieves consistency comparable to classical logistic regression with independent data.

Motivated by my research interest in high dimensional Bayesian inference...



---
## My Direction

Given above results from the anchor paper, I want to formulate a Bayesian logistic regression model under network dependence. Then analyze its properties under this regime e.g. posterior contraction rates, consistency, computational approaches such as MCMC.

Keywords: High-dimensional Bayesian Inference, restricted strong convexity

### Prior Knowledge on Bayesian Inference

**Bayesian Theorem:** $\mathbb{P}(A \mid B)=\frac{\mathbb{P}(B \mid A)\cdot \mathbb{P}(A)}{\mathbb{P}(B)}$

From Bayesian perspective, parameter $\theta$ is treated as a random variable and follows some distribution. If $Y$ represents the data, the probability density function of $\theta$ give $Y$ can be represented as:

$$f(\theta \mid Y) = \frac{f(Y \mid \theta) \cdot f(\theta)}{f(Y)} = \frac{\text{Likelihood} \cdot \text{Prior}}{\text{Normalizing Constant}} \propto \text{Likelihood} \cdot \text{Prior}$$

---

### STEP 1 - Model the network in Python

**Adjacency Matrix:** $A \in \mathbb{R}^{N\times N}_sym$ represent the network struture. If node $i$ is connected to node $j$, then $A_{ij} = A_{ji} = 1$. Otherwise $A_{ij} = A_{ji} = 0$. Each non-diagonal entry is generated using a Bernoulli distribution with tunable parameter $s$ controlling sparsity.

**Covariate Matrix:** $Z \in \mathbb{R}^{N\times p}$ represent the collection of node-specific features, where $Z_i$ corresponds to the covariate vector of node $i$. $Z$ captures the individual effects, and entries of Z are generated independent from a chosen distribution.

#### Determination of the Outcome Vector X
Each node's outcome $X_i \in {0, 1}$ for $i = 1,\dots, N$ could be determined by a probablistic model (map) using the score vector $S$ and $S_i=\text{Individual Feature} + \beta \times \text{Neighbor Influence}$. $\beta$ is the "Peer Effect"

**Notes:** There is a trade-off between individual features and neighbor influence, and the outcome depends on which effect is stronger. For example, If a node’s own features strongly favor 1, but neighbors are mostly 0, this means the node's own feature dominates. If a node has most of its neighbors being 1, the probability for itself being one increases.


**Code:**



---

## Informal Reference Section

Bayesian Data Analysis Third Edition, Gelman

High-Dimensional Bayesian Regularized Regression with the bayesreg Package (arXiv:1611.06649)
