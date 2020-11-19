# Bayesian Regression Modeling with rstanarm
# Chapter 1 - Introduction to Bayesian Linear Models

## Exploring the data

```r
# Print the first 6 rows
head(songs)

# Print the structure
str(songs)


```

Output:

```bash
# Print the first 6 rows
head(songs)
        track_name    artist_name          album_name song_age valence   tempo
1    Crazy In Love Beyoncé Dangerously In Love     5351    70.1  99.259
2     Naughty Girl Beyoncé Dangerously In Love     5351    64.3  99.973
3         Baby Boy Beyoncé Dangerously In Love     5351    77.4  91.039
4     Hip Hop Star Beyoncé Dangerously In Love     5351    96.8 166.602
5      Be With You Beyoncé Dangerously In Love     5351    75.6  74.934
6 Me, Myself and I Beyoncé Dangerously In Love     5351    55.5  83.615
  popularity duration_ms
1         72      235933
2         59      208600
3         57      244867
4         39      222533
5         42      260160
6         54      301173
# Print the structure
str(songs)
'data.frame':	215 obs. of  8 variables:
 $ track_name : chr  "Crazy In Love" "Naughty Girl" "Baby Boy" "Hip Hop Star" ...
 $ artist_name: chr  "Beyoncé" "Beyoncé" "Beyoncé" "Beyoncé" ...
 $ album_name : chr  "Dangerously In Love" "Dangerously In Love" "Dangerously In Love" "Dangerously In Love" ...
 $ song_age   : int  5351 5351 5351 5351 5351 5351 5351 5351 5351 5351 ...
 $ valence    : num  70.1 64.3 77.4 96.8 75.6 55.5 56.2 39.8 9.92 68.1 ...
 $ tempo      : num  99.3 100 91 166.6 74.9 ...
 $ popularity : int  72 59 57 39 42 54 43 41 41 42 ...
 $ duration_ms: int  235933 208600 244867 222533 260160 301173 259093 298533 360440 219160 ...
 
 ```
 
 ***
 
 ## Fitting a frequentist linear regression
 
 ```r
 # Create the model here
lm_model <- lm(popularity ~ song_age, data = songs)

# Produce the summary
summary(lm_model)

# Print a tidy summary of the coefficients
tidy(lm_model)
 
 
 ```
 
Output:

```bash
# Create the model here
lm_model <- lm(popularity ~ song_age, data = songs)
# Produce the summary
summary(lm_model)

Call:
lm(formula = popularity ~ song_age, data = songs)

Residuals:
    Min      1Q  Median      3Q     Max 
-38.908  -2.064   2.936   7.718  34.627 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept) 68.6899499  2.2619577  30.367  < 2e-16 ***
song_age    -0.0058526  0.0007327  -7.988 8.52e-14 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 14.9 on 213 degrees of freedom
Multiple R-squared:  0.2305,	Adjusted R-squared:  0.2269 
F-statistic: 63.81 on 1 and 213 DF,  p-value: 8.516e-14
# Print a tidy summary of the coefficients
tidy(lm_model)
# A tibble: 2 x 5
  term        estimate std.error statistic  p.value
  <chr>          <dbl>     <dbl>     <dbl>    <dbl>
1 (Intercept) 68.7      2.26         30.4  2.45e-79
2 song_age    -0.00585  0.000733     -7.99 8.52e-14

```
***

## Fitting a Bayesian linear regression

```r

# Create the model here
stan_model <- stan_glm(popularity ~ song_age, data = songs)

# Produce the summary
summary(stan_model)

# Print a tidy summary of the coefficients
tidy(stan_model)
```

Output:

```bash

# Create the model here
stan_model <- stan_glm(popularity ~ song_age, data = songs)
# Produce the summary
summary(stan_model)

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
# Print a tidy summary of the coefficients
tidy(stan_model)
# A tibble: 2 x 3
  term        estimate std.error
  <chr>          <dbl>     <dbl>
1 (Intercept) 68.7      2.26    
2 song_age    -0.00584  0.000664

```

***

## Convergence criteria
What is the accepted threshold for Rhat to conclude the model has converged?

> 1.1

***

## Difference between frequentists and Bayesians

What is the core difference between frequentists and Bayesians?

> Frequentists believe data is random, Bayesians assume parameters are random

***

## Creating credible intervals

```r

# Create the 90% credible intervals
posterior_interval(stan_model)

# Create the 95% credible intervals
posterior_interval(stan_model, prob = 0.95)

# Create the 80% credible intervals
posterior_interval(stan_model, prob = 0.80)

```

Output:

```bash

# Create the 90% credible intervals
posterior_interval(stan_model)
                      5%          95%
(Intercept) 64.961634106 72.286064792
song_age    -0.007040511 -0.004646661
sigma       13.763101640 16.214679503
# Create the 95% credible intervals
posterior_interval(stan_model, prob = 0.95)
                    2.5%        97.5%
(Intercept) 64.241161155 73.103937723
song_age    -0.007149552 -0.004480484
sigma       13.513820180 16.440518686
# Create the 80% credible intervals
posterior_interval(stan_model, prob = 0.80)
                     10%          90%
(Intercept) 65.723078923 71.476145694
song_age    -0.006718015 -0.004898744
sigma       13.988468030 15.942366163
>


```

***

*End of Chapter 1*


