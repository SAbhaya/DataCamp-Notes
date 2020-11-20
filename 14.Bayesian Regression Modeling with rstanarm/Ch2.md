# Chapter 2 - Modifying a Bayesian Model

## Altering chains

```r

# 3 chains, 1000 iterations, 500 warmup
model_3chains <- stan_glm(popularity ~ song_age, data = songs,
    chains = 3, iter = 1000, warmup = 500)

# Print a summary of model_3chains
summary(model_3chains)

# 2 chains, 100 iterations, 50 warmup
model_2chains <- stan_glm(popularity ~ song_age, data = songs,
    chains = 2, iter = 100, warmup = 50)

# Print a summary of model_2chains
summary(model_2chains)

```

Output:

```bash

# 3 chains, 1000 iterations, 500 warmup
model_3chains <- stan_glm(popularity ~ song_age, data = songs,
    chains = 3, iter = 1000, warmup = 500)
# Print a summary of model_3chains
summary(model_3chains)

Model Info:
 function:     stan_glm
 family:       gaussian [identity]
 formula:      popularity ~ song_age
 algorithm:    sampling
 sample:       1500 (posterior sample size)
 priors:       see help('prior_summary')
 observations: 215
 predictors:   2

Estimates:
              mean   sd   10%   50%   90%
(Intercept) 68.6    2.2 65.8  68.6  71.5 
song_age     0.0    0.0  0.0   0.0   0.0 
sigma       15.0    0.7 14.0  15.0  15.9 

Fit Diagnostics:
           mean   sd   10%   50%   90%
mean_PPD 52.5    1.4 50.7  52.5  54.3 

The mean_ppd is the sample average posterior predictive distribution of the outcome variable (for details see help('summary.stanreg')).

MCMC diagnostics
              mcse Rhat n_eff
(Intercept)   0.1  1.0  1559 
song_age      0.0  1.0  1569 
sigma         0.0  1.0  1623 
mean_PPD      0.0  1.0  1348 
log-posterior 0.0  1.0   745 

For each parameter, mcse is Monte Carlo standard error, n_eff is a crude measure of effective sample size, and Rhat is the potential scale reduction factor on split chains (at convergence Rhat=1).
# 2 chains, 100 iterations, 50 warmup
model_2chains <- stan_glm(popularity ~ song_age, data = songs,
    chains = 2, iter = 100, warmup = 50)
Warning message: There were 1 chains where the estimated Bayesian Fraction of Missing Information was low. See
http://mc-stan.org/misc/warnings.html#bfmi-low
Warning message: Examine the pairs() plot to diagnose sampling problems
Warning message: The largest R-hat is 1.24, indicating chains have not mixed.
Running the chains for more iterations may help. See
http://mc-stan.org/misc/warnings.html#r-hat
Warning message: Bulk Effective Samples Size (ESS) is too low, indicating posterior means and medians may be unreliable.
Running the chains for more iterations may help. See
http://mc-stan.org/misc/warnings.html#bulk-ess
Warning message: Tail Effective Samples Size (ESS) is too low, indicating posterior variances and tail quantiles may be unreliable.
Running the chains for more iterations may help. See
http://mc-stan.org/misc/warnings.html#tail-ess
Warning message: Markov chains did not converge! Do not analyze results!
# Print a summary of model_2chains
summary(model_2chains)

Model Info:
 function:     stan_glm
 family:       gaussian [identity]
 formula:      popularity ~ song_age
 algorithm:    sampling
 sample:       100 (posterior sample size)
 priors:       see help('prior_summary')
 observations: 215
 predictors:   2

Estimates:
              mean   sd   10%   50%   90%
(Intercept) 65.7    8.7 62.0  68.4  70.8 
song_age     0.0    0.0  0.0   0.0   0.0 
sigma       16.5    5.2 14.0  15.0  16.7 

Fit Diagnostics:
           mean   sd   10%   50%   90%
mean_PPD 49.2    8.7 45.2  51.9  53.8 

The mean_ppd is the sample average posterior predictive distribution of the outcome variable (for details see help('summary.stanreg')).

MCMC diagnostics
              mcse Rhat n_eff
(Intercept)    3.2  1.2   8  
song_age       0.0  1.0 132  
sigma          2.0  1.3   7  
mean_PPD       3.5  1.3   6  
log-posterior 18.1  1.3   7  

For each parameter, mcse is Monte Carlo standard error, n_eff is a crude measure of effective sample size, and Rhat is the potential scale reduction factor on split chains (at convergence Rhat=1).
>

```
***


The model_2chains doesn't converge

***

## Determine Prior Distributions

```r

# Estimate the model
stan_model <- stan_glm(popularity ~ song_age, data = songs)

# Print a summary of the prior distributions
prior_summary(stan_model)

```

Output:

```r

# Estimate the model
stan_model <- stan_glm(popularity ~ song_age, data = songs)
# Print a summary of the prior distributions
prior_summary(stan_model)
Priors for model 'stan_model' 
------
Intercept (after predictors centered)
  Specified prior:
    ~ normal(location = 0, scale = 10)
  Adjusted prior:
    ~ normal(location = 0, scale = 170)

Coefficients
  Specified prior:
    ~ normal(location = 0, scale = 2.5)
  Adjusted prior:
    ~ normal(location = 0, scale = 0.03)

Auxiliary (sigma)
  Specified prior:
    ~ exponential(rate = 1)
  Adjusted prior:
    ~ exponential(rate = 0.059)
------
See help('prior_summary.stanreg') for more details

```
***

## Calculate Adjusted Scales

```r

# Calculate the adjusted scale for the intercept
10 * sd(songs$popularity)

# Calculate the adjusted scale for `song_age`
(2.5/ sd(songs$song_age)) * sd(songs$popularity)

# Calculate the adjusted scale for `valence`
(2.5/ sd(songs$valence)) * sd(songs$popularity)

```

Output:

```bash

# Calculate the adjusted scale for the intercept
10 * sd(songs$popularity)
[1] 169.5062
# Calculate the adjusted scale for `song_age`
(2.5/ sd(songs$song_age)) * sd(songs$popularity)
[1] 0.03047504
# Calculate the adjusted scale for `valence`
(2.5/ sd(songs$valence)) * sd(songs$popularity)
[1] 1.98643

```

***

## Unadjusted Priors

```r
# Estimate the model with unadjusted scales
no_scale <- stan_glm(popularity ~ song_age, data = songs,
    prior_intercept = normal(autoscale = FALSE),
    prior = normal(autoscale = FALSE),
    prior_aux = exponential(autoscale = FALSE)
)

# Print the prior summary
prior_summary(no_scale)

```

Output:

```bash
# Estimate the model with unadjusted scales
no_scale <- stan_glm(popularity ~ song_age, data = songs,
    prior_intercept = normal(autoscale = FALSE),
    prior = normal(autoscale = FALSE),
    prior_aux = exponential(autoscale = FALSE)
)
# Print the prior summary
prior_summary(no_scale)
Priors for model 'no_scale' 
------
Intercept (after predictors centered)
 ~ normal(location = 0, scale = 10)

Coefficients
 ~ normal(location = 0, scale = 2.5)

Auxiliary (sigma)
 ~ exponential(rate = 1)
------
See help('prior_summary.stanreg') for more details


```
***

## Changing Priors

```r

# Estimate a model with flat priors
flat_prior <- stan_glm(popularity ~ song_age, data = songs,
    prior_intercept  = NULL, prior  = NULL, prior_aux  = NULL)

# Print a prior summary
prior_summary(flat_prior)

```

Output:

```bash

# Estimate a model with flat priors
flat_prior <- stan_glm(popularity ~ song_age, data = songs,
    prior_intercept  = NULL, prior  = NULL, prior_aux  = NULL)
# Print a prior summary
prior_summary(flat_prior)
Priors for model 'flat_prior' 
------
Intercept (after predictors centered)
 ~ flat

Coefficients
 ~ flat

Auxiliary (sigma)
 ~ flat
------
See help('prior_summary.stanreg') for more details
>

```

***

## Specifying informative priors

```r
# Estimate the model with an informative prior
inform_prior <- stan_glm(popularity ~ song_age, data = songs,
    prior = normal(location = 20, scale = 0.1, autoscale = FALSE))

# Print the prior summary
prior_summary(inform_prior)

```

Output:

```bash

# Estimate the model with an informative prior
inform_prior <- stan_glm(popularity ~ song_age, data = songs,
    prior = normal(location = 20, scale = 0.1, autoscale = FALSE))
Warning message: Tail Effective Samples Size (ESS) is too low, indicating posterior variances and tail quantiles may be unreliable.
Running the chains for more iterations may help. See
http://mc-stan.org/misc/warnings.html#tail-ess
# Print the prior summary
prior_summary(inform_prior)
Priors for model 'inform_prior' 
------
Intercept (after predictors centered)
  Specified prior:
    ~ normal(location = 0, scale = 10)
  Adjusted prior:
    ~ normal(location = 0, scale = 170)

Coefficients
 ~ normal(location = 20, scale = 0.1)

Auxiliary (sigma)
  Specified prior:
    ~ exponential(rate = 1)
  Adjusted prior:
    ~ exponential(rate = 0.059)
------
See help('prior_summary.stanreg') for more details

```

***

## Consequences of informative priors

The inform_prior model you estimated is loaded in the environment. As a reminder, that model was specified with a normal prior on the predictor variable with a mean of 20 and standard deviation of 0.1. How did the specified prior affect the parameter estimates?


> The estimate was almost the same as the prior

## Altering the Estimation

```r
# Estimate the model with a new `adapt_delta`
adapt_model <- stan_glm(popularity ~ song_age, data = songs,
  control = list(adapt_delta = 0.99))

# View summary
summary(adapt_model)

# Estimate the model with a new `max_treedepth`
tree_model <- stan_glm(popularity ~ song_age, data = songs,
  control = list(max_treedepth = 15))

# View summary
summary(tree_model)


```

Output:

```bash

# Estimate the model with a new `adapt_delta`
adapt_model <- stan_glm(popularity ~ song_age, data = songs,
  control = list(adapt_delta = 0.99))
# View summary
summary(adapt_model)

Model Info:
 function:     stan_glm
 family:       gaussian [identity]
 formula:      popularity ~ song_age
 algorithm:    sampling
 sample:       1000 (posterior sample size)
 priors:       see help('prior_summary')
 observations: 215
 predictors:   2

Estimates:
              mean   sd   10%   50%   90%
(Intercept) 68.7    2.2 65.7  68.7  71.5 
song_age     0.0    0.0  0.0   0.0   0.0 
sigma       15.0    0.8 14.0  14.9  15.9 

Fit Diagnostics:
           mean   sd   10%   50%   90%
mean_PPD 52.5    1.4 50.8  52.5  54.3 

The mean_ppd is the sample average posterior predictive distribution of the outcome variable (for details see help('summary.stanreg')).

MCMC diagnostics
              mcse Rhat n_eff
(Intercept)   0.1  1.0  1089 
song_age      0.0  1.0  1080 
sigma         0.0  1.0   989 
mean_PPD      0.0  1.0   857 
log-posterior 0.1  1.0   511 

For each parameter, mcse is Monte Carlo standard error, n_eff is a crude measure of effective sample size, and Rhat is the potential scale reduction factor on split chains (at convergence Rhat=1).
# Estimate the model with a new `max_treedepth`
tree_model <- stan_glm(popularity ~ song_age, data = songs,
  control = list(max_treedepth = 15))
# View summary
summary(tree_model)

Model Info:
 function:     stan_glm
 family:       gaussian [identity]
 formula:      popularity ~ song_age
 algorithm:    sampling
 sample:       1000 (posterior sample size)
 priors:       see help('prior_summary')
 observations: 215
 predictors:   2

Estimates:
              mean   sd   10%   50%   90%
(Intercept) 68.7    2.2 65.7  68.7  71.5 
song_age     0.0    0.0  0.0   0.0   0.0 
sigma       15.0    0.8 14.0  14.9  15.9 

Fit Diagnostics:
           mean   sd   10%   50%   90%
mean_PPD 52.5    1.4 50.8  52.5  54.3 

The mean_ppd is the sample average posterior predictive distribution of the outcome variable (for details see help('summary.stanreg')).

MCMC diagnostics
              mcse Rhat n_eff
(Intercept)   0.1  1.0  1089 
song_age      0.0  1.0  1080 
sigma         0.0  1.0   989 
mean_PPD      0.0  1.0   857 
log-posterior 0.1  1.0   511 

For each parameter, mcse is Monte Carlo standard error, n_eff is a crude measure of effective sample size, and Rhat is the potential scale reduction factor on split chains (at convergence Rhat=1).


```

***

*End of Chapter 2*
