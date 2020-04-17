# Random Forests

## Bagged trees vs. Random Forest

In Random Forest, only a subset of features are selected at random at each split in a decision tree. In bagging, all features are used.

## Train a Random Forest model

```r

# Train a Random Forest
set.seed(1)  # for reproducibility
credit_model <- randomForest(formula = default ~ ., 
                             data = credit_train)
                             
# Print the model output                             
print(credit_model)

```

Output:

```bash

> # Train a Random Forest
> set.seed(1)  # for reproducibility
> credit_model <- randomForest(formula = default ~ ., 
                               data = credit_train)
> 
> # Print the model output
> print(credit_model)

Call:
 randomForest(formula = default ~ ., data = credit_train) 
               Type of random forest: classification
                     Number of trees: 500
No. of variables tried at each split: 4

        OOB estimate of  error rate: 23.62%
Confusion matrix:
     no yes class.error
no  518  44  0.07829181
yes 145  93  0.60924370
> 

```
***


