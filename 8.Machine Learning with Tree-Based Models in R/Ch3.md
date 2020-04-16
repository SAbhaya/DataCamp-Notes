# Chapter 3 - Bagged Trees

## Advantages of bagged trees

* Increases the accuracy of the resulting predictions
* Reduces variance by averaging a set of observations

## Train a bagged tree model

`bagging()` function from the `ipred` package


```r
# Bagging is a randomized model, so let's set a seed (123) for reproducibility
set.seed(123)

# Train a bagged model
credit_model <- bagging(formula = default ~ ., 
                        data = credit_train,
                        coob = TRUE)

# Print the model
print(credit_model)

```

Output:

```bash

> # Bagging is a randomized model, so let's set a seed (123) for reproducibility
> set.seed(123)
> 
> # Train a bagged model
> credit_model <- bagging(formula = default ~ ., 
                          data = credit_train,
                          coob = TRUE)
> 
> # Print the model
> print(credit_model)

Bagging classification trees with 25 bootstrap replications 

Call: bagging.data.frame(formula = default ~ ., data = credit_train, 
    coob = TRUE)

Out-of-bag estimate of misclassification error:  0.2788
> 

```
