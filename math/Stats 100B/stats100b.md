# Stats 100B - Introduction to Mathematical Statistics

**Instructor:** Professor Nicolas Christou

---

## Distribution Summary

$$\textbf{Discrete Distributions}$$

| Distribution | Probability Mass Function | Mean | Variance | Moment Generating Function |
|--------------|---------------------------|:----:|:--------:|----------------------------|
| Binomial | $P(X=x)=\binom{n}{x}p^x(1-p)^{n-x}$<br>$x=0,1,\ldots,n$ | $np$ | $np(1-p)$ | $\left(pe^t+(1-p)\right)^n$ |
| Geometric | $P(X=x)=(1-p)^{x-1}p$<br>$x=1,2,\ldots$ | $\frac{1}{p}$ | $\frac{1-p}{p^2}$ | $\frac{pe^t}{1-(1-p)e^t}$ |
| Negative Binomial | $P(X=x)=\binom{x-1}{r-1}p^r(1-p)^{x-r}$<br>$x=r,r+1,\ldots$ | $\frac{r}{p}$ | $\frac{r(1-p)}{p^2}$ | $\left(\frac{pe^t}{1-(1-p)e^t}\right)^r$ |
| Hypergeometric | $P(X=x)=\dfrac{\binom{r}{x}\binom{N-r}{n-x}}{\binom{N}{n}}$<br>$x=0,\ldots,n$ if $n\le r$<br>$x=0,\ldots,r$ if $n>r$ | $\dfrac{nr}{N}$ | $n\dfrac{r}{N}\dfrac{N-r}{N}\dfrac{N-n}{N-1}$ | Fairly complicated |
| Poisson | $P(X=x)=\dfrac{\lambda^xe^{-\lambda}}{x!}$<br>$x=0,1,\ldots$ | $\lambda$ | $\lambda$ | $\exp\!\left(\lambda(e^t-1)\right)$ |

$$\textbf{Continuous Distributions}$$

| Distribution | Probability Density Function | Mean | Variance | Moment Generating Function |
|--------------|------------------------------|:----:|:--------:|----------------------------|
| Uniform | $f(x)=\dfrac1{b-a}$<br>$a\le x\le b$ | $\dfrac{a+b}{2}$ | $\dfrac{(b-a)^2}{12}$ | $\dfrac{e^{tb}-e^{ta}}{t(b-a)}$ |
| Gamma | $f(x)=\dfrac{x^{\alpha-1}e^{-x/\beta}}{\beta^\alpha\Gamma(\alpha)}$<br>$\alpha,\beta>0,\ x\ge0$ | $\alpha\beta$ | $\alpha\beta^2$ | $(1-\beta t)^{-\alpha}$ |
| Exponential | $f(x)=\lambda e^{-\lambda x}$<br>$\lambda>0,\ x\ge0$ | $\dfrac1\lambda$ | $\dfrac1{\lambda^2}$ | $\left(1-\dfrac{t}{\lambda}\right)^{-1}$ |
| Beta | $f(x)=\dfrac{x^{\alpha-1}(1-x)^{\beta-1}}{B(\alpha,\beta)}$<br>$\alpha,\beta>0,\ 0\le x\le1$ | $\dfrac{\alpha}{\alpha+\beta}$ | $\dfrac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$ |  |
| Normal | $f(x)=\dfrac1{\sigma\sqrt{2\pi}}e^{-\frac12(\frac{x-\mu}{\sigma})^2}$<br>$-\infty < x < \infty$ | $\mu$ | $\sigma^2$ | $e^{\mu t+\frac{\sigma^2t^2}{2}}$ |

---

# Lec 1: Random vectirs and properties

## Mean and variance of a random vector

Let $\mathbf{Y}=\begin{pmatrix} Y_1 \\ Y_2 \\ \vdots \\ Y_n \end{pmatrix}$ be a random vector with $E\mathbf{Y}=\begin{pmatrix} EY_1 \\ EY_2 \\ \vdots \\ EY_n \end{pmatrix}=\begin{pmatrix} \mu_1 \\ \mu_2 \\ \vdots \\ \mu_n \end{pmatrix}$. The covariance matrix of $\textbf{Y}$ denoted with $\Sigma = cov(Y)$ is defined as follows:

$$
\begin{aligned}
cov(\mathbf{Y})&=E(\mathbf{Y} - \boldsymbol{\mu})(\mathbf{Y} - \boldsymbol{\mu})^T \\
&= E\begin{pmatrix} \mathbf{Y}_1 - \boldsymbol{\mu}_1 \\ \mathbf{Y}_2 - \mathbf{\mu}_2 \\ \vdots \\ \mathbf{Y}_n - \boldsymbol{\mu}_n \end{pmatrix} \begin{pmatrix} \mathbf{Y}_1 - \boldsymbol{\mu}_1 & \mathbf{Y}_2 - \boldsymbol{\mu}_2 & \dots & \mathbf{Y}_n - \boldsymbol{\mu}_n \end{pmatrix} \\
&= \begin{pmatrix}
(\mathbf{Y}_1-\boldsymbol{\mu}_1)^2 &
(\mathbf{Y}_1-\boldsymbol{\mu}_1)(\mathbf{Y}_2-\boldsymbol{\mu}_2) &
\cdots &
(\mathbf{Y}_1-\boldsymbol{\mu}_1)(\mathbf{Y}_n-\boldsymbol{\mu}_n) \\
(\mathbf{Y}_2-\boldsymbol{\mu}_2)(\mathbf{Y}_1-\boldsymbol{\mu}_1) &
(\mathbf{Y}_2-\boldsymbol{\mu}_2)^2 &
\cdots &
(\mathbf{Y}_2-\boldsymbol{\mu}_2)(\mathbf{Y}_n-\boldsymbol{\mu}_n) \\
\vdots & \vdots & \ddots & \vdots \\
(\mathbf{Y}_n-\boldsymbol{\mu}_n)(\mathbf{Y}_1-\boldsymbol{\mu}_1) &
(\mathbf{Y}_n-\boldsymbol{\mu}_n)(\mathbf{Y}_2-\boldsymbol{\mu}_2) &
\cdots &
(\mathbf{Y}_n-\boldsymbol{\mu}_n)^2
\end{pmatrix} \\
&= \begin{pmatrix}
\sigma_{11} & \sigma_{12} & \cdots & \sigma_{1n} \\
\sigma_{21} & \sigma_{22} & \cdots & \sigma_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
\sigma_{n1} & \sigma_{n2} & \cdots & \sigma_{nn}
\end{pmatrix} = \boldsymbol{\Sigma}
\end{aligned}
$$

So $\boldsymbol{\Sigma}$ is the covariance matrix of $\mathbf{Y}$. It is symmetric and positive definite. Suppose $Y_1, \dots, Y_n$ are independent identically distributed (i.i.d.) random variables. This means that $E[Y_i] = \mu, i = 1, \dots, n$; $var[Y_i] = \sigma^2, i = 1, \dots, n$ and $cov[Y_i, Y_j] = 0, i \neq j$. Then $E[\mathbf{Y}]=\mu \begin{pmatrix} 1 \\ 1 \\ \vdots \\ 1\end{pmatrix} $, $var[\mathbf{Y}]=\sigma^2I_n$


## Result 1 
Expected value and variance of a linear combination of $\mathbf{Y}$. Let $\mathbf{a}^T=\begin{pmatrix} a_1 & a_2 & \dots & a_n \end{pmatrix}$ be a vector of constants and let $q=\mathbf{a}^T\mathbf{Y}$. Then $E[q]=E[\mathbf{a}^T\mathbf{Y}]=\mathbf{a}^TE[\mathbf{Y}]=\mathbf{a}^T\boldsymbol{\mu}$. The variance of q can be found as follows:

$$
\begin{aligned}
var[q]=E[(q-\boldsymbol{\mu}_q)^2]
&=E[(\mathbf{a}^T\mathbf{Y} - \mathbf{a}^T\boldsymbol{\mu})(\mathbf{a}^T\mathbf{Y} - \mathbf{a}^T\boldsymbol{\mu})] \\
&= \mathbf{a}^T E[(\mathbf{Y} - \boldsymbol{\mu})(\mathbf{Y} - \boldsymbol{\mu})^T] \mathbf{a} \\
&= \mathbf{a}^T \boldsymbol{\Sigma} \mathbf{a}
\end{aligned}
$$

Note: $q$ is a scalar and therefore its variance should be a scalar and not a matrix. ($\mathbf{a}^T \boldsymbol{\Sigma} \mathbf{a}$ is $1 \times 1$) We can also express the variance of a linear combination as follows:

$$
\begin{aligned}
var[\sum_{i=1}^na_iY_i]&=cov[\sum_{i=1}^na_iY_i, \sum_{j=1}^na_jY_j] \\
&= \sum_{i=1}^n \sum_{j=1}^n a_ia_j cov[Y_i, Y_j] \\
&= \sum_{i=1}^n a_i^2 var[Y_i] + \sum_{i=1}^n \sum_{j \neq i}^n a_i a_j cov[Y_i, Y_j] \\
&= \sum_{i=1}^n a_i^2 var[Y_i] + 2\sum_{i=1}^{n-1} \sum_{j > i}^n a_i a_j cov[Y_i, Y_j]
\end{aligned}
$$


## Result 2
Let $\mathbf{A}$ be a $p \times n$ matrix of constants. $\mathbf{Q} = \mathbf{A}\mathbf{Y}$ is a $p \times 1$ vector and therefore its variance should be a $p \times p$ matrix. $E[\mathbf{Q}]=E[\mathbf{A}\mathbf{Y}]=\mathbf{A}E[Y]=\mathbf{A}\boldsymbol{\mu}$. For the variance of $\mathbf{Q}$ use the definition of the covariance matrix of a random vector.

$$
\begin{aligned}
var[\mathbf{Q}]&=E[(\mathbf{Q} - \boldsymbol{\mu}_\mathbf{Q})(\mathbf{Q} - \boldsymbol{\mu}_\mathbf{Q})^T] \\
&= E[(\mathbf{A}\mathbf{Y} - \mathbf{A}\boldsymbol{\mu})(\mathbf{A}\mathbf{Y} - \mathbf{A}\boldsymbol{\mu})^T] \\
&= \mathbf{A} E[(\mathbf{Y} - \boldsymbol{\mu})(\mathbf{Y} - \boldsymbol{\mu})^T] \mathbf{A}^T \\
&= \mathbf{A} \boldsymbol{\Sigma} \mathbf{A}^T
\end{aligned}
$$


## Result 3 - Expectation of a quadratic expression
Let $\mathbf{Y}$ be a random vector $n \times 1$ and let $\mathbf{A}$ be an $n \times n$ matrix of constants. Consider the quadratic expression $\mathbf{Y}^T\mathbf{A}\mathbf{Y}$. To find the expected value of this quadratic expression, we will use properties of hte trace of a square matrix. We can do this because $\mathbf{Y}^T\mathbf{A}\mathbf{Y}$ isi a scalar. We will also need this result: $E[\mathbf{Y}\mathbf{T}^T]=\boldsymbol{\Sigma} + \boldsymbol{\mu}\boldsymbol{\mu}^T$. 

$$
\begin{aligned}
E[\mathbf{Y}^T\mathbf{A}\mathbf{Y}]=E[tr(\mathbf{Y}^T\mathbf{A}\mathbf{Y})]&=E[tr(\mathbf{A}\mathbf{Y}\mathbf{Y}^T)] \\
&= tr(E[\mathbf{A}\mathbf{Y}\mathbf{Y}^T]) \\
&= tr(\mathbf{A}E[\mathbf{Y}\mathbf{Y}^T]) \\
&= tr(\mathbf{A}(\boldsymbol{\Sigma} + \boldsymbol{\mu}\boldsymbol{\mu}^T)) \\
&= tr(\mathbf{A}\boldsymbol{\Sigma} + \mathbf{A}\boldsymbol{\mu}\boldsymbol{\mu}^T) \\
&= tr(\mathbf{A}\boldsymbol{\Sigma}) + tr(\mathbf{A}\boldsymbol{\mu}\boldsymbol{\mu}^T) \\
&= tr(\mathbf{A}\boldsymbol{\Sigma}) + \boldsymbol{\mu}^T\mathbf{A}\boldsymbol{\mu} \\
\end{aligned}
$$


## Other Results
$cov[\mathbf{a}^T\mathbf{Y}, \mathbf{b}^T\mathbf{Y}] = \mathbf{a}^T\boldsymbol{\Sigma}\mathbf{b}$. This is a scalar

$cov[\mathbf{A}^T\mathbf{Y}, \mathbf{B}^T\mathbf{Y}] = \mathbf{A}^T\boldsymbol{\Sigma}\mathbf{B}$. This is a matrix


---


# Lec 2: A note on expectation, independence, and exponential families

1. Let $X$ be a continuous random variable. Then $E[X]=\int_x xf(x)dx$
2. Suppose we want to find the expectation of a function of $X$. Let $Y = g(X)$. Show that $E[g(X)]=\int_xg(x)f(x)dx$
    <div style="border:2px solid black; padding:12px; margin:12px 0; border-radius:4px;">
    
    **Proof**:

    One way to compute $E[Y]$ is to find the pdf of $Y$. Use the method of cdf. Begin with the cdf of Y.
    $$F_Y(y) = P[Y \leq y] = P[g(X) \leq y] = P[X \leq w(y)] = F_X(w(y))$$

    Take derivative on both sides w.r.t. $y$ to get

    $$f_Y(y) = w'(y) f_x(w(y)) \qquad E[Y]=\int_y y w'(y) f_x(w(y)) dy$$
    
    Back to the proof. Let $I = \int_xg(x)f(x)dx$, $y = g(x)$ and solve for $x$. We get $x = w(y)$, $\frac{dx}{dy} = w'(y)$. Transform the integral $I$ in terms of y: 

    $$I = \int_xg(x)f(x)dx = \int_y y f(w(y)) w'(y) dy = E[Y]$$
    </div>

## Kernel Function 

Let $X \sim \Gamma(\alpha, \beta), \alpha > 0, \beta > 0, x > 0$. Then $f(x) = \frac{x^{\alpha-1} e^{-\frac{x}{\beta}}}{\Gamma(\alpha)\beta^\alpha}$. The part of the pdf $x^{\alpha-1} e^{-\frac{x}{\beta}}$ is called kernel function.

**Definition (gamma function)**: $\Gamma(\alpha) = \int_0^\infty x^{\alpha-1} e^{-x} dx$

**Properties**: 
- $\Gamma(\alpha) = (\alpha - 1)\Gamma(\alpha - 1)$
- $\Gamma(\alpha) = (\alpha - 1)! \quad$ (if $\alpha$ is an integer)

**Example**: Let $X \sim exp(1)$, then $f(x)=e^{-x}$. Find $E[X^3]=\int_0^\infty x^3 e^{-x}dx$
    To evaluate this integral, we can use the kernel function of gamma distribution. 
    $$\int_0^\infty \frac{x^{4-1} e^{-\frac{x}{1}}}{\Gamma(4)1^4} dx = 1 \Rightarrow \int_0^\infty x^3 e^{-x}dx = \int_0^\infty x^{4-1} e^{-\frac{x}{1}} = \Gamma(4) = 3! = 6$$

## Note on Independence 
Let $X$, $Y$ be random variables with joint pdf $f(x,y)$. Then $X$, $Y$ are independent if $f(x,y)=f(x)f(y)$ 

Note: To find the marginal pdf, use $f(x) = \int_y f(x,y)dy$ and $f(y) = \int_x f(x,y)dx$

### Theorem: 
Let $X$, $Y$ be independent random variables. Then $E[XY]=E[X]E[Y]$

<div style="border:2px solid black; padding:12px; margin:12px 0; border-radius:4px;">

**Proof**:

$XY$ is a function of $X$ and $Y$. Therefore, using the expectation of a function of $x$ and $y$ $E[g(x,y)] = \int_x \int_y g(x,y) f(x,y) dx dy$, we get:

$$
\begin{aligned}
E[XY] &= \int_x \int_y xy f(x,y) dx dy \\
&= \int_x \int_y xy f(x)f(y) dx dy \\
&= (\int_x x f(x) dx)( \int_y y f(y) dy) \\
&= E[X]E[Y]
\end{aligned}
$$

and $cov[X,Y]=E[XY] - E[X]E[Y] = 0$
</div>

### Corollary: 
Let $X$, $Y$ be independent random variables, and let $g(x)$ and $h(y)$ be functions of $x$ and $y$ alone respectively. Then $E[g(X)h(Y)] = E[g(X)]E[h(Y)]$

### Theorem: 
Let $g(x)$ be a function of $X$ alone and $h(y)$ be a function of $Y$ alone. Then $X$, $Y$ are independent iff $f(x,y)=g(x)h(y)$.

<div style="border:2px solid black; padding:12px; margin:12px 0; border-radius:4px;">

**Proof**:

Let $c = \int_{-\infty}^\infty g(x) dx$ and $d = \int_{-\infty}^\infty h(y) dy$. 

$$
cd = (\int_{-\infty}^\infty g(x) dx)(\int_{-\infty}^\infty h(y) dy) 
= \int_{-\infty}^\infty \int_{-\infty}^\infty g(x)h(y) dx dy 
= \int_{-\infty}^\infty \int_{-\infty}^\infty f(x,y) dx dy = 1
$$

Then marginal distribution of $X$ and $Y$ are given as 

$$f(x) = \int_{-\infty}^\infty f(x,y) dy = \int_{-\infty}^\infty g(x)h(y) dy = \int_{-\infty}^\infty h(y) dy \cdot g(x) = d \cdot g(x)$$

$$f(y) = \int_{-\infty}^\infty f(x,y) dx = \int_{-\infty}^\infty g(x)h(y) dx = \int_{-\infty}^\infty g(x) dx \cdot h(y) = c \cdot h(y)$$

Finally, 

$$f(x,y) = c \cdot d \cdot g(x)h(y) = f(x)f(y) \Rightarrow \text{X and Y are independent}$$

</div>


## Exponential families

A probability density function or probability mass function is called an exponential family if it can be expressed as 

$$f(x \mid \boldsymbol{\theta}) = h(x)c(\boldsymbol{\theta}) \exp\big(\sum_{i=1}^k w_i(\boldsymbol{\theta})t_i(x)\big)$$

Note: let $d$ be $dim(\boldsymbol{\theta})$. If $d=k$, we have full exponential family; 
if $d < k$, we have curved exponential family.

**Example**: Let $X \sim bin(n,p)$ with $n$ fixed. 

$$
\begin{aligned}
p(x) &= \binom{n}{x} p^x(1-p)^{n-x} \\
&= \binom{n}{x} \big(\frac{p}{1-p}\big)^x(1-p)^n \\
&= \binom{n}{x} (1-p)^n \exp{\big(\log{\big(\frac{p}{1-p}\big)^x}\big)} \\
&= \binom{n}{x} (1-p)^n \exp{\big(x\log{\big(\frac{p}{1-p}\big)}\big)} 
\end{aligned}
$$

Therefore, this pmf is an exponential family with

$h(x) = \binom{n}{x}, c(p) = (1-p)^n, t_1(x) = x, w_1(p) = \log{\big(\frac{p}{1-p}\big)}$

### Theorem: 
Suppose a random variable $X$ has a pdf or pmf that can be expressed in the form of exponential family. Then, 
- (a) $E\big[\sum_{i=1}^k \frac{\partial w_i(\boldsymbol{\theta})}{\partial \theta_j}t_i(x)\big] = -\frac{\partial}{\partial \theta_j}\log c(\boldsymbol{\theta})$
- (b) $var\big[\sum_{i=1}^k \frac{\partial w_i(\boldsymbol{\theta})}{\partial \theta_j}t_i(x)\big] = -\frac{\partial^2}{\partial \theta_j^2}\log c(\boldsymbol{\theta}) - E\big[\sum_{i=1}^k \frac{\partial^2 w_i(\boldsymbol{\theta})}{\partial \theta_j^2}t_i(x)\big]$


<div style="border:2px solid black; padding:12px; margin:12px 0; border-radius:4px;">

**Proof of (a)**:

$$
\begin{aligned}
\int_x f(x \mid \boldsymbol{\theta})dx &= 1 \\
\int_x h(x)c(\boldsymbol{\theta}) \exp\big(\sum_{i=1}^k w_i(\boldsymbol{\theta})t_i(x)\big) dx &= 1
\end{aligned}
$$

Differentiate both sides w.r.t. $\theta_j$:

$$
\begin{aligned}
&\int_x h(x) \frac{\partial c(\boldsymbol{\theta})}{\partial \theta_j} \exp\big(\sum_{i=1}^k w_i(\boldsymbol{\theta})t_i(x)\big) dx \\
+ &\int_x h(x)c(\boldsymbol{\theta}) \sum_{i=1}^k \frac{\partial w_i(\boldsymbol{\theta})}{\partial \theta_j}t_i(x)\exp\big(\sum_{i=1}^k w_i(\boldsymbol{\theta})t_i(x)\big) dx = 0
\end{aligned}
$$

Multiply the first integral by $\frac{c(\boldsymbol{\theta})}{c(\boldsymbol{\theta})}$ and note that $\frac{\partial \log c(\boldsymbol{\theta})}{\partial \theta_j} = \frac{\partial c(\boldsymbol{\theta})}{\partial \theta_j} \frac{1}{c(\boldsymbol{\theta})}$

$$
\begin{aligned}
&\int_x h(x) \frac{\partial c(\boldsymbol{\theta})}{\partial \theta_j} \exp\big(\sum_{i=1}^k w_i(\boldsymbol{\theta})t_i(x)\big) \frac{c(\boldsymbol{\theta})}{c(\boldsymbol{\theta})} dx \\
+ &\int_x h(x)c(\boldsymbol{\theta}) \sum_{i=1}^k \frac{\partial w_i(\boldsymbol{\theta})}{\partial \theta_j}t_i(x)\exp\big(\sum_{i=1}^k w_i(\boldsymbol{\theta})t_i(x)\big) dx = 0
\end{aligned}
$$

After rearranging we get

$$
\begin{aligned}
\int_x \sum_{i=1}^k \frac{\partial w_i(\boldsymbol{\theta})}{\partial \theta_j} t_i(x) h(x)c(\boldsymbol{\theta}) \exp\big(\sum_{i=1}^k w_i(\boldsymbol{\theta})t_i(x)\big) &dx = \\
- \frac{\partial \log c(\boldsymbol{\theta})}{\partial \theta_j} \int_x h(x)c(\boldsymbol{\theta}) \exp\big(\sum_{i=1}^k w_i(\boldsymbol{\theta})t_i(x)\big) &dx
\end{aligned}
$$

Or

$$E\big[ \sum_{i=1}^k \frac{\partial w_i(\boldsymbol{\theta})}{\partial \theta_j} t_i(x) \big] = - \frac{\partial}{\partial \theta_j} \log c(\boldsymbol{\theta})$$

To prove statement (b), differentiate a second time and rearrange

</div>

**Example**: Let $X \sim \text{Poission}(\lambda)$. Use the theorem above to show that $E[X] = \lambda$ and $var[X] = \lambda$ 

$$p(x)=\frac{\lambda^xe^{-\lambda}}{x!} = \frac{1}{x!}e^{-\lambda}e^{\log\lambda^x} = \frac{1}{x!}e^{-\lambda}e^{x\log\lambda}$$

So this pmf is an exponential family with 

$h(x) = \frac{1}{x!}, c(\lambda) = e^{-\lambda}, t_1(x) = x, w_1(\lambda) = \log\lambda$

$$E[\frac{1}{\lambda}X] = -(-1) \quad \Rightarrow \quad E[X] = \lambda$$

$$
var[\frac{1}{\lambda}X] = 0 - E[-\frac{1}{\lambda^2}X] \\
\quad \Rightarrow \quad \frac{1}{\lambda^2} var[X] = \frac{1}{\lambda^2} E[X] \\
\quad \Rightarrow \quad var[X] = E[X] = \lambda
$$


---


# Lec 3: Moment generating functions

### Definition (MGF)
$$
M_X(t) = E[e^{tX}]=
\begin{cases}
\displaystyle \sum_x e^{tx}p(x), & \text{if } X \text{ is discrete},\\[1.2em]
\displaystyle \int_x e^{tx}f(x) dx, & \text{if } X \text{ is continuous}.
\end{cases}
$$

Aside:

$$
e^x=1 + \frac{x}{1!} + \frac{x^2}{2!} + \frac{x^3}{3!} + \dots \qquad 
e^x=1 + \frac{tx}{1!} + \frac{(tx)^2}{2!} + \frac{(tx)^3}{3!} + \dots
$$

Let $X$ be a discrete random variable.

$$
\begin{aligned}
M_t = \sum_x e^{tX}p(x) &= \sum_x \big[ 1 + \frac{tx}{1!} + \frac{(tx)^2}{2!} + \frac{(tx)^3}{3!} + \dots \big] p(x) \\
&= \sum_x p(x) + \sum_x \frac{tx}{1!} p(x) + \sum_x \frac{(tx)^2}{2!} p(x) + \sum_x \frac{(tx)^3}{3!} p(x) + \dots
\end{aligned}
$$

To find the $k_{th}$ moment, simply evaluate the $k_{th}$ derivative of $M_X(t)$ at $t=0$.

$$E[X]^k = [M_X(t)]_{t=0}^{k_{th} \text{derivative}}$$

First moment: $M_X'(t) = \sum_x x p(x) + \sum_x \frac{2t}{2!}x^2 p(x) + \dots \qquad M_X'(0) = \sum_x x p(x) = E[X]$

Second moment: $M_X'(t) = \sum_x x^2 p(x) + \sum_x \frac{6t}{3!}x^3 p(x) + \dots \qquad M_X''(0) = \sum_x x^2 p(x) = E[X^2]$

Or from direct differentiation of MGF from the definition and evaluate at $t=0$.

$$
\begin{aligned}
M_X(t) &= E[E^{tX}] \quad \Rightarrow \quad M_X(0) = E[1] = 1 \\
M_X'(t) &= \frac{\partial M_X(t)}{\partial t} = E[Xe^{tX}] \quad \Rightarrow \quad M_X'(0) = E[X] \\
M_X''(t) &= \frac{\partial^2 M_X(t)}{\partial t^2} = E[X^2e^{tX}] \quad \Rightarrow \quad M_X''(0) = E[X^2]
\end{aligned}
$$

## Corollary 
Instead of differentiating $M_X(t)$, we can differentiate $\log[M_X(t)]$ and evaluate the first and second derivative at $t=0$. This will give $E[X]$ and $var[X]$.

$$
\begin{aligned}
\Psi(t) &= \log[M_X(t)] \\

\Psi'(t) &= \frac{M_X'(t)}{M_X(t)} \quad \Rightarrow \quad \frac{M_X'(0)}{M_X(0)} = E[X] \\

\Psi''(t) &= \frac{M_X''(t)M_X(t) - (M_X'(t))^2}{(M_X(t))^2} \quad \Rightarrow \quad \frac{E[X^2]\cdot 1 - E[X]^2}{1^2} = var[X]
\end{aligned}
$$

### MGF of binomial random variable

Let $X \sim bin(n,p)$

$$
\begin{aligned}
M_X(t) &= E[e^{tX}] \\
&= \sum_{x=0}^n e^{tx} \binom{n}{x} p^x(1-p)^{n-x} \\
&= \sum_{x=0}^n \binom{n}{x} (pe^t)^x(1-p)^{n-x} \qquad \text{applying binomial theorem} \\
&= (pe^t + 1 - p)^n
\end{aligned}
$$

### MGF of Poission random variable

Let $X \sim Poisson(\lambda)$

$$
\begin{aligned}
M_X(t) &= E[e^{tX}] \\
&= \sum_{x=0}^\infty e^{tx} \frac{\lambda^x e^{-\lambda}}{x!} \\
&= e^{-\lambda} \sum_{x=0}^\infty \frac{(\lambda e^{t})^x}{x!} \\
&= e^{-\lambda} e^{\lambda e^t} \\
&= e^{\lambda(e^t - 1)}
\end{aligned}
$$

### MGF of gamma random variable

Let $X \sim \Gamma(\alpha, \beta)$

$$
\begin{aligned}
M_X(t) &= E[e^{tX}] \\
&= \int_{x=0}^\infty e^{tx} \frac{x^{\alpha-1} e^{-\frac{x}{\beta}}}{\Gamma(\alpha)\beta^\alpha} dx \\
&= \int_{x=0}^\infty \frac{x^{\alpha-1} e^{-x (\frac{1}{\beta} - t)}}{\Gamma(\alpha)\beta^\alpha} dx \\
&= \frac{\beta^{*\alpha}}{\beta^\alpha} \int_{x=0}^\infty \frac{x^{\alpha-1} e^{-\frac{x}{\beta^*}}}{\Gamma(\alpha)\beta^{*\alpha}} dx \qquad \text{where } \beta^* = \frac{\beta}{1 - \beta t} \\
&= \frac{\beta^{*\alpha}}{\beta^\alpha} \\
&= (1-\beta t)^{-\alpha} \\
\end{aligned}
$$

### MGF of exponential random variable

Let $X \sim exp(\lambda)$. The exponential distribution is a special case of $\Gamma(\alpha, \beta)$ with $\alpha = 1$ and $\beta = \frac{1}{\lambda}$, therefore, $M_X(t) = (1-\frac{t}{\lambda})^{-1}$

### MGF of standard normal random variable

Let $Z \sim N(0,1)$

$$
\begin{aligned}
M_Z(t) &= E[e^{tZ}] \\
&= \int_{-\infty}^\infty e^{tz} \frac{1}{\sqrt{2\pi}} e^{-\frac{z^2}{2}} dz \\
&= e^{\frac{t^2}{2}} \int_{-\infty}^\infty \frac{1}{\sqrt{2\pi}} e^{-\frac{(z-t)^2}{2}} dz \\
&= e^{\frac{t^2}{2}}
\end{aligned}
$$

## Theorem 
Let $X$, $Y$ be independent random vairables with moment generating functions $M_X(t)$, $M_Y(t)$ respectively. Then, the moment generating function of the sum of these two random variables is equal to the product of the individual moment generating functions:

$$M_{X+Y}(t) = M_X(t)M_Y(t)$$

**Proof**: $M_{X+Y}(t) = E[e^{t(X+Y)}] = E[e^{tX} \cdot e^{tY}] \overset{\text{indep}}{=} E[e^{tX}]E[e^{tY}] = M_X(t)M_Y(t)$ 

### Example 

$$X \sim bin(n_1, p), Y \sim bin(n_2, p), X \perp\!\!\!\perp Y$$

$$M_{X+Y}(t) = M_X(t)M_Y(t) = (pe^t + 1 - p)^{n_1 + n_2}$$

$$X + Y \sim bin(n_1 + n_2, p)$$

### Example 

$$X \sim \text{Poission}(\lambda_1), Y \sim \text{Poission}(\lambda_2), X \perp\!\!\!\perp Y$$

$$M_{X+Y}(t) = M_X(t)M_Y(t) = e^{(\lambda_1 + \lambda_2)(e^t - 1)}$$

$$X + Y \sim \text{Poission}(\lambda_1 + \lambda_2)$$

## Properties of moment generating functions
Let $X$ be a random variable with moment genearting function $M_X(t) = E[e^{tX}]$, and $a$, $b$ are constants
- $M_{X+a}(t) = E[e^{t(X+a)}] = e^{ta} E[e^{tX}] = e^{at}M_X(t)$
- $M_{bX}(t) = E[e^{tbX}] = M_X(bt)$
- $M_{\frac{X+a}{b}}(t) = e^{\frac{a}{b}t} E[e^{\frac{t}{b}X}] = e^{\frac{a}{b}t} M_X(\frac{t}{b})$

### Example - MGF of normal random variable

Use the moment generating function of $Z \sim N(0,1)$ to find the moment generating function of $X \sim N(\mu, \sigma)$

$$Z = \frac{X - \mu}{\sigma} \quad \Rightarrow \quad X = \mu + \sigma Z$$

$$
\begin{aligned}
M_X(t) = M_{\mu + \sigma Z}(t) &= E[e^{t(\mu + \sigma Z)}] \\
&= e^{t\mu} E[e^t\sigma Z] \\
&= e^{t\mu} M_Z(\sigma t) \\
&= e^{t\mu} e^{\frac{1}{2}t^2\sigma^2} \\
&= e^{t\mu + \frac{1}{2}t^2\sigma^2}
\end{aligned}
$$

### Example 
Suppose $X$, $Y$ are indepedent random variables. Find the distribution of $X+Y$, where $X \sim N(\mu_1, \sigma_1), Y \sim N(\mu_2, \sigma_2)$

$$M_{X+Y}(t) = M_X(t)M_Y(t) = e^{t\mu_1 + \frac{1}{2}t^2\sigma_1^2} e^{t\mu_2 + \frac{1}{2}t^2\sigma_2^2} = e^{t(\mu_1 + \mu_2) + \frac{1}{2}t^2(\sigma_1^2 + \sigma_2^2)}$$

$$E[X+Y] = \mu_1 + \mu_2 \qquad var[X+Y] = \sigma_1^2 + \sigma_2^2$$

$$X+Y \sim N(\mu_1 + \mu_2, \sigma_1^2 + \sigma_2^2)$$

### Example
Let $X_1, X_2, \dots, X_n \overset{\text{i.i.d.}}{~} N(\mu, \sigma)$. Use moment generating functions to find the distribution.

#### (a). $T = X_1 + X_2 + \dots + X_n$

$$
\begin{aligned}
M_T(t) = M_{X_1 + X_2 + \dots + X_n}(t) 
&= M_{X_1}(t)M_{X_2}(t) \dots M_{X_n}(t) \\
&= \big( M_{X_i}(t) \big)^n \\
&= \big( e^{t\mu + \frac{1}{2}t^2\sigma^2} \big)^n \\
&= e^{tn\mu + \frac{1}{2}t^2n\sigma^2} \\
\end{aligned}
$$

$$T \sim N(n\mu, n\sigma^2)$$


#### (b). $\bar{X} = \frac{\sum_{i=1}^n X_i}{n}$

$$M_\bar{X}(t) = M_{\frac{T}{n}}(t) = M_T(\frac{t}{n}) = e^{t\mu + \frac{1}{2}t^2\frac{\sigma^2}{n}}$$

$$\bar{X} \sim N(\mu, \frac{\sigma^2}{n})$$


---


# Lec 4: Functions of random variables 

## Method of cdf

### Example 
Let $X \sim \Gamma(\alpha, \beta)$. Find the distribution of $Y = cX, c > 0$, with method of cdf.

$$
\begin{aligned}
F_Y(y) &= P(Y \leq y) \\
&= P(cX \leq y) \\
&= P(X \leq \frac{y}{c}) \\
&= F_X(\frac{y}{c}) \\
f_Y(y) &= \frac{1}{c} f_X(\frac{y}{c}) \\
&= \frac{1}{c} \frac{\frac{y}{c}^{\alpha-1} e^{-\frac{y}{c\beta}}}{\Gamma(\alpha)\beta^\alpha} \\
&= \frac{y^{\alpha-1} e^{-\frac{y}{c\beta}}}{\Gamma(\alpha)(c\beta)^\alpha}
\end{aligned}
$$

Therefore, $Y \sim \Gamma(\alpha, c\beta)$

### Example
Let $Z \sim N(0,1)$. Find the pdf of $X = Z^2$.

$$
\begin{aligned}
F_X(x) &= P(X \leq x) \\
&= P(Z^2 \leq x) \\
&= P(-\sqrt{x} \leq Z \leq \sqrt{x}) \\
&= P(Z \leq \sqrt{x}) - P(Z \leq -\sqrt{x}) \\
&= F_Z{\sqrt{x}} - F_Z{-\sqrt{x}} \\
f_X(x) &= \frac{1}{2}x^{-\frac{1}{2}}f_Z(\sqrt{x}) + \frac{1}{2}x^{-\frac{1}{2}}f_Z(-\sqrt{x}) \\
&= \frac{1}{2}x^{-\frac{1}{2}} \frac{1}{\sqrt{2\pi}}e^{-\frac{x}{2}} + \frac{1}{2}x^{-\frac{1}{2}}\frac{1}{\sqrt{2\pi}}e^{-\frac{x}{2}} \\
&= \frac{x^{-\frac{1}{2}}e^{-\frac{x}{2}}}{\sqrt{2\pi}} \\
&= \frac{x^{-\frac{1}{2}}e^{-\frac{x}{2}}}{\Gamma{\frac{1}{2}}2^{\frac{1}{2}}}
\end{aligned}
$$

Therefore, $Z^2 \sim \Gamma(\frac{1}{2}, 2) = \chi^2_1$

## Method of transformation

Let $X$ be a continuous random variable with pdf $f(x)$. Let $Y=g(X)$ be a monotone function (either increasing or decreasing), then there is an one-to-one transformation. The pdf of Y(X) is given by 

$$f_Y(y) = f_X[g^{-1}(y)]\big| \frac{d}{dy}g^{-1}(y) \big|$$

<div style="border:2px solid black; padding:12px; margin:12px 0; border-radius:4px;">

**Proof**:

**Case 1: decreasing function**

$$
\begin{aligned}
F_Y(y) &= P(Y \leq y) \\
&= P(g(x) \leq y) \\
&= P(X \leq g^{-1}(y)) \\
&= F_X(g^{-1}(y)) \\
f_Y(y) &= \frac{\partial}{\partial y}F_Y(y) \\
&= \frac{\partial}{\partial y}F_X(g^{-1}(y)) \\
&= f_X(g^{-1}(y)) \frac{d}{dy}g^{-1}(y) \quad (\text{positive} \cdot \text{positive})
\end{aligned}
$$

**Case 2: increasing function**

$$
\begin{aligned}
F_Y(y) &= P(Y \leq y) \\
&= P(g(x) \leq y) \\
&= P(X > g^{-1}(y)) \\
&= 1 - F_X(g^{-1}(y)) \\
f_Y(y) &= \frac{\partial}{\partial y}F_Y(y) \\
&= \frac{\partial}{\partial y}(1 - F_X(g^{-1}(y))) \\
&= - f_X(g^{-1}(y)) \frac{d}{dy}g^{-1}(y) \quad (-\text{positive} \cdot \text{negative})
\end{aligned}
$$

Therefore $f_Y(y) = f_X[g^{-1}(y)]\big| \frac{d}{dy}g^{-1}(y) \big|$

**Results**

Let $X$ be a continuous random variable and let $Y=g(X)$
- If $g(X)$ is increasing, then $F_Y(y) = F_X[g^{-1}(y)]$
- If $g(X)$ is decreasing, then $F_Y(y) = 1 - F_X[g^{-1}(y)]$

</div>

### Example 
Let $X \sim \Gamma(\alpha, \beta)$. Find the distribution of $Y = cX, c > 0$, with method of transformation.

$$
\begin{aligned}
g(x) &= cX \quad \Rightarrow \quad g^{-1}(y)=\frac{y}{c} \\
f_Y(y) &= f_X[g^{-1}(y)]\big| \frac{d}{dy}g^{-1}(y) \big| \\
&= f_x(\frac{y}{c})\cdot \frac{1}{c} \\
&= \frac{y^{\alpha-1} e^{-\frac{y}{c\beta}}}{\Gamma(\alpha)(c\beta)^\alpha}
\end{aligned}
$$

### Example 
Let $X \sim unif(0,1)$, $Y = ln(X)$

$$f(x)=1, \quad F(x)=x$$
$$y = g(x) = \log(y) \quad \Rightarrow \quad g^{-1}(y)=e^{-y}$$
$$f_Y(y) = f_X(e^{-y}) \big| \frac{d}{dy}e^{-y}\big| = |-e^{-y}| = e^{-y}$$

Therefore, $Y \sim exp(1)$