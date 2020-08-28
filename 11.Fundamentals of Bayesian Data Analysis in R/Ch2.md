# Chapter 2 - How does Bayesian inference work?
## Take a generative model for a spin 

### 1
```r

# The generative zombie drug model

# Set parameters
prop_success <- 0.42
n_zombies <- 100

# Simulating data
data <- c()
for(zombie in 1:n_zombies) {
  data[zombie] <- runif(1, min = 0, max = 1) < prop_success
}
data <- as.numeric(data)
data

```
Output:

```bash
> # The generative zombie drug model
> 
> # Set parameters
> prop_success <- 0.42
> n_zombies <- 100
> 
> # Simulating data
> data <- c()
> for(zombie in 1:n_zombies) {
    data[zombie] <- runif(1, min = 0, max = 1) < prop_success
  }
> data <- as.numeric(data)
> data
  [1] 1 1 0 1 0 1 1 0 0 0 1 1 0 0 1 0 1 0 0 1 1 0 1 1 0 1 0 0 0 0 0 0 0 1 0 0 0
 [38] 1 1 1 1 0 0 0 1 1 1 0 1 1 0 0 1 0 0 1 0 0 0 0 0 1 0 0 0 1 0 1 0 0 1 0 0 0
 [75] 1 0 0 1 1 1 0 1 0 1 0 1 0 0 0 0 0 0 0 0 0 0 1 0 0 0
> 

```

### 2

```r

# The generative zombie drug model

# Set parameters
prop_success <- 0.42
n_zombies <- 100

# Simulating data
data <- c()
for(zombie in 1:n_zombies) {
  data[zombie] <- runif(1, min = 0, max = 1) < prop_success
}

# Count cured
data <- sum(data)
data

```
Output:

```bash
> # The generative zombie drug model
> 
> # Set parameters
> prop_success <- 0.42
> n_zombies <- 100
> 
> # Simulating data
> data <- c()
> for(zombie in 1:n_zombies) {
    data[zombie] <- runif(1, min = 0, max = 1) < prop_success
  }
> 
> # Count cured
> data <- sum(data)
> data
[1] 47
```
***

## Take the binomial distribution for a spin

* n The number of times you want to run the generative model
* size The number of trials.
* prob The underlying proportion of success as a number between 0.0 and 1.0.

```r

rbinom(n = 200, size = 100, prob = 0.42)

```

Ouput:

```bash

> # Chenge the parameters
> rbinom(n = 200, size = 100, prob = 0.42)
  [1] 36 47 35 43 42 33 50 41 32 49 36 46 45 39 34 40 42 35 38 40 40 47 37 47 46
 [26] 43 44 33 32 40 43 36 42 43 40 43 46 48 45 49 44 33 33 41 45 46 47 47 33 42
 [51] 37 41 42 39 39 53 42 50 37 48 41 38 44 44 38 52 37 46 39 40 42 48 39 35 48
 [76] 40 41 43 37 37 43 45 43 51 34 39 38 42 46 49 37 45 46 48 45 43 35 44 42 37
[101] 48 38 53 39 40 30 43 35 36 43 39 46 42 45 45 43 37 33 37 33 40 42 36 45 46
[126] 40 46 36 36 44 45 50 43 37 37 51 39 48 45 45 33 27 47 46 39 36 41 48 44 52
[151] 37 45 39 41 43 46 39 41 41 42 40 46 42 49 43 38 31 38 37 43 44 38 45 44 37
[176] 50 35 36 37 40 46 46 34 46 47 34 46 44 38 45 38 47 43 45 41 49 44 42 53 44
> 

```

***

## How many visitors could your site get (1)?

```r

# Fill in the parameters
n_samples <- 100000
n_ads_shown <- 100
proportion_clicks <- 0.1
n_visitors <- rbinom(n_samples, size = n_ads_shown, 
                     prob = proportion_clicks)

# Visualize n_visitors
hist(n_visitors)

```
Output:


![ch2plot1](ch2plot1.png)

***

## Adding a prior to the model

```r
n_samples <- 100000
n_ads_shown <- 100
proportion_clicks <- runif(n_samples, min = 0.0, max = 0.2)
n_visitors <- rbinom(n = n_samples, size = n_ads_shown, prob = proportion_clicks)

# Visualize proportion clicks
hist(proportion_clicks)

# Visualize n_visitors
hist(n_visitors)

```

Output:

![ch2plot2](ch2plot2.png)

![ch2plot3](ch2plot3.png)


***





