# Carl Friedrich Gauss (1777-1855)  
**The Prince of Mathematics**

Carl Fridrich Gauss was a German mathematican well known for his extraordinary work in mathematical science. He had contributed to many fields in mathematics such as analysis, algebra, geometry, number theory, and statistics. One of his most crucial discovery in statistics is the Gaussian curve - Normal Distribution. He also developed ordinary least square regression

---

## **Heptadecagon**  
Another big accomplishment concerns his construction of the regular heptadecagon (17-gon): At the age of 19 (1796), Gauss proved that a regular 17-sided polygon can be constructed using just a compass and a straightedge in a single night. Classical Euclid geometry had already established construction of $2^{k}*3$ -gon, $2^{k}*4$ -gon, $2^{k}*5$ -gon, as well as any products formed from those in 4th century BC. However, the construction of other regular polygons with a prime number of sides, such as the 11-gon, 13-gon, and 17-gon, remained unknown. It was later proven that the regular 11-gon and 13-gon are not constructible.


### **Theory**  
Trigonometrically, construction of 17-gon relies on algebraically deriving the value of $\cos(\frac{2\pi}{17})$ or $\sin(\frac{2\pi}{17})$. Gauss discovers that regular polygons with Fermat number of sides can be sketched ($p$ is a Fermat number if $p = 2^{2^n} + 1$ for $n \in \mathbb{Z}$). That mean equilateral triangle, regular pentagon, and even 257-gon 65537-gon are constructible.

squarerooting is supported by...

---

### **SOME REQUIRED IDEAS**

**$\textbf{ALGEBRA AND NUMBER THEORY}$**

- **Complex Number:** 
Def: $i :=\sqrt{-1}$; Euler's formula: $e^{i\theta}=\cos(\theta)+i\sin(\theta)$; complex conjugate $\overline{a+bi}:=a-bi$

- **Root of Unity:** 
Def: An n-th root of unity is a complex number satisfying $z^n=1$; primitive root of unity: $\zeta=e^{\frac{2\pi i}{n}}$

- **Modular Arithmetics (Congruence):** 
Def: $a \equiv b$ (mod n) indicates 17 divides $(a-b)$

- **Multiplicative Group Mod Prime:** 
$(\mathbb{Z}/n\mathbb{Z})^\times={1,2,\dots,n-1}$ under multiplication of mod n. Facts: it has n-1 elements, it is cyclic, and every elements has a multiplicative inverse.

- **Quadratic Residues:** 
Def: An integer $a$ is a quadratic residue mod n if $x^2\equiv a$ (mod n) has an integer solution. For prime 17, it has exactly 8 quad residues and 8 non-quad residues. This fact will be used for first splitting.

**$\textbf{POLYNOMIAL ALGEBRA}$**

- **Minimal Polynomial:** 
Def: The minimal polynomial: $\Phi_{n}(x)=1+x+x^2+\dots+x^{n-1}$ of $\zeta$ over $\mathbb{Q}$ is the monic polynomial of smallest degree in $\mathbb{Q}[x]$ that has $\zeta$ as a root.

- **Degree of Algebraic Number:**
Def: $[\mathbb{Q}(\zeta) : \mathbb{Q}] = $ degree of minimal polynomial.

**$\textbf{LIGHT GALOIS THEORY}$**

- **Field:** 
Def: A field is a set of number closed under addition and multiplication (except by 0).

- **Field Extension:** 
Def: A field extension $\mathbb{Q}(\zeta)$ is all rational combinations of powers of $\zeta$

- **Automorphism:** 
Def: A field automorphism is a bijective map $T: F \rightarrow F$ that preserves addition and multiplcaition.

- **Fixed Field:** 
Def: A fixed field is the set of elements unchanged by an automorphism (example: complex conjugates)

- **Cyclic Group:** 
Def: A cyclic group is the group where every element is a power of one generator. Fact: $(\mathbb{Z}/n\mathbb{Z})^\times$ is cyclic of order (n-1).

- **Subgroup of index 2:** 
Quadratic residues form a subgroup of size 8. This is why the first splitting is quadratic.

---


### **Proof**  
There might be other proofs, but this proof is basic on the intuition from this source:  
https://ocw.mit.edu/courses/18-702-algebra-ii-spring-2011/4ebf8b1ca041c015dd8fd86fd19c4198_MIT18_702S11_seventeengon5.pdf

$$\cos(\frac{2\pi}{17}) = \frac{\sqrt{17}-1+\sqrt{34-2\sqrt{17}} + 2\sqrt{17+3\sqrt{17}-\sqrt{34-2\sqrt{17}}-2\sqrt{34+2\sqrt{17}}}}{16}$$



#### **STEP 1**  

Let $\theta = \frac{2\pi}{17}$ $\zeta = e^{\theta i}$, want to show: $\cos(\frac{2\pi}{17})=\frac{\zeta + \zeta^{-1}}{2}$ and $[\mathbb{Q}(\zeta + \zeta^{-1}) : \mathbb{Q}] = 8$

By Euler's formula, we naturally obtained the formula for $\cos(\theta)$ and $\sin(\theta)$, so $\frac{\zeta+\zeta^{-1}}{2}=\frac{e^{\theta i} + e^{-\theta i}}{2}=\cos(\theta)=\cos(\frac{2\pi}{17})$

Since 17 is a prime, then the 17th cyclotomic polynomial is $\Phi_{17}(x)=1+x+x^2+\dots+x^{16}$ and $1+\zeta+\zeta^2+\dots+\zeta^{16}=0$. This polynomial is irreducible over $\mathbb{Q}$ and $\zeta$ is a primitive 17th root of unity, i.e. $\zeta^{17}=1$. Then $[\mathbb{Q}(\zeta) : \mathbb{Q}] = \text{deg} \Phi_{17}=\phi(17)=16$. $\zeta$ and $\zeta^{-1}$ are complex conjugates in $\mathbb{Q}(\zeta)$. Since complex conjugation is an automorphism of order 2, its fixed field has index 2 and $[\mathbb{Q}(\zeta + \zeta^{-1}) : \mathbb{Q}] = \frac{16}{2}= 8$


#### **STEP 2**

17 has quadratic residues set: $R=\{1,2,4,8,9,13,15,16\}$ and non-quadratic residues set: $N=\{3,5,6,7,10,11,12,14\}$. 

Define $\alpha = \sum_{r\in R}\zeta^r$ and $\beta = \sum_{n\in N}\zeta^n$. Since $1+\sum_{k=1}^{16}\zeta^k=0$, $\alpha+\beta=-1$. 

To evaluate $\alpha\beta=\sum_{r\in R}\sum_{n\in N}\zeta^{r+n}$ is the same as counting how many times each power $\zeta^k$ for $k \in {1,2,\dots, 16}$ appears and fix $k \not\equiv 0$ (mod 17). The number of solutions to $r+n\equiv k$ (mod 17) $(r \in R,\ n \in N)$ is 4. Therefore $\alpha\beta=4\sum_{k=1}^{16}\zeta^k$, but $1+\sum_{k=1}^{16}\zeta^k=0$, so $\alpha\beta=-4$.

Then substitute $\alpha+\beta=-1$ and $\alpha\beta=-4$ as coefficients into the quadratic equation $x^2-(\alpha+\beta)x + \alpha\beta=(x-\alpha)(x-\beta)=0$. We have $x^2+x-4=0$ and the solution is $x=\frac{-1\pm \sqrt{17}}{2}$. Therefore $\alpha = \frac{-1+\sqrt{17}}{2}$ and $\beta = \frac{-1- \sqrt{17}}{2}$.

This step has shown $\sqrt{17} \in \mathbb{Q}(\zeta)$ and reduced the degree-8 problem into degree-4.

#### **STEP 3**

The multiplicative group $(\mathbb{Z}/17\mathbb{Z})^\times$ is cyclic of order 16. Its generator gives all 16 elements. Notice that R composes of precisely the even powers of 3: $R=\{3^0,3^2,3^4,3^6,3^8,3^10,3^12,3^14\}$ (mod 17). This form a cyclic subgroup of order 8. Inside this subgroup, the element $3^4$ has order 2. So the subgroup of residues itself has a subgroup of index 2 and that subgroup has 4 elements.

Define $R_1 = \{1,4,16,13\}$ and $R_2 = \{2,8,15,9\}$. Let $\gamma = \sum_{k \in R_1}\zeta^k$ and $\delta = \sum_{k \in R_2}\zeta^k$, then $\gamma + \delta = \alpha=\frac{-1+\sqrt{17}}{2}$. To compute $\gamma\delta=\sum_{a \in R_1}\sum_{b \in R_2}\zeta^{a+b}$, Each exponent in $R$ appears exactly twice and each exponent in $N$ appears exactly once. After summing and simplifying using $1+\sum_{k=1}^{16}\zeta^k=0$, obtains $\gamma\delta = \frac{-1-\sqrt{17}}{4}$

$\gamma$ and $\delta$ satisfy $x^2-(\gamma+\delta)x+\gamma\delta=(x-\gamma)(x-\delta)=0$. the solution is $x\frac{-1+\sqrt{17}\pm \sqrt{34-2\sqrt{17}}}{4}$. So $\gamma=x\frac{-1+\sqrt{17}+\sqrt{34-2\sqrt{17}}}{4}$ and $\delta=x\frac{-1+\sqrt{17}-\sqrt{34-2\sqrt{17}}}{4}$

Now this has been reduced to degree 2.

#### **STEP 4**
