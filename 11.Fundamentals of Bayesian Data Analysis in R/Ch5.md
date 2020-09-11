# Chapter 5 - More parameters, more data, and more Bayes
## rnorm, dnorm, and the weight of newborns

```r

# Assign mu and sigma
mu <- 3500
sigma <- 600

weight_distr <- rnorm(n = 100000, mean = mu, sd = sigma)
hist(weight_distr, 60, xlim = c(0, 6000), col = "lightgreen")

# Create weight
weight <- seq(0, 6000, by = 100)

# Calculate likelihood
likelihood <- dnorm(weight, mu, sigma)

# Plot the distribution of weight
plot(weight, likelihood, type = "h")

```

Ouput:

![ch5plot1](ch5plot1.png)

![ch5plot2](ch5plot2.png)





