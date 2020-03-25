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

***

## Randomly order the data frame

```r

# Set seed
set.seed(42)

# Shuffle row indices: rows
rows <- sample(nrow(diamonds))

# Randomly order data
shuffled_diamonds <- diamonds[rows,]

```
***

## Try an 80/20 split

```r

# Determine row to split on: split
split <- round(nrow(diamonds) * 0.80)

# Create train
train <- diamonds[1:split,]

# Create test
test <- diamonds[(split + 1):nrow(diamonds), ]

```

***

## Predict on test set

```r

# Fit lm model on train: model
model <- lm(price ~ ., data = train)

# Predict on test: p
p <- predict(model, test)

```

***

## Calculate test set RMSE by hand

```r

# Compute errors: error
error <- p - test$price

# Calculate RMSE
sqrt(mean(error^2))

```

Output:

```bash

> # Compute errors: error
> error <- p - test$price
> 
> # Calculate RMSE
> sqrt(mean(error^2))
[1] 1136.596
> 

```



## Calculate test set RMSE by hand

Calculate test set RMSE by hand

```r

# Compute errors: error
error <- p - test$price

# Calculate RMSE
sqrt(mean(error^2))

```

Output:

```bash

> # Compute errors: error
> error <- p - test$price
> 
> # Calculate RMSE
> sqrt(mean(error^2))
[1] 1136.596
> 

```

***

## 10-fold cross-validation

the caret package makes this very easy to do cross validation

> model <- train(y ~ ., my_data)

which type of cross-validation 
the number of cross-validation folds with the trainControl() function -> pass to the trControl argument in train():

```r

model <- train(
  y ~ ., 
  my_data,
  method = "lm",
  trControl = trainControl(
    method = "cv", 
    number = 10,
    verboseIter = TRUE
  )
)

```

Fit the model using 10-fold CV

```r
# Fit lm model using 10-fold CV: model
model <- train(
  price ~ ., 
  diamonds,
  method = "lm",
  trControl = trainControl(
    method = "cv", 
    number = 10,
    verboseIter = TRUE
  )
)

# Print model to console
model
```

Output:

```bash
> # Fit lm model using 10-fold CV: model
> model <- train(
    price ~ ., 
    diamonds,
    method = "lm",
    trControl = trainControl(
      method = "cv", 
      number = 10,
      verboseIter = TRUE
    )
  )
+ Fold01: intercept=TRUE 
- Fold01: intercept=TRUE 
+ Fold02: intercept=TRUE 
- Fold02: intercept=TRUE 
+ Fold03: intercept=TRUE 
- Fold03: intercept=TRUE 
+ Fold04: intercept=TRUE 
- Fold04: intercept=TRUE 
+ Fold05: intercept=TRUE 
- Fold05: intercept=TRUE 
+ Fold06: intercept=TRUE 
- Fold06: intercept=TRUE 
+ Fold07: intercept=TRUE 
- Fold07: intercept=TRUE 
+ Fold08: intercept=TRUE 
- Fold08: intercept=TRUE 
+ Fold09: intercept=TRUE 
- Fold09: intercept=TRUE 
+ Fold10: intercept=TRUE 
- Fold10: intercept=TRUE 
Aggregating results
Fitting final model on full training set
> 
> # Print model to console
> model
Linear Regression 

25000 samples
    9 predictor

No pre-processing
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 22501, 22500, 22500, 22499, 22499, 22500, ... 
Resampling results:

  RMSE      Rsquared   MAE     
  1139.172  0.9197933  741.5344

Tuning parameter 'intercept' was held constant at a value of TRUE
> 

```

***

## 5-fold cross-validation

```r

# Fit lm model using 5-fold CV: model
model <- train(
  medv ~ ., 
  Boston,
  method = "lm",
  trControl = trainControl(
    method = "cv", 
    number = 5,
    verboseIter = TRUE
  )
)

# Print model to console
model

```

Output:

```bash
> # Fit lm model using 5-fold CV: model
> model <- train(
    medv ~ ., 
    Boston,
    method = "lm",
    trControl = trainControl(
      method = "cv", 
      number = 5,
      verboseIter = TRUE
    )
  )
+ Fold1: intercept=TRUE 
- Fold1: intercept=TRUE 
+ Fold2: intercept=TRUE 
- Fold2: intercept=TRUE 
+ Fold3: intercept=TRUE 
- Fold3: intercept=TRUE 
+ Fold4: intercept=TRUE 
- Fold4: intercept=TRUE 
+ Fold5: intercept=TRUE 
- Fold5: intercept=TRUE 
Aggregating results
Fitting final model on full training set
> 
> # Print model to console
> model
Linear Regression 

506 samples
 13 predictor

No pre-processing
Resampling: Cross-Validated (5 fold) 
Summary of sample sizes: 404, 404, 406, 406, 404 
Resampling results:

  RMSE      Rsquared   MAE     
  4.865282  0.7230966  3.389431

Tuning parameter 'intercept' was held constant at a value of TRUE
> 

```
***

## 5 x 5-fold cross-validation

Repeated cross-validation gives you a better estimate of the test-set error

```r

# Fit lm model using 5 x 5-fold CV: model
model <- train(
  medv ~ ., 
  Boston,
  method = "lm",
  trControl = trainControl(
    method = "repeatedcv", 
    number = 5,
    repeats = 5, 
    verboseIter = TRUE
  )
)

# Print model to console
model

```

Output:

```bash

> # Fit lm model using 5 x 5-fold CV: model
> model <- train(
    medv ~ ., 
    Boston,
    method = "lm",
    trControl = trainControl(
      method = "repeatedcv", 
      number = 5,
      repeats = 5, 
      verboseIter = TRUE
    )
  )
+ Fold1.Rep1: intercept=TRUE 
- Fold1.Rep1: intercept=TRUE 
+ Fold2.Rep1: intercept=TRUE 
- Fold2.Rep1: intercept=TRUE 
+ Fold3.Rep1: intercept=TRUE 
- Fold3.Rep1: intercept=TRUE 
+ Fold4.Rep1: intercept=TRUE 
- Fold4.Rep1: intercept=TRUE 
+ Fold5.Rep1: intercept=TRUE 
- Fold5.Rep1: intercept=TRUE 
+ Fold1.Rep2: intercept=TRUE 
- Fold1.Rep2: intercept=TRUE 
+ Fold2.Rep2: intercept=TRUE 
- Fold2.Rep2: intercept=TRUE 
+ Fold3.Rep2: intercept=TRUE 
- Fold3.Rep2: intercept=TRUE 
+ Fold4.Rep2: intercept=TRUE 
- Fold4.Rep2: intercept=TRUE 
+ Fold5.Rep2: intercept=TRUE 
- Fold5.Rep2: intercept=TRUE 
+ Fold1.Rep3: intercept=TRUE 
- Fold1.Rep3: intercept=TRUE 
+ Fold2.Rep3: intercept=TRUE 
- Fold2.Rep3: intercept=TRUE 
+ Fold3.Rep3: intercept=TRUE 
- Fold3.Rep3: intercept=TRUE 
+ Fold4.Rep3: intercept=TRUE 
- Fold4.Rep3: intercept=TRUE 
+ Fold5.Rep3: intercept=TRUE 
- Fold5.Rep3: intercept=TRUE 
+ Fold1.Rep4: intercept=TRUE 
- Fold1.Rep4: intercept=TRUE 
+ Fold2.Rep4: intercept=TRUE 
- Fold2.Rep4: intercept=TRUE 
+ Fold3.Rep4: intercept=TRUE 
- Fold3.Rep4: intercept=TRUE 
+ Fold4.Rep4: intercept=TRUE 
- Fold4.Rep4: intercept=TRUE 
+ Fold5.Rep4: intercept=TRUE 
- Fold5.Rep4: intercept=TRUE 
+ Fold1.Rep5: intercept=TRUE 
- Fold1.Rep5: intercept=TRUE 
+ Fold2.Rep5: intercept=TRUE 
- Fold2.Rep5: intercept=TRUE 
+ Fold3.Rep5: intercept=TRUE 
- Fold3.Rep5: intercept=TRUE 
+ Fold4.Rep5: intercept=TRUE 
- Fold4.Rep5: intercept=TRUE 
+ Fold5.Rep5: intercept=TRUE 
- Fold5.Rep5: intercept=TRUE 
Aggregating results
Fitting final model on full training set
> 
> # Print model to console
> model
Linear Regression 

506 samples
 13 predictor

No pre-processing
Resampling: Cross-Validated (5 fold, repeated 5 times) 
Summary of sample sizes: 404, 405, 405, 404, 406, 406, ... 
Resampling results:

  RMSE     Rsquared  MAE     
  4.83666  0.726918  3.391754

Tuning parameter 'intercept' was held constant at a value of TRUE
> 


```
***

## Making predictions on new data

```r
# Predict on full Boston dataset
predict(model, Boston)

```
***




