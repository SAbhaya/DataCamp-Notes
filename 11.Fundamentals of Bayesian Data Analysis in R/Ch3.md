# Chapter 3 - Why use Bayesian Data Analysis?
## Explore using the Beta distribution as a prior
The Beta distribution -> model uncertainty over a parameter bounded between 0 and 1.\

### 1

```r
# Explore using the rbeta function
beta_sample <- rbeta(n = 1000000, shape1 = 1, shape2 = 1)

# Visualize the results
hist(beta_sample)

```

Output:

![ch3plot1](ch3plot1.png)

### 2

```r
# Modify the parameters
beta_sample <- rbeta(n = 1000000, shape1 = -1, shape2 = 1)

# Explore the results
head(beta_sample)
```

Output:

```bash
> # Modify the parameters
> beta_sample <- rbeta(n = 1000000, shape1 = -1, shape2 = 1)
Warning message: NAs produced
> 
> # Explore the results
> head(beta_sample)
[1] NaN NaN NaN NaN NaN NaN
> 

```

### 3

```r
# Modify the parameters
beta_sample <- rbeta(n = 1000000, shape1 = 100, shape2 = 100)

# Visualize the results
hist(beta_sample)

```
Output:

![ch3plot2](ch3plot2.png)

### 4

```r
# Modify the parameters
beta_sample <- rbeta(n = 1000000, shape1 = 100, shape2 = 20)

# Visualize the results
hist(beta_sample)

```
Output:

![ch3plot3](ch3plot3.png)

***

## Change the model to use an informative prior

```r

n_draws <- 100000
n_ads_shown <- 100

# Change the prior on proportion_clicks
proportion_clicks <- 
  rbeta(n_draws, shape1 = 5, shape2 = 95)
n_visitors <- 
  rbinom(n_draws, size = n_ads_shown, 
         prob = proportion_clicks)
prior <- 
  data.frame(proportion_clicks, n_visitors)
posterior <- 
  prior[prior$n_visitors == 13, ]

# This plots the prior and the posterior in the same plot
par(mfcol = c(2, 1))
hist(prior$proportion_clicks, 
     xlim = c(0, 0.25))
hist(posterior$proportion_clicks, 
     xlim = c(0, 0.25))
     
```

Output:

![ch3plot4](ch3plot4.png)

***

## Fit the model using another dataset

```r

# Define parameters
n_draws <- 100000
n_ads_shown <- 100
proportion_clicks <- runif(n_draws, min = 0.0, max = 0.2)
n_visitors <- rbinom(n = n_draws, size = n_ads_shown, 
                     prob = proportion_clicks)
prior <- data.frame(proportion_clicks, n_visitors)

# Create the posteriors for video and text ads
posterior_video <- prior[prior$n_visitors == 13, ]
posterior_text <- prior[prior$n_visitors == 6, ]

# Visualize the posteriors
hist(posterior_video$proportion_clicks, xlim = c(0, 0.25))
hist(posterior_text$proportion_clicks, xlim = c(0, 0.25))

```

Output:

![ch3plot6](ch3plot6.png)
![ch3plot5](ch3plot5.png)

***

