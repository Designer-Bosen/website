[← Back](index.html)

# Phase 4: Experiment 

## Experiment Parameters
- $N$: number of observations
- $p$: number of covariates 
- $T$: number of random experiments

## Data Generating Parameters
- $p_{\not 0}$: number of non-zero entries in $\theta$, $(p_{\not 0} < p)$
- $p_A$: graph sparsity $(p_A \in [0,1])$ 
- $\beta^*$: true beta being generated from a distribution
- $\theta^*$: true theta with entries being generated from a distribution
- $Z$: covariate matrix being generated from a distribution
- $A$: adjacenct matrix generated with $p_A$

## Optimization Hyperparameters
- Prior for $\beta$ and corresponding distribution parameters (e.g. Gaussian with $\tau^2$)
- Prior for $\theta$ and corresponding distribution parameters (e.g. Laplace with $\lambda$)

## Experiment
For each of the following settings: 

| Setting                        |   N   |   p   |
|:-------------------------------|:------|:------|
| 1. Low Dimensional                | 100   | 20    |
| 2. Moderate High Dimensional      | 100   | 200   |
| 3. Strong High Dimensional        | 100   | 500   |

- Generate 9 set of $(\beta^*, \theta^*, X, A, Z)$, 
with $p_{\not0} \in \{5, 10, 20\}$ and $p_A \in \{0.02, 0.05, 0.1\}$
- For each set, 
    - run $T$ number of random experiments. Tune the hyperparameters if needed.
    - Record the output in a file.