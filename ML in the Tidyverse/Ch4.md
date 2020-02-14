# Build, Tune & Evaluate Classification Models

## Prepare train-test-validate parts

```r

set.seed(42)

# Prepare the initial split object
data_split <- initial_split(attrition, prop = 0.75)

# Extract the training dataframe
training_data <- training(data_split)

# Extract the testing dataframe
testing_data <- testing(data_split)

```

***

Build a dataframe for 5-fold cross validation from the training_data using vfold_cv().
Prepare the cv_data dataframe by extracting the train and validate dataframes for each fold.

```r

set.seed(42)
cv_split <- vfold_cv(training_data, v = 5)

cv_data <- cv_split %>% 
  mutate(
    # Extract the train dataframe for each split
    train = map(splits, ~training(.x)),
    # Extract the validate dataframe for each split
    validate = map(splits, ~testing(.x))
  )
  
```

***

## Build cross-validated models

```r

# Build a model using the train data for each fold of the cross validation
cv_models_lr <- cv_data %>% 
  mutate(model = map(train, ~glm(formula = Attrition ~ ., 
                               data = .x, family = "binomial")))

```

***

## Predictions of a single model

```r

# Extract the first model and validate 
model <- cv_models_lr$model[[1]]
validate <- cv_models_lr$validate[[1]]

# Prepare binary vector of actual Attrition values in validate
validate_actual <- validate$Attrition == "Yes"

# Predict the probabilities for the observations in validate
validate_prob <- predict(model, validate, type = "response")

# Prepare binary vector of predicted Attrition values for validate
validate_predicted <- validate_prob > 0.5

```
***

## Performance of a single model

```r

library(Metrics)

# Compare the actual & predicted performance visually using a table
table(validate_actual, validate_predicted)

# Calculate the accuracy
accuracy(validate_actual, validate_predicted)

# Calculate the precision
precision(validate_actual, validate_predicted)

# Calculate the recall
recall(validate_actual, validate_predicted)

```
Output:

```bash

> library(Metrics)
> 
> # Compare the actual & predicted performance visually using a table
> table(validate_actual, validate_predicted)
               validate_predicted
validate_actual FALSE TRUE
          FALSE   176   13
          TRUE     15   17
> 
> # Calculate the accuracy
> accuracy(validate_actual, validate_predicted)
[1] 0.8733032
> 
> # Calculate the precision
> precision(validate_actual, validate_predicted)
[1] 0.5666667
> 
> # Calculate the recall
> recall(validate_actual, validate_predicted)
[1] 0.53125
> 

```
***

## Prepare for cross-validated performance




