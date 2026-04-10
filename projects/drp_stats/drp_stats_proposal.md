[← Back](index.html)

# Stats 199 - Directed Research Proposal (Tentative)

**Bosen Yang** mentored by **Professor Michailidis**

**Topic: High-Dimensional Bayesian Inference for Logistic Regression Under Network Dependence**

---

# Preliminary Backgrounds and Motivations

The recent work *High Dimensional Logistic Regression Under Network Dependence*, conducted by Professor Michailidis and collaborators, develops a framework incorporating dependence using an **Ising-type interaction term** and estimates parameters using **penalized maximum pseudo-likelihood (PMPL)**.

This approach avoids the computational intractability of the full likelihood while still achieving the classical high-dimensional estimation rate under sparsity assumptions. The full likelihood of the model is computationally intractable due to the **partition function** in the Ising-type interaction term, which motivates the use of pseudo-likelihood estimation. While the pseudo-likelihood approach provides computationally feasible estimation, it sacrifices the full probabilistic structure of the model and limits Bayesian interpretation. However, under assumptions such as **Dobrushin-type weak dependence conditions**, the PMPL estimator achieves consistency rates comparable to the classical independent logistic regression setting.

Based on these results, the following research direction is motivated: From a Bayesian perspective, can we study high-dimensional logistic regression under weak network dependence and establish theory for posterior contraction and parameter estimation?

---

# Research Objectives

The main goal of this project is to investigate **Bayesian inference for high-dimensional models under network dependence**.

### 1. Assumptions

Focus on weak dependence frameworks such as **Dobrushin conditions**, which allow concentration inequalities for dependent random variables.

### 2. Theory

Analyze properties required for high-dimensional inference:

- Gradient concentration of likelihood or pseudo-likelihood  
- Restricted Strong Convexity (RSC) or related curvature conditions  
- Convergence of optimization or posterior inference  

### 3. Model Formulation

Develop a **Bayesian formulation** of the logistic model by placing sparsity-inducing priors on regression coefficients.

Investigate:

- Posterior contraction rates  
- Consistency  
- Computational approaches such as **MCMC**

---

# Research Plan

## Phase 1 — Literature Review

Review foundational literature on:

- High-dimensional logistic regression  
- Ising and graphical models  
- Pseudo-likelihood estimation under dependence  
- High-dimensional Bayesian inference  
- Convexity and sparsity  

## Phase 2 — Framework Formulation (Theory)

Formulate the Bayesian logistic regression model under network dependence.

Analyze whether gradient concentration, convexity, and sparsity conditions hold in this regime.

## Phase 3 — Simulation (Computation)

Implement simulations to:

- Verify theoretical findings  
- Illustrate behavior of the Bayesian model  
- Compare with pseudo-likelihood estimators  

---

# Expected Outcomes

- A **3–5 page report** summarizing theoretical results and simulations. 
- Numerical experiments implemented in **Python,**.