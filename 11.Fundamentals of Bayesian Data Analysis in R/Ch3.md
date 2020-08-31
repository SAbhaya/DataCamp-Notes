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

