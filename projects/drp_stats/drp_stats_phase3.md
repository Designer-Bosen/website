[← Back](index.html)

# Phase 3: Optimization

With $\beta \sim N(0, \tau^2_\beta)$ and $\theta^T_i \sim \text{Laplace}(\lambda)$, the MAP estimator is given as:

$$(\hat\theta^T, \hat\beta)_{MAP}=\arg\min_{\theta^T, \beta} \big\{ \mathcal{L}(\theta^T, \beta)\big\}$$

with

$$\mathcal{L}(\theta^T, \beta)=-\ell(\theta^T, \beta) + \lambda\|\theta^T\|_1 + \frac{\beta^2}{2\tau_\beta^2}$$

### GRADIENT (smooth-terms)

$$\nabla_{\beta}\mathcal{L}(\theta^T, \beta)=-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)m_i(X)+\frac{\beta}{\tau_\beta^2}$$

$$\nabla_{\theta^T} \big\{-\ell(\theta^T, \beta) \big\}= -2\sum_{i=1}^N X_i \sigma(-2X_iS_i)\mathbf{Z}_i$$

### HESSIAN MATRIX (smooth-terms)

$$\nabla_{\beta}^2\mathcal{L}(\theta^T, \beta)=4\sum_{i=1}^N w_i m_i(X)^2+\frac{1}{\tau_\beta^2}$$

$$\nabla^2_{\theta^T} \big\{-\ell(\theta^T, \beta) \big\}=4\sum_{i=1}^N w_i \mathbf{Z}_i\mathbf{Z}_i^T$$

$$\nabla_{\beta}\nabla_{\theta^T} \big\{-\ell(\theta^T, \beta) \big\}= 4\sum_{i=1}^N w_i \mathbf{Z}_im_i(X)$$

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

----

### PSEUDO CODE (Promimal GD with backtracking line search)

**STEP 1: Define Parameters**
- $\lambda$: Laplace prior penalty coefficient (tuning)
- $\tau^2$: Gaussian prior variance (tuning)
- $\text{tol}=10^{-6}$: Stopping tolerance
- $\text{shrink}=0.5$: Backtracking step size shrinkage factor
- $\text{eps}=0.001$: Backtracking decreasing factor
- $\eta_{\beta0}=1$: GD w.r.t $\beta$ initial step size
- $\eta_{\theta0}=1$: GD w.r.t $\theta^T$ initial step size

**STEP 2: Define Functions**
- $\text{fcn}(\theta, \beta)$: $\mathcal{L}(\theta^T, \beta)=-\ell(\theta^T, \beta) + \lambda\|\theta^T\|_1 + \frac{\beta^2}{2\tau_\beta^2}$
- $\text{grad}_\beta(\theta, \beta)$: $\nabla_{\beta}\mathcal{L}(\theta^T, \beta)=-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)m_i(X)+\frac{\beta}{\tau_\beta^2}$
- $\text{grad}_\theta(\theta, \beta)$: $\nabla_{\theta^T} \big\{-\ell(\theta^T, \beta) \big\}=-2\sum_{i=1}^N X_i \sigma(-2X_iS_i)\mathbf{Z}_i$

**STEP 3: Optimize**

- Initialize $\beta_{old}=0$, $\theta_{old}=\vec0$, 
- Initialize $\beta_{new}=\beta_{old}$, $\theta_{new}=\theta_{old}$
- Input $X$, $A$, $Z$

while True:
- $g_{\beta} = \text{grad}_\beta(\theta_\text{old}, \beta_\text{old})$
- $g_{\theta} = \text{grad}_\theta(\theta_\text{old}, \beta_\text{old})$
- $f_{old} = \text{fcn}(\theta_\text{old}, \beta_\text{old})$
- $\eta_{\beta} = \eta_{\beta0}$
- $\eta_{\theta} = \eta_{\theta0}$

- while True:
    - $\beta_\text{trail} = \beta_\text{old} - \eta_{\beta} \cdot g_{\beta}$
    - $\theta_\text{trail} = \theta_\text{old} - \eta_{\theta} \cdot g_{\theta}$
    - $\theta_\text{trail} = \text{sign}(\theta_\text{trail}) \cdot \max(|\theta_\text{trail}|-\eta_{\theta} \lambda)$
    - $f_{new} = \text{fcn}(\theta_\text{trail}, \beta_\text{trail})$
    - if $f_{new} \leq f_{old} - \text{eps} \cdot (\eta_{\beta} \cdot \|g_{\theta}\|_2^2 + \eta_{\beta} \cdot g_{\beta}^2)$ 
        - break
    - $\eta_{\beta}  = \text{shrink} \cdot \eta_{\beta}$
    - $\eta_{\theta} = \text{shrink} \cdot \eta_{\theta}$

- if $\frac{\|\theta_\text{old} - \theta_\text{old}\|_2 + |\beta_\text{old} - \beta_\text{old}|}{\|\theta_\text{old}\|_2 + |\beta_\text{old}| + 10^{-8}} < \text{tol}$
    - break
- $\beta_{old}=\beta_{new}$, $\theta_{old}=\theta_{new}$

return $\beta_{new}$, $\theta_{new}$