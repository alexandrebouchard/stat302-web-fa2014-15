---
layout: post
title: "JAGS tutorial"
category: 'Tool'
---

JAGS
--------

This is a brief tutorial on using JAGS and the online execution system for Stats 302. The tools necessary for the second assignment exercises are explained through an example problem that originally appeared on WeBWorK. We first describe how a probabilistic model is encoded in the JAGS language, then show how samples are drawn from the distribution specified by the JAGS model, followed by a simple strategy to analyze the results. 

The JAGS language provides a systematic method to define probabilistic relationships between variables of interest. In particular, it equips the user with a syntax to describe relationships often represented through probabilistic graphical models (which in turn use conditional factorizations of the joint distribution). JAGS models are specified within ```.bugs``` files that look like: 

```
data {

}

model {

}

```
where ```data{ ... }``` blocks contain observed data (i.e. constants, or realizations of random variables), and importantly the ``` model{ ... }``` block contains the model specification. 

Once a model has been defined in the JAGS language, users interact with it via sampling to estimate quantities of interest. Quantities of interest of a random variable defined within the ```.bugs``` file could be its pmf, expectation, or variance. 

To sample from JAGS, we invoke the ```jags.samples``` command within an R script. This function call looks something like this: 

```
	samples <- 
    jags.samples(model.bugs,
                 variable, 
                 nIter) 
```
where ```jags.samples``` where ```model.bugs``` is the model file, ```variable``` is the random variable of interest, and ```nIter``` is the number of simulations used to approximate the random variable (we set it to 1000000 in our simulations). 

For this course all JAGS modeling is completed through a custom built cloud service located [here](http://54.201.228.108/), see the course webpage for [additional details](http://www.stat.ubc.ca/~bouchard/courses/stat302-fa2014-15//tool/2014/10/06/jags.html). Once you've successively logged into this service the two files ```.bugs``` and ```.R``` for model specification and sampling/analytics, respectively are found here: 

<img src="{{ site.url }}/images/JAGS/jags-file-location.png" style="width: 200px"/>

Clicking on either of these two files allows you to edit their contents and then save by pressing ```Commit``` at the top. When you press ```Run``` the cloud service executes the contents of the R file and outputs the results in (i) the terminal window, and (ii) bottom left for generated files (see below). 

<img src="{{ site.url }}/images/JAGS/full-JAGS.png" alt="Drawing" style="width: 800px;"/>

The questions we will use JAGS to answer are in determining the pmf, cdf, expectation and variance. To estimate these quantities we sample a random variable specified in the JAGS ```.bugs``` model, and then use these samples to approximate the quantity of interest. For example, the following tools calculate the pmf, cdf, expectation, and variance, 

```
  coda.pmf(samples, show.table = TRUE) 
  coda.cdf(samples, show.table = TRUE)
  e <- coda.expectation(samples)
  v <- coda.variance(samples)
```

for ```samples``` generated by the ```jags.samples(...)``` function call. Since we will be estimating these quantities many times (for different variables), I suggest making this R utility function:

```
coda.tools <- function(samples)
{
  coda.pmf(samples, show.table = TRUE) 
  coda.cdf(samples, show.table = TRUE)
  e <- coda.expectation(samples)
  v <- coda.variance(samples)
  print(c("Expected value", round(e[[1]], 3)), quote = F)
  print(c("Variance", round(v[[1]], 3)), quote = F)
}
```

With the basics in hand, let's try an illustrative example.

Example problem (from WeBWorK)
-------------------

A fair die is rolled once, and the number score is noted. Let the random variable $X$ be twice this score. Define the variable $Y$ to be zero if an odd number appears and $X$ otherwise. By finding the probability mass function in each case, find the expectation of the following random variables:

- $X$
- $Y$
- $X + Y$ 
- $XY$ 

We will answer this problem both analytically and with JAGS. We will compare the numerical estimates with the exact results. 

####JAGS Model

For a 6 sided die we translate the problem statement into the JAGS language as: 

````
data {
n <- 6
}

model {
  
  for (i in 1:n){
    p[i] <- 1 / n
  }  
  
  face_rolled ~ dcat(p)
 
  X <- 2 * face_rolled

  Y <- ifelse(equals(face_rolled,2) || equals(face_rolled, 4) || equals(face_rolled, 6), X, 0)
        
  S <- X + Y
  P <- X * Y
  
}
````
Now, we answer each part of the question with JAGS. 

Part a) 

The probability mass function is: 

\begin{array}{|r|r|r|r|r|r|r|}
\\hline
x & 2 & 4 & 6 & 8 & 10 & 12 \\\\
\\hline
\P(X=x)&1/6&1/6&1/6&1/6&1/6&1/6 \\\\
\\hline
\end{array}

Hence, $\E(X) = \frac{1}{6}(2+4+6+8+10+12) = 7.0$

```
  samples.X <- 
    jags.samples(model,
                 c('X'), 
                 1000000) 
                 
    coda.tools(samples.X)   

k Approximation of P(X = k)
2 0.167082
4 0.166848
6 0.166388
8 0.166524
10 0.166685
12 0.166473

k Approximation of P(X <= k)
2 0.167082
4 0.33393
6 0.500318
8 0.666842
10 0.833527
12 1

[1] Expected value 6.997         
[1] Variance 11.674
        
              
```

<img src="{{ site.url }}/images/JAGS/X-pmf.png" style="float: left; width: 40%; margin-right: 1%; margin-bottom: 0.5em;">
<img src="{{ site.url }}/images/JAGS/X-cdf.png" style="float: left; width: 40%; margin-right: 1%; margin-bottom: 0.5em;">
<p style="clear: both;">

Part b) 
The probability mass function here is 

\begin{array}{|r|r|r|r|r|}
\\hline
y & 0 & 4 & 8 & 12 \\\\
\\hline
\P(Y=y) & 1/2 & 1/6 & 1/6 & 1/6 \\\\
\\hline
\end{array}

Hence, $\E(Y) = 0 + \frac{1}{6}(4+8+12) = 4.0 $

```
  samples.Y <- 
    jags.samples(model,
                 c('Y'), 
                 1000000) 
                 
    coda.tools(samples.Y)  

k Approximation of P(Y = k)
0 0.499993
4 0.16676
8 0.167149
12 0.166098

k Approximation of P(Y <= k)
0 0.499993
4 0.666753
8 0.833902
12 1

[1] Expected value 3.997         
[1] Variance 21.305 
               
```
<img src="{{ site.url }}/images/JAGS/Y-pmf.png" style="float: left; width: 40%; margin-right: 1%; margin-bottom: 0.5em;">
<img src="{{ site.url }}/images/JAGS/Y-cdf.png" style="float: left; width: 40%; margin-right: 1%; margin-bottom: 0.5em;">
<p style="clear: both;">

Part c) 

Setting $S = X+Y$,  we have 

\begin{array}{|r|r|r|r|r|r|r|}
\\hline
s & 2 & 6 & 8 & 10 & 16 & 24 \\\\
\\hline
\P(S=s) & 1/6 & 1/6 & 1/6 & 1/6 & 1/6 & 1/6  \\\\
\\hline
\end{array}

$\E(S) = \frac{1}{6}(2+6+8+10+16+24) = 11.0$

```
  samples.S <- 
    jags.samples(model,
                 c('S'), 
                 1000000) 
                 
    coda.tools(samples.S)    
    
k Approximation of P(S = k)
2 0.167152
6 0.166186
8 0.166859
10 0.166205
16 0.166736
24 0.166862

k Approximation of P(S <= k)
2 0.167152
6 0.333338
8 0.500197
10 0.666402
16 0.833138
24 1

[1] Expected value 11.001        
[1] Variance 51.73          
```
<img src="{{ site.url }}/images/JAGS/S-pmf.png" style="float: left; width: 40%; margin-right: 1%; margin-bottom: 0.5em;">
<img src="{{ site.url }}/images/JAGS/S-cdf.png" style="float: left; width: 40%; margin-right: 1%; margin-bottom: 0.5em;">
<p style="clear: both;">

Part d) 

Letting $P = XY$ we have

\begin{array}{|r|r|r|r|r|}
\\hline
p & 0 & 16 & 64 & 144 \\\\
\\hline
P(P=p) & 1/2 & 1/6 & 1/6 & 1/6 \\\\
\\hline
\end{array}

$\E(P) = \frac{1}{6}(16+64+144) = \frac{112}{3}\\approx 37.3$

```
  samples.P <- 
    jags.samples(model,
                 c('P'), 
                 1000000) 
                 
    coda.tools(samples.P) 
    
k Approximation of P(P = k)
0 0.50079
16 0.166597
64 0.166601
144 0.166012

k Approximation of P(P <= k)
0 0.50079
16 0.667387
64 0.833988
144 1

[1] Expected value 37.234        
[1] Variance 2781.122            
```

<img src="{{ site.url }}/images/JAGS/P-pmf.png" style="float: left; width: 40%; margin-right: 1%; margin-bottom: 0.5em;">
<img src="{{ site.url }}/images/JAGS/P-cdf.png" style="float: left; width: 40%; margin-right: 1%; margin-bottom: 0.5em;">
<p style="clear: both;">

#### Putting it all together in one script

```
require(rjags)
require(coda.discrete.utils)

coda.tools <- function(samples)
{
  coda.pmf(samples, show.table = TRUE) 
  coda.cdf(samples, show.table = TRUE)
  e <- coda.expectation(samples)
  v <- coda.variance(samples)
  print(c("Expected value", round(e[[1]], 3)), quote = F)
  print(c("Variance", round(v[[1]], 3)), quote = F)
}

sample.properties <- function(variable, model, nIter)
{
  model.samples <- 
    jags.samples(model,
                 variable, 
                 nIter) 
  coda.tools(model.samples)
}

nIter <- 1000000
dice.model <- jags.model('dice-model.bugs')
sample.properties('X', dice.model, nIter)
sample.properties('Y', dice.model, nIter)
sample.properties('S', dice.model, nIter)
sample.properties('P', dice.model, nIter)
```


