---
layout: post
title: "doomsday-jags"
---

Follow the following steps to create the model (raise your hand if you encounter any issues):

- From ``Tools`` select ``JAGS``
- Log in using your student id (you may have to sign up again, my apologies)
- Create a new ``Model``
- Click on the newly created model, and create two files: ``model.bugs`` and ``run.R``
- Use the following template, where you only have to modify ``model.bugs`` (remember to click on ``Commit`` frequently.

``run.R``:

```r
require(rjags)
require(ggplot2)
require(coda.discrete.utils)
set.seed(14718)

coda.density <- function(jags.output)
{
  for (variable.name in names(jags.output)) 
  {
    coda.output <- as.mcmc.list(jags.output[[variable.name]])
    my.matrix <- as.matrix(coda.output)
    my.frame <- data.frame(x= my.matrix[,1])
    
    p <- ggplot(my.frame, aes(x)) + 
      geom_density(adjust=1, size=2) +
      labs(x = "x", y = "Approximation of f(x)") +
      ggtitle(paste("Density of ", variable.name, sep=""))
    ggsave(p, filename = paste(variable.name , "-density.pdf",sep=""))
  }
  
}
model <- jags.model(
  'model.bugs')

samples <- 
  jags.samples(model,
               c('Z'), 
               100000) 
              
coda.density(samples)
coda.expectation(samples)
```

``model.bugs``:

```r
# This contains what we want to condition on
data {

  Y <- 0.06
    
}

model {
  
  # fill this with the model
  
}
```

To run this model:

- Click on the home button corresponding to the file ``run.R`` (this instructs what should be ran)
- Click on ``Run``


Once you are done, you can start thinking about the following optional and harder problem (taken from a real-world WW2 story):

- Suppose you observe the serial numbers of captured german tanks, for example ``94,90,57,21,14``
- How can you use this information to make a probabilistic guess at the total number of german tanks?