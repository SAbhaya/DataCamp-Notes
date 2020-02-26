# Chapter 2 - Evaluating and extending parallel slopes model
## R-squared vs. adjusted R-squared


![R^2 = 1 - \frac{SSE}{SST} \,,](https://render.githubusercontent.com/render/math?math=R%5E2%20%3D%201%20-%20%5Cfrac%7BSSE%7D%7BSST%7D%20%5C%2C%2C)

SSE - the sum of the squared residuals

SST - the total sum of the squares

SSE can only decrease as new variable are added to the model, while the SST depends only on the response variable and therefore is not affected by changes to the model.

increase ![R^2](https://render.githubusercontent.com/render/math?math=R%5E2) by adding any additional variable to your model—even random noise.


The adjusted ![R^2](https://render.githubusercontent.com/render/math?math=R%5E2) includes a term that penalizes a model for each additional explanatory variable (where p is the number of explanatory variables).

![R^2_{adj} = 1 - \frac{SSE}{SST} \cdot \frac{n-1}{n-p-1} \,,](https://render.githubusercontent.com/render/math?math=R%5E2_%7Badj%7D%20%3D%201%20-%20%5Cfrac%7BSSE%7D%7BSST%7D%20%5Ccdot%20%5Cfrac%7Bn-1%7D%7Bn-p-1%7D%20%5C%2C%2C)

```r

# R^2 and adjusted R^2
summary(mod)

# add random noise
mario_kart_noisy <- mario_kart %>%
  mutate(noise = rnorm(141))
  
# compute new model
mod2 <- lm(formula = totalPr ~ wheels + cond + noise, data=mario_kart_noisy)

# new R^2 and adjusted R^2
summary(mod2)

```

Output:

```bash

> # R^2 and adjusted R^2
> summary(mod)

Call:
lm(formula = totalPr ~ wheels + cond, data = mario_kart)

Residuals:
     Min       1Q   Median       3Q      Max 
-11.0078  -3.0754  -0.8254   2.9822  14.1646 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  42.3698     1.0651  39.780  < 2e-16 ***
wheels        7.2328     0.5419  13.347  < 2e-16 ***
condused     -5.5848     0.9245  -6.041 1.35e-08 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 4.887 on 138 degrees of freedom
Multiple R-squared:  0.7165,	Adjusted R-squared:  0.7124 
F-statistic: 174.4 on 2 and 138 DF,  p-value: < 2.2e-16
> 
> # add random noise
> mario_kart_noisy <- mario_kart %>%
    mutate(noise = rnorm(141))
> 
> # compute new model
> mod2 <- lm(formula = totalPr ~ wheels + cond + noise, data=mario_kart_noisy)
> 
> # new R^2 and adjusted R^2
> summary(mod2)

Call:
lm(formula = totalPr ~ wheels + cond + noise, data = mario_kart_noisy)

Residuals:
     Min       1Q   Median       3Q      Max 
-10.9831  -3.1908  -0.7462   2.9170  14.1940 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  42.3688     1.0685  39.652  < 2e-16 ***
wheels        7.2263     0.5439  13.285  < 2e-16 ***
condused     -5.5820     0.9275  -6.018 1.52e-08 ***
noise         0.1529     0.4348   0.352    0.726    
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 4.903 on 137 degrees of freedom
Multiple R-squared:  0.7168,	Adjusted R-squared:  0.7106 
F-statistic: 115.6 on 3 and 137 DF,  p-value: < 2.2e-16
> 

```
***

## Prediction


```r

# return a vector
predict(mod)

# return a data frame
data.frame(augment(mod))


```

***


## Fitting a model with interaction

> `lm(y ~ x + z + x:z, data = mydata)`

interaction between `x` and `z` will be a third term in the model.

```r

# include interaction
lm(formula = totalPr ~ cond + duration + cond:duration, data = mario_kart)

```

Output:

```bash

> # include interaction
> lm(formula = totalPr ~ cond + duration + cond:duration, data = mario_kart)

Call:
lm(formula = totalPr ~ cond + duration + cond:duration, data = mario_kart)

Coefficients:
      (Intercept)           condused           duration  condused:duration  
           58.268            -17.122             -1.966              2.325
> 


```



