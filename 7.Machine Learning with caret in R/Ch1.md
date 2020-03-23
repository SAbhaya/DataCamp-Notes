# Machine Learning with caret in R
## Chapter 1 - Regression models: fitting them and evaluating their performance

## In-sample RMSE for linear regression on diamonds

```r
# Fit lm model: model
model <- lm(price ~ ., data = diamonds)

# Predict on full data: p
p <-  predict(model,diamonds)

# Compute errors: error
error <- p - diamonds$price

# Calculate RMSE
sqrt(mean(error^2))

```

Output:

```bash

> # Fit lm model: model
> model <- lm(price ~ ., data = diamonds)
> 
> # Predict on full data: p
> p <-  predict(model,diamonds)
> 
> # Compute errors: error
> error <- p - diamonds$price
> 
> # Calculate RMSE
> sqrt(mean(error^2))
[1] 1129.843

```
