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


