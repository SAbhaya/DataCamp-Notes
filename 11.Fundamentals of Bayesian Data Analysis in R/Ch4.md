# Chapter 4 - Bayesian inference with Bayes' theorem
## Cards and the sum rule

```r

# Calculate the probability of drawing any of the four aces
prob_to_draw_ace <- (1/52 +1/52 +1/52+1/52)

```

***

## Cards and the product rule

```r

# Calculate the probability of picking four aces in a row
prob_to_draw_four_aces <- ((4/52)*(3/51)*(2/50)*(1/49))

```
## From rbinom to dbinom

```r

# Rewrite this code so that it uses dbinom instead of rbinom
n_ads_shown <- 100
proportion_clicks <- 0.1
n_visitors <- rbinom(n = 99999, 
    size = n_ads_shown, prob = proportion_clicks)
prob_13_visitors <- sum(n_visitors == 13) / length(n_visitors)
prob_13_visitors

```
Output:

```bash
> # Rewrite this code so that it uses dbinom instead of rbinom
> n_ads_shown <- 100
> proportion_clicks <- 0.1
> n_visitors <- rbinom(n = 99999, 
      size = n_ads_shown, prob = proportion_clicks)
> prob_13_visitors <- sum(n_visitors == 13) / length(n_visitors)
> prob_13_visitors
[1] 0.07416074
> 
```

New code

```r
# Rewrite this code so that it uses dbinom instead of rbinom
n_ads_shown <- 100
proportion_clicks <- 0.1
prob_13_visitors <- dbinom(13, size=n_ads_shown, prob = proportion_clicks)
prob_13_visitors
```

Output:

```bash
# Rewrite this code so that it uses dbinom instead of rbinom
n_ads_shown <- 100
proportion_clicks <- 0.1
prob_13_visitors <- dbinom(13, size=n_ads_shown, prob = proportion_clicks)
prob_13_visitors

```
***

## Calculating probabilities with dbinom

### 2

```r

# Change the code according to the instructions
n_ads_shown <- 100
proportion_clicks <- 0.1
n_visitors <- seq(0, 100)
prob <- dbinom(n_visitors, 
    size = n_ads_shown, prob = proportion_clicks)
prob

plot(n_visitors, prob, type = "h")

```
Output:

![ch4plot1](ch4plot1.png)

### 3

```r

# Change the code according to the instructions
n_ads_shown <- 100
proportion_clicks <- seq(0, 1, by = 0.01)
n_visitors <- 13
prob <- dbinom(n_visitors, 
    size = n_ads_shown, prob = proportion_clicks)
prob

plot(proportion_clicks, prob, type = "h")

```

Output:

![ch4plot2](ch4plot2.png)


Looking at the plot, what value of proportion_clicks seems to give the maximum likelihood to produce n_visitors = 13?
</br>
proportion_clicks = 0.13

***

## Calculating a joint distribution

```r

n_ads_shown <- 100
proportion_clicks <- seq(0, 1, by = 0.01)
n_visitors <- seq(0, 100, by = 1)
pars <- expand.grid(proportion_clicks = proportion_clicks,
                    n_visitors = n_visitors)
pars$prior <- dunif(pars$proportion_clicks, min = 0, max = 0.2)
pars$likelihood <- dbinom(pars$n_visitors, 
    size = n_ads_shown, prob = pars$proportion_clicks)

# Add the column pars$probability and normalize it
pars$probability <- pars$likelihood*pars$prior
pars$probability <- pars$probability/sum(pars$probability)

```
***

## Conditioning on the data (again)

```r
n_ads_shown <- 100
proportion_clicks <- seq(0, 1, by = 0.01)
n_visitors <- seq(0, 100, by = 1)
pars <- expand.grid(proportion_clicks = proportion_clicks,
                    n_visitors = n_visitors)
pars$prior <- dunif(pars$proportion_clicks, min = 0, max = 0.2)
pars$likelihood <- dbinom(pars$n_visitors, 
    size = n_ads_shown, prob = pars$proportion_clicks)
pars$probability <- pars$likelihood * pars$prior
pars$probability <- pars$probability / sum(pars$probability)
# Condition on the data 
pars <- pars[pars$n_visitors == 6,]
# Normalize again
pars$probability <- pars$probability/sum(pars$probability)
# Plot the posterior pars$probability
plot(pars$proportion_clicks, pars$probability, type = "h")

```

Output:

![ch4plot3](ch4plot3.png)




