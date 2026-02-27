# Directed Reading Program - Generalized Matrix Norms
**Mentored by PhD Student William Chang**  
[Anchor paper](https://epubs.siam.org/doi/epdf/10.1137/23M1605545)


## Concepts and Foundational Knowledge

### Matrix Norm
Let $M_{m\times n}$ denote the space of all $m \times n$ matrices over $\mathbb{C}$. A function: $\|\cdot\|:V \to \mathbb{R}$ is called a matrix norm if for all matrices $A$, $B$ and all scalars $\alpha$:
1) Positivity: $\|A\| \geq 0$, $\|A\|=0 \Leftrightarrow A=0$
2) Homogeneity: $\|\alpha A\|=|\alpha|\|A\|$
3) Triangular Inequality: $\|A+B\|\leq \|A\|+\|B\|$


NOTES: concepts about ellipsotope and why this matter in higher dimensions. Consider why hypersphere inclusion problem is easy to solve but ellipsotope is hard in higher dimensions.


NOTES: study hilbert space and Banach space in functional analysis.


## Notes on the Paper

### Generalized Matrix Norm



### Holder Conjugates

$\\frac{1}{p}+\frac{1}{q}\geq 1$



### NP-Hardness

**Naive Understanding:** NP-hard problems references to the set of problem is at least as hard as every problem in NP (does not have to belong to the NP set). NP can be reduced to NP-hard problem in polynomial time. Then, solving an NP-hard problem efficiently would imply that all NP problems can be solved efficiently.


### 2.3 Duality
Def: Let $(\mathcal{V}, \|\cdot\|_\mathcal{V})$ be a (real) normed vector space. We denote by $\mathcal{V}^*$ its dual vector space, i.e., $\mathcal{V}=\{f:\mathcal{V}\rightarrow \mathbb{R}|f \text{is linear and continuous}\}$, endowed with the dual norm $\|\cdot\|^*$, defined for $f \in \mathcal{V}^*$ as  
$$\|f\|^* = \sup_{\|x\|<1} f(x) \tag{2.1}$$

**My answer why this definition make $\|f\|^*$ non-negative:** 
The unit ball is symmetric: if $\|x\|<1$, then $\|-x\|<1$. Since $f$ is linear, $f(-x)=-f(x)$. Hence the set $\{f(x):\|x\|<1\}$ is symmetric about $0$, so its supremum is non-negative.






# Research in Machine Learning Application