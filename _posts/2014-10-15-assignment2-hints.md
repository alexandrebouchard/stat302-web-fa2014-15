---
layout: post
title: "Assignment 2 - mathematical prerequisites"
---

The following facts should be combined together to form a solution to problem 3 on the assignment. Specific hints follow the list of mathematical prerequisites. 

--------------------
**Exponent laws**

Recall the following properties of exponents: 
$$
\begin{eqnarray}
 x^mx^n &=& x^{n+m} \\\\
 \left(x^m\right)^m &=& x^{nm}
 \end{eqnarray}
$$

for additional exponent rules see [Wolfram's Exponent Laws page](http://mathworld.wolfram.com/ExponentLaws.html). 

--------------------
The **geometric series**
\\begin{eqnarray}
\sum_{n=0}^\infty ar^n &=& a + ar + ar^2 + \cdots
\\end{eqnarray}


is convergent if $|r|<1$ and its sum is given as: 


\\begin{eqnarray}
\sum_{n=0}^\infty ar^n &=& \frac{a}{1-r}, \ \ \ |r| < 1 
\\end{eqnarray}

If $|r|\geq 1$, the geometric series is divergent. 

--------------------
**Factoring**

Consider a series with coefficients $c\_n$ for $n=1,2,\ldots$

\\begin{eqnarray}
\sum\_{n=0}^\infty c\_n r^n = c\_0 + c\_1 r + c\_2 r^2 + \cdots
\\end{eqnarray}

if the $c\_n$s take finitely many values then the sum can be rewritten by grouping all terms with common $c\_n$. For example, suppose that for all even numbered $n$ $c\_n$ is 1 and for all odd numbered $n$ $c\_n$ is 2. Then this infinite sum can be rewritten as: 

\\begin{eqnarray}
1 + 2 \cdot r + 1 \cdot r^2 + 2 \cdot r^3 +\cdots&=& 1\cdot(1 + r^2 + r^4 + \cdots) + 2 \cdot(r + r^3 + r^5 + \cdots) \\\\
&=& \sum\_{k=0}^\infty 1 r^{2k} + \sum\_{k=0}^\infty 2 r^{2k+1}
\\end{eqnarray}

--------------------
**Trigonometric functions**

The function $\sin(x)$ is periodic with period $2\pi$. That means that $\sin(x + 2k\pi) = \sin(x)$ for all integers $k$. The same holds for $\cos(x)$. 

--------------------
**Taylor Series for the Exponential function**

The exponential function $e^x$ can be represented as the sum: 
\\begin{eqnarray}
e^x &=& \sum\_{n=0}^\infty \frac{x^n}{n!} = 1 + \frac{x}{1!} + \frac{x^2}{2!} + \cdots
\\end{eqnarray}
for all $x\in\mathbb{R}$. 

--------------------
**Question 3.2** 

Use the property that $E(g(X)) = \sum\_{x \in S\_X} g(x) P(X = x)$ where $g(X) = \sin(X)$. Later in the question you should formally calculate the pmf of $Y= \sin(X)$ and from the definition of expectation, $E(Y) = \sum\_{y\in S\_Y} y P(Y=y)$, show that these quantities are equivalent. (NB: $S\_X$ and $S\_Y$ denote the support of $X$ and $Y$, respectively)

--------------------
**Question 4.1**

The problem states that the number of mutations per replication, say $X$, is Poisson distributed. That is, the random variable $X\sim\mathrm{Poi}(\lambda)$. The question also states that "...it is discovered that in $74\%$ of the cases where the virus replicates, the replicate contains at least one mutation". Together these two facts can be used to determine $\lambda$. In particular, what does "the replicate contains at least one mutation" imply?

--------------------
**Question 4.2**

Do not use linearity to calculate the expected value. Instead, use the definition of expectation. 

--------------------