---
layout: post
title: "Distributions - Quick reference"
category: 'Tool'
---

- [Discrete distributions](#discrete)
- [Continuous distributions](#continuous)


### Discrete distributions <a name="discrete"></a>

**Bernoulli:**

- Parameter: a *success probability* $0\leq p \leq 1$
- Notation in JAGS: ``X ~ dbern(p)``
- Mathematical notation: $X \sim \textrm{Bern}(p)$, or $X \sim \textrm{Bernoulli}(p)$
- PMF: $\P(X = 1) = p, \;\;\;\;\P(X = 0) = 1-p$
- Mean: $p$
- Variance: $p(1-p)$
- Relationship(s) to other random variables: $\textrm{Bern}(p) = \textrm{Bin}(1, p)$

**Binomial:**

- Parameters: a *success probability* $0 \leq p \leq 1$, and a *number of trials* $n \in \mathbb{N}$
- Notation in JAGS: ``X ~ dbin(p, n)`` **Note:** $p$ and $n$ have a different order in JAGS and the textbook.
- Mathematical notation: $X \sim \textrm{Bin}(n, p)$, or  $X \sim \textrm{Binomial}(n, p)$
- PMF: $\P(X = i) = \binom{n}{i} p^i (1-p)^{n-i}$, for $i \in \\{0, 1, \dots, n\\}$
- Mean: $np$
- Variance: $np(1-p)$
- Relationship(s) to other random variables:
	* If $X \sim \textrm{Bin}(n, p)$, then $X$ can be written as the sum of Bernoulli random variables: $X = X\_1 + X\_2 + \ldots + X\_n$ where $X\_i\sim \textrm{Bern}(p)$ for $i=1,2, \ldots, n$
	* For large $n$ (>100), small $p$ (<0.01), and intermediate $\lambda = np$ (<20), the Binomial distribution is approximated by the Poisson distribution with parameter $\lambda$.  

**Categorical:**

- Parameter: a set of possible values $\{1, 2, \dots, n\}$, and a normalized vector of probabilities $p = (p\_1, \dots, p\_n)$ giving the probability of each value in the set
- Notation in JAGS: ``X ~ dcat(p)``
- Mathematical notation: $X \sim \textrm{Cat}(p)$, or $X \sim \textrm{Categorical}(p)$
- PMF: $\P(X = i) = p\_i$, for $i \in \\{1, 2, \dots, n\\}$

**Poisson:** 

- Parameter: a *rate* $\lambda=$lambda > 0
- Notation in JAGS: ``X ~ dpois(lambda)``
- Mathematical notation: $X \sim \textrm{Poi}(\lambda)$ or $X \sim \textrm{Poisson}(\lambda)$
- PMF: $\P(X = i) = e^{-\lambda} \lambda^i / i!$, for $i \in \\{0, 1, 2, 3, \dots\\}$
- Mean: $\lambda$
- Variance: $\lambda$
- Relationship(s) to other random variables: As an approximation for the Binomial distribution (see above). 

**Discrete uniform:** 

- Parameter: a set of possible values $\{1, 2, \dots, n\}$
- Notation in JAGS: use a categorical distribution, setting equal probability to each value
- Mathematical notation: $X \sim \textrm{Unif}(\\{1, 2, \dots, n\\})$
- PMF: $\P(X = i) = 1/n$, for $i \in \\{1, 2, \dots, n\\}$
- Relationship(s) to other random variables: Categorical distribution with each value having the same probability. 

**Geometric**

- Parameter: a probability of success, $0 \leq p \leq 1$
- Notation in JAGS: ```Y ~ dnegbin(p, 1) ``` then $X = Y + 1$ is geometrically distributed with parameter $p$
- Mathematical notation: $X \sim \textrm{Geometric}(p)$
- PMF: $P(X = i) = (1-p)^{i-1}p$, for $i \in \\{1, 2, 3, \ldots \\}$
- Mean: $\frac1p$
- Variance: $\frac{1-p}{p^2}$
- Relationship(s) to other random variables:  $\textrm{Geometric}(p) = \textrm{NB}(p, r = 1)$

**Negative Binomial**

- Parameters: a probability of success, $0 \leq p \leq 1$, and a number of successes, $r\in \mathbb{N}$
- Notation in JAGS: ```Y ~ dnegbin(p, r)``` then $X = Y + r$ has the negative binomial distribution with parameters $r$ and $p$  
- Mathematical notation: $X \sim \textrm{NB}(p, r)$
- PMF: $P(X = i) = \binom{i-1}{r-1}(1-p)^{i-r}p^r$, for $i \in \\{r, r + 1, r + 2, r + 3, \ldots\\}$
- Mean: $\frac{r}{p}$
- Variance: $\frac{r(1-p)}{p^2}$
- Relationship(s) to other random variables:  $\textrm{Geometric}(p) = \textrm{NB}(p, r = 1)$


**Hypergeometric** 

- Parameters: Population size $N$ with $m$ successes and $n$ draws without replacement ($0 \leq n, m \leq N$). 
- Notation in JAGS: Not implemented. 
- Mathematical notation: $X \sim \textrm{HyperGeometric}(N, n, m)$
- PMF: $P(X = i) = \frac{\binom{m}{i}\binom{N-m}{n-i}}{\binom{N}{n}},$ for $i\in \\{\textrm{max}(0,m+n - N),\ldots, \textrm{min}(n,m)\\}$.
- Mean: $\frac{nm}{N}$
- Variance: $\frac{nm}{N}\left(1-\frac{m}{N}\right)\left(\frac{N-n}{N-1}\right)$
- Relationship(s) to other random variables: If $n=1$ the the hypergeometric is equivalent to a Bernoulli distribution with parameter $p = \frac{m}{N}$. 


### Continuous distributions <a name="continuous"></a>

**Uniform**

- Parameters: An interval $(a,b)$ specified by two parameters $-\infty < a < b < \infty$.
- Notation in JAGS: ``X ~ dunif(a, b)``.
- Mathematical notation: $X \sim \textrm{Unif}(a, b)$.
- PDF: $f(x) = \frac{1}{b-a},$ for $x\in (a,b)$, and $0$ otherwise.
- Mean: $\frac{a + b}{2}$
- Variance: $\frac{(a + b)^2}{12}$

**Normal**

- Parameters: mean $\mu\in \RR$ and variance $\sigma^2\in \mathbb{R}\_{>0}$.
- Notation in JAGS: ``X ~ dnorm(mu,1/sigma^2)``.
- Mathematical notation: $X \sim \textrm{N}(\mu, \sigma^2)$
- PDF: $f(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{ -(x-\mu)^2/2\sigma^2},$ for $x\in \RR$.
- Mean: $\mu$
- Variance: $\sigma^2$
- Relationship(s) to other random variables: (1) If $X\_1,\ldots,X\_n\simiid \textrm{N}(\mu, \sigma)$ then $S\_n = \sum\_{i=1}^n X\_i \sim \textrm{N}(n\mu, n\sigma^2)$; (2) Normal approximation for binomially distribution random variables: for large $n$ if $X\sim\mathrm{Binomial}(n,p)$ then $\frac{X - np}{\sqrt{np(1-p)}}$ is approximately distributed as $\mathrm{N}(0,1)$; (3) If $X\_1,\ldots,X\_n$ are independent and identically distributed with finite mean $\mu$ and variance $\sigma^2$ then $S\_n = \sum\_{i=1}^n X\_i \approx n\mu + \sigma\sqrt{n} Z$ where $Z\sim\mathrm{N}(0,1)$. 

**Exponential**

- Parameters: rate parameter $\lambda>0$. 
- Notation in JAGS: ``X ~ dexp(lambda)``.
- Mathematical notation: $X \sim \textrm{Exp}(\lambda)$.
- PDF: $f(x) = \lambda \exp(-\lambda x),$ for $x>0$, $0$ otherwise.
- Mean: $\frac{1}{\lambda}$
- Variance: $\frac{1}{\lambda^2}$
- Relationship(s) to other random variables: (1) If $X\_1,\ldots,X\_n\simiid \textrm{Exp}(\lambda)$ then $S\_n = \sum\_{i=1}^n X\_i \sim \textrm{Gamma}(\mbox{shape} = n, \mbox{rate} = \lambda)$; (2) The waiting time between occurrences in a Poisson Process with intensity parameter $\lambda$ is exponentially distributed with parameter $\lambda$. 

**Gamma**

- Parameters: Shape parameter $\alpha>0$ and rate parameter $\lambda>0$. 
- Notation in JAGS: ``X ~ dgamma(alpha, lambda)``. 
- Mathematical notation: $X \sim \textrm{Gamma}(\alpha, \lambda)$
- PDF: $f(x) = \frac{\lambda \exp(-\lambda x)(\lambda x)^{\alpha-1}}{\Gamma(\alpha)}$, for $x>0$, $0$ otherwise. 
- Mean: $\frac{\alpha}{\lambda}$
- Variance: $\frac{\alpha}{\lambda^2}$
- Relationship(s) to other random variables: (1) If $X\_1,\ldots,X\_n\simiid \textrm{Exp}(\lambda)$ then $S\_n = \sum\_{i=1}^n X\_i \sim \textrm{Gamma}(\mbox{shape} = n, \mbox{rate} = \lambda)$; (2) The waiting time between $n$ occurrences in a Poisson Process with intensity parameter $\lambda$ is gamma distributed with shape parameter $n$ and rate parameter $\lambda$. 