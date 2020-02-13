# Build, Tune & Evaluate Regression Models

## The test-train split


Use `rsample` package to split the data

> Random split of the data it is good practice to set a seed before splitting it.

```r

set.seed(42)

# Prepare the initial split object
gap_split <- initial_split(gapminder, prop = 0.75)

# Extract the training dataframe
training_data <- training(gap_split)

# Extract the testing dataframe
testing_data <- testing(gap_split)

# Calculate the dimensions of both training_data and testing_data
dim(training_data)
dim(testing_data)

```

Output:

```bash

> set.seed(42)
> 
> # Prepare the initial split object
> gap_split <- initial_split(gapminder, prop = 0.75)
> 
> # Extract the training dataframe
> training_data <- training(gap_split)
> 
> # Extract the testing dataframe
> testing_data <- testing(gap_split)
> 
> # Calculate the dimensions of both training_data and testing_data
> dim(training_data)
[1] 3003    7
> dim(testing_data)
[1] 1001    7
> 

```
***

## Cross-validation dataframes

Split the training data into a series of 5 train-validate sets using the `vfold_cv()` function from the `rsample` package.


```r

set.seed(42)

# Prepare the dataframe containing the cross validation partitions
cv_split <- vfold_cv(training_data, v = 5)

cv_data <- cv_split %>% 
  mutate(
    # Extract the train dataframe for each split
    train = map(splits, ~training(.x)), 
    # Extract the validate dataframe for each split
    validate = map(splits, ~testing(.x))
  )

# Use head() to preview cv_data
head(cv_data)

```

Output:

```bash

> set.seed(42)
> 
> # Prepare the dataframe containing the cross validation partitions
> cv_split <- vfold_cv(training_data, v = 5)
> 
> cv_data <- cv_split %>% 
    mutate(
      # Extract the train dataframe for each split
      train = map(splits, ~training(.x)), 
      # Extract the validate dataframe for each split
      validate = map(splits, ~testing(.x))
    )
> 
> # Use head() to preview cv_data
> head(cv_data)
# A tibble: 5 x 4
  splits   id    train                validate          
* <list>   <chr> <list>               <list>            
1 <rsplit> Fold1 <tibble [2,402 x 7]> <tibble [601 x 7]>
2 <rsplit> Fold2 <tibble [2,402 x 7]> <tibble [601 x 7]>
3 <rsplit> Fold3 <tibble [2,402 x 7]> <tibble [601 x 7]>
4 <rsplit> Fold4 <tibble [2,403 x 7]> <tibble [600 x 7]>
5 <rsplit> Fold5 <tibble [2,403 x 7]> <tibble [600 x 7]>
> 

```
***

## Build cross-validated models

```r

# Build a model using the train data for each fold of the cross validation
cv_models_lm <- cv_data %>% 
  mutate(model = map(train, ~lm(formula = life_expectancy ~ ., data = .x)))

```
***

## Preparing for evaluation


```r

cv_prep_lm <- cv_models_lm %>% 
  mutate(
    # Extract the recorded life expectancy for the records in the validate dataframes
    validate_actual = map(validate, ~.x$life_expectancy),
    # Predict life expectancy for each validate set using its corresponding model
    validate_predicted = map2(.x = model, .y = validate, ~predict(.x, .y))
  )
 
```
***
## Evaluate model performance



* Measure Mean Absolute Error (MAE) between these two vectors

```r

library(Metrics)
# Calculate the mean absolute error for each validate fold       
cv_eval_lm <- cv_prep_lm %>% 
  mutate(validate_mae = map2_dbl(validate_actual, validate_predicted, ~mae(actual = .x, predicted = .y)))

# Print the validate_mae column
cv_eval_lm$validate_mae

# Calculate the mean of validate_mae column
mean(cv_eval_lm$validate_mae)

```

Output:

```bash

> library(Metrics)
> # Calculate the mean absolute error for each validate fold
> cv_eval_lm <- cv_prep_lm %>% 
    mutate(validate_mae = map2_dbl(validate_actual, validate_predicted, ~mae(actual = .x, predicted = .y)))
> 
> # Print the validate_mae column
> cv_eval_lm$validate_mae
       1        2        3        4        5 
1.477976 1.491672 1.541386 1.403481 1.435348
> 
> # Calculate the mean of validate_mae column
> mean(cv_eval_lm$validate_mae)
[1] 1.469972
> 

```
***

## Building and tuning a random forest model

## Build a random forest model

```r

library(ranger)

# Build a random forest model for each fold
cv_models_rf <- cv_data %>% 
  mutate(model = map(train, ~ranger(formula = life_expectancy ~ ., data = .x, num.trees = 100, seed = 42)))

# Generate predictions using the random forest model
cv_prep_rf <- cv_models_rf %>% 
  mutate(validate_predicted = map2(.x = model, .y = validate, ~predict(.x, .y)$predictions))

```
***

## Evaluate a random forest model

```r

library(ranger)

# Calculate validate MAE for each fold
cv_eval_rf <- cv_prep_rf %>% 
  mutate(validate_mae = map2_dbl(validate_actual, validate_predicted, ~mae(actual = .x, predicted = .y)))

# Print the validate_mae column
cv_eval_rf$validate_mae

# Calculate the mean of validate_mae column
mean(cv_eval_rf$validate_mae)

```

Output:

```bash

> library(ranger)
> 
> # Calculate validate MAE for each fold
> cv_eval_rf <- cv_prep_rf %>% 
    mutate(validate_mae = map2_dbl(validate_actual, validate_predicted, ~mae(actual = .x, predicted = .y)))
> 
> # Print the validate_mae column
> cv_eval_rf$validate_mae
        1         2         3         4         5 
0.8336129 0.8162233 0.8096234 0.7584762 0.7605624
> 
> # Calculate the mean of validate_mae column
> mean(cv_eval_rf$validate_mae)
[1] 0.7956996
> 


```
***

## Fine tune your model


```r

# Prepare for tuning your cross validation folds by varying mtry
cv_tune <- cv_data %>% 
  crossing(mtry = 2:5) 

# Build a model for each fold & mtry combination
cv_model_tunerf <- cv_tune %>% 
  mutate(model = map2(.x = train, .y = mtry, ~ranger(formula = life_expectancy~.,data = .x, mtry = .y,num.trees = 100, seed = 42)))


```
***

## The best performing parameter

```r

# Generate validate predictions for each model
cv_prep_tunerf <- cv_model_tunerf %>% 
  mutate(validate_predicted = map2(.x = model, .y = validate, ~predict(.x, .y)$predictions))

# Calculate validate MAE for each fold and mtry combination
cv_eval_tunerf <- cv_prep_tunerf %>% 
  mutate(validate_mae = map2_dbl(.x = validate_actual, .y = validate_predicted, ~mae(actual = .x, predicted = .y)))

# Calculate the mean validate_mae for each mtry used  
cv_eval_tunerf %>% 
  group_by(mtry) %>% 
  summarise(mean_mae = mean(validate_mae))

```

Output:

```bash

> # Generate validate predictions for each model
> cv_prep_tunerf <- cv_model_tunerf %>% 
    mutate(validate_predicted = map2(.x = model, .y = validate, ~predict(.x, .y)$predictions))
> 
> # Calculate validate MAE for each fold and mtry combination
> cv_eval_tunerf <- cv_prep_tunerf %>% 
    mutate(validate_mae = map2_dbl(.x = validate_actual, .y = validate_predicted, ~mae(actual = .x, predicted = .y)))
> 
> # Calculate the mean validate_mae for each mtry used
> cv_eval_tunerf %>% 
    group_by(mtry) %>% 
    summarise(mean_mae = mean(validate_mae))
# A tibble: 4 x 2
   mtry mean_mae
  <int>    <dbl>
1     2    0.796
2     3    0.791
3     4    0.789
4     5    0.791
> 

```

***

## Build & evaluate the best model

```r
# Build the model using all training data and the best performing parameter
best_model <- ranger(formula = life_expectancy ~ ., data = training_data,
                     mtry = 4, num.trees = 100, seed = 42)

# Prepare the test_actual vector
test_actual <- testing_data$life_expectancy

# Predict life_expectancy for the testing_data
test_predicted <- predict(best_model, testing_data)$predictions

# Calculate the test MAE
mae(test_actual, test_predicted)

```

Output:

```bash

> # Build the model using all training data and the best performing parameter
> best_model <- ranger(formula = life_expectancy ~ ., data = training_data,
                       mtry = 4, num.trees = 100, seed = 42)
> 
> # Prepare the test_actual vector
> test_actual <- testing_data$life_expectancy
> 
> # Predict life_expectancy for the testing_data
> test_predicted <- predict(best_model, testing_data)$predictions
> 
> # Calculate the test MAE
> mae(test_actual, test_predicted)
[1] 0.6785683
> 


```




