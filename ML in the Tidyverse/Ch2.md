# Multiple Models with broom
## Tidy up the coefficients of your models


```r

# Extract the coefficient statistics of each model into nested dataframes
model_coef_nested <- gap_models %>% 
    mutate(coef = map(model, ~tidy(.x)))
    
# Simplify the coef dataframes for each model    
model_coef <- model_coef_nested %>%
    unnest(coef)

# Plot a histogram of the coefficient estimates for year         
model_coef %>% 
  filter(term == "year") %>% 
  ggplot(aes(x = estimate)) +
  geom_histogram()
  
```

![hist](ch2plot1.png)


***

## Glance at the fit of your models

`glance()` to calculate how well the linear models fit the data.

```r

# Extract the fit statistics of each model into dataframes
model_perf_nested <- gap_models %>% 
    mutate(fit = map(model, ~glance(.x)))

# Simplify the fit dataframes for each model    
model_perf <- model_perf_nested %>% 
    unnest(model_perf_nested$fit)
    
# Look at the first six rows of model_perf
head(model_perf)

```

Output:

```bash

> # Extract the fit statistics of each model into dataframes
> model_perf_nested <- gap_models %>% 
      mutate(fit = map(model, ~glance(.x)))
> 
> # Simplify the fit dataframes for each model
> model_perf <- model_perf_nested %>% 
      unnest(model_perf_nested$fit)
> 
> # Look at the first six rows of model_perf
> head(model_perf)
# A tibble: 6 x 15
  country data  model fit   r.squared adj.r.squared sigma statistic  p.value
  <fct>   <lis> <lis> <lis>     <dbl>         <dbl> <dbl>     <dbl>    <dbl>
1 Algeria <tib~ <lm>  <tib~     0.952         0.951 2.18       996. 1.11e-34
2 Argent~ <tib~ <lm>  <tib~     0.984         0.984 0.431     3137. 8.78e-47
3 Austra~ <tib~ <lm>  <tib~     0.983         0.983 0.511     2905. 5.83e-46
4 Austria <tib~ <lm>  <tib~     0.987         0.986 0.438     3702. 1.48e-48
5 Bangla~ <tib~ <lm>  <tib~     0.949         0.947 1.83       921. 7.10e-34
6 Belgium <tib~ <lm>  <tib~     0.990         0.990 0.331     5094. 5.54e-52
# ... with 6 more variables: df <int>, logLik <dbl>, AIC <dbl>, BIC <dbl>,
#   deviance <dbl>, df.residual <int>
> 



```

***
