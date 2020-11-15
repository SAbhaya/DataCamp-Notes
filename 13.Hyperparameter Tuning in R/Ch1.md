# Chapter 1 - Introduction to hyperparameters

## Model parameters vs. hyperparameters

```r

# Fit a linear model on the breast_cancer_data.
linear_model <- lm(concavity_mean ~ symmetry_mean, breast_cancer_data)

# Look at the summary of the linear_model.
summary(linear_model)

# Extract the coefficients.
coefficients(linear_model)

```

Output:

```bash

# Fit a linear model on the breast_cancer_data.
linear_model <- lm(concavity_mean ~ symmetry_mean, breast_cancer_data)
# Look at the summary of the linear_model.
summary(linear_model)

Call:
lm(formula = concavity_mean ~ symmetry_mean, data = breast_cancer_data)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.201877 -0.039201 -0.008432  0.030655  0.226150 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)   -0.15311    0.04086  -3.747 0.000303 ***
symmetry_mean  1.33366    0.21257   6.274 9.57e-09 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.06412 on 98 degrees of freedom
Multiple R-squared:  0.2866,	Adjusted R-squared:  0.2793 
F-statistic: 39.36 on 1 and 98 DF,  p-value: 9.575e-09
# Extract the coefficients.
coefficients(linear_model)
  (Intercept) symmetry_mean 
   -0.1531055     1.3336568 


```
***

## What are the coefficients?

```r

library(ggplot2)

# Plot linear relationship.
ggplot(data = breast_cancer_data, 
        aes(x = symmetry_mean, y = concavity_mean)) +
  geom_point(color = "grey") +
  geom_abline(slope = linear_model$coefficients[2], 
      intercept = linear_model$coefficients[1])
      
```

Output:

![ch1plot1](ch1plot1.png)

***

## Machine learning with caret


```r# Create partition index
index <- createDataPartition(breast_cancer_data$diagnosis, p = 0.7, list = FALSE)

# Subset `breast_cancer_data` with index
bc_train_data <- breast_cancer_data[index, ]
bc_test_data  <- breast_cancer_data[-index, ]

# Define 3x5 folds repeated cross-validation
fitControl <- trainControl(method = "repeatedcv", number = 5, repeats = 3)

# Run the train() function
gbm_model <- train(diagnosis ~ ., 
                   data = bc_train_data, 
                   method = "gbm", 
                   trControl = fitControl,
                   verbose = FALSE)

# Look at the model
gbm_model

```

Output:

```bash

# Look at the model
gbm_model
Stochastic Gradient Boosting 

70 samples
10 predictors
 2 classes: 'B', 'M' 

No pre-processing
Resampling: Cross-Validated (5 fold, repeated 3 times) 
Summary of sample sizes: 56, 56, 56, 56, 56, 56, ... 
Resampling results across tuning parameters:

  interaction.depth  n.trees  Accuracy   Kappa    
  1                   50      0.9000000  0.8000000
  1                  100      0.8857143  0.7714286
  1                  150      0.8904762  0.7809524
  2                   50      0.8952381  0.7904762
  2                  100      0.8904762  0.7809524
  2                  150      0.8857143  0.7714286
  3                   50      0.8952381  0.7904762
  3                  100      0.8952381  0.7904762
  3                  150      0.8904762  0.7809524

Tuning parameter 'shrinkage' was held constant at a value of 0.1

Tuning parameter 'n.minobsinnode' was held constant at a value of 10
Accuracy was used to select the optimal model using the largest value.
The final values used for the model were n.trees = 50, interaction.depth =
 1, shrinkage = 0.1 and n.minobsinnode = 10.
>

```

***




