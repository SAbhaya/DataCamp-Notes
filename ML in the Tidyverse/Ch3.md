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

