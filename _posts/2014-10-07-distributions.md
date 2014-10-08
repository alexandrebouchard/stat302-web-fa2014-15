---
layout: post
title: "Distributions - Quick reference"
---

**Bernoulli:**

- Parameter: a *success probability* $p$
- Notation in JAGS: ``X ~ dbern(p)``
- Mathematical notation: $X \sim \textrm{Bern}(p)$, or $X \sim \textrm{Bernoulli}(p)$
- PMF: $\P(X = 1) = p, \;\;\;\;\P(X = 0) = 1-p$
- Mean: $p$
- Variance: $p(1-p)$

**Binomial:**

- Parameters: a *success probability* $p$, and a *number of trials* $n$
- Notation in JAGS: ``X ~ dbin(p, n)`` **Note:** $p$ and $n$ have a different order in JAGS and the textbook.
- Mathematical notation: $X \sim \textrm{Bin}(n, p)$, or  $X \sim \textrm{Binomial}(n, p)$
- PMF: $\P(X = i) = \binom{n}{i} p^i (1-p)^{n-i}$, for $i \in \{0, 1, \dots, n\}$
- Mean: $np$
- Variance: $np(1-p)$

**Categorical:**

- Parameter: a set of possible values $\{1, 2, \dots, n\}$, and a vector of probabilities $p = (p\_1, \dots, p\_n)$ giving the probability of each value in the set
- Notation in JAGS: ``X ~ dcat(p)``
- Mathematical notation: $X \sim \textrm{Cat}(p)$, or $X \sim \textrm{Categorical}(p)$
- PMF: $\P(X = i) = p\_i$, for $i \in \{1, 2, \dots, n\}$

**Poisson:** 

- Parameter: a *rate* $\lambda=$lambda > 0
- Notation in JAGS: ``X ~ dpois(lambda)``
- Mathematical notation: $X \sim \textrm{Poi}(\lambda)$ or $X \sim \textrm{Poisson}(\lambda)$
- PMF: $\P(X = i) = e^{-\lambda} \lambda^i / i!$, for $i \in \{0, 1, 2, 3, \dots\}$
- Mean: $\lambda$
- Variance: $\lambda$

**Discrete uniform:** 

- Parameter: a set of possible values $\{1, 2, \dots, n\}$
- Notation in JAGS: use a categorical distribution, setting equal probability to each value
- Mathematical notation: $X \sim \textrm{Unif}(\{1, 2, \dots, n\})$
- PMF: $\P(X = i) = 1/n$, for $i \in \{1, 2, \dots, n\}$