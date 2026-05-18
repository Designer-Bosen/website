[← Back](index.html)

# Phase 1: Model the network in Python

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
- For each node i:  
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