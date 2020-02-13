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

