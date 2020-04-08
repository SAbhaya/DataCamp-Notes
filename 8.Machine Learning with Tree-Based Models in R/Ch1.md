# Chapter 1 - Classification Trees

## Build a classification tree

```r

# Look at the data
str(creditsub)

# Create the model
credit_model <- rpart(formula = default ~ ., 
                      data = creditsub, 
                      method = "class")

# Display the results
rpart.plot(x = credit_model, yesno = 2, type = 0, extra = 0)

```

Output:

```bash

# Look at the data
str(creditsub)

# Create the model
credit_model <- rpart(formula = default ~ ., 
                      data = creditsub, 
                      method = "class")

# Display the results
rpart.plot(x = credit_model, yesno = 2, type = 0, extra = 0)

```

Plot:

![ch1plot1](ch1plot1.png)

***

## Advantages of tree-based methods

What are some advantages of using tree-based methods over other supervised learning methods?

Model interpretability (easy to understand why a prediction is made).
No pre-processing (e.g. normalization) of the data is required.

***
## Train/test split



