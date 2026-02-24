# Carl Friedrich Gauss (1777-1855)  
**The Prince of Mathematics**

Carl Fridrich Gauss was a German mathematican well known for his extraordinary work in mathematical science. He had contributed to many fields in mathematics such as analysis, algebra, geometry, number theory, and statistics. One of his most crucial discovery in statistics is the Gaussian curve - Normal Distribution. He also developed ordinary least square regression

### **Heptadecagon**  
Another big accomplishment concerns his construction of the regular heptadecagon (17-gon): At the age of 19 (1796), Gauss proved that a regular 17-sided polygon can be constructed using just a compass and a straightedge in a single night. Classical Euclid geometry had already established construction of $2^k*3$ -gon, $2^k*4$ -gon, $2^k*5$ -gon, as well as any products formed from those in 4th century BC. However, the construction of other regular polygons with a prime number of sides, such as the 11-gon, 13-gon, and 17-gon, remained unknown. It was later proven that the regular 11-gon and 13-gon are not constructible.

The complete proof can be found <here:link>

### **Theory**  
Trigonometrically, construction of 17-gon relies on algebraically deriving the value of $cos(\frac{2\pi}{17})$ or $sin(\frac{2\pi}{17})$. Gauss discovers that regular polygons with Fermat number of sides can be sketched ($p$ is a Fermat number if $p = 2^{2^n} + 1$ for $n \in \mathbf{Z}$). That mean equilateral triangle, regular pentagon, and even 257-gon 65537-gon are constructible.

### **Proof**  
There might be other proofs, but this proof is basic on the intuition from this source:  
https://ocw.mit.edu/courses/18-702-algebra-ii-spring-2011/4ebf8b1ca041c015dd8fd86fd19c4198_MIT18_702S11_seventeengon5.pdf

$$\cos(\frac{2\pi}{17}) = \frac{\sqrt{17}-1+\sqrt{34-2\sqrt{17}} + 2\sqrt{17+3\sqrt{17}-\sqrt{34-2\sqrt{17}}-2\sqrt{34+2\sqrt{17}}}}{16}$$

We will use some field theory concepts and the complex plane.  

#### STEP 1  
**We want to show:** $\cos(\frac{2\pi}{17})=\frac{\zeta + \zeta^{-1}}{2}$  
Let $\theta = \frac{2\pi}{17}$ $\zeta = e^{\theta i} = e^{\frac{2\pi}{17}i}$. Then $\zeta$ is a primitive 17th root of unity, i.e. $\zeta^17=1$.  
Since 17 is a prime, then the 17th cyclotomic polynomial is $\Phi_17(x)=1+x+x^2+\dots+x^16$