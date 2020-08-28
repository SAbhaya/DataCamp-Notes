# How does Bayesian inference work?
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

