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

