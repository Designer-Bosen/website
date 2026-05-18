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

Motivated by my research interest in high dimensional statistics Bayesian inference...



---
## My Direction

Given the above results from the anchor paper, I aims to conduct a simulation study of a logistic regression model under network dependence in Bayesian perspective. The goal is to empirically evaluate its performance under this regime, including posterior contraction behavior, estimation accuracy, and computational efficiency of inference methods such as MCMC. **Keywords**: High-dimensional Bayesian Inference, restricted strong convexity

**Bayesian Theorem:** $\mathbb{P}(A \mid B)=\frac{\mathbb{P}(B \mid A)\cdot \mathbb{P}(A)}{\mathbb{P}(B)}$

From Bayesian perspective, parameter $\theta$ is treated as a random variable and follows some distribution. If $Y$ represents the outcome data, the probability density function of $\theta$ give $Y$ can be represented as:

$$f(\theta \mid Y) = \frac{f(Y \mid \theta) \cdot f(\theta)}{f(Y)} = \frac{\text{Likelihood} \cdot \text{Prior}}{\text{Normalizing Constant}} \propto \text{Likelihood} \cdot \text{Prior}$$

---

## Phase 1: [Model the network in Python](index.html?file=drp_stats_phase1.md)

## Phase 2: [Bayesian Logistic Regression Model Setup](index.html?file=drp_stats_phase2.md)

## Phase 3: [Optimization](index.html?file=drp_stats_phase3.md)

## Phase 4: [Experiment Flow](index.html?file=drp_stats_phase4.md)


---

## Informal Reference Section

Bayesian Data Analysis Third Edition, Gelman

High-Dimensional Bayesian Regularized Regression with the bayesreg Package (arXiv:1611.06649)

https://johnwlambert.github.io/subgradient-methods/

https://www.bayesrulesbook.com/chapter-13

https://www2.stat.duke.edu/~banks/521-lectures.dir/lect10.pdf