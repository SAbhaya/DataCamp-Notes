# Boosted Trees
## Bagged trees vs. boosted trees

Boosted trees improve the model fit by considering past fits and bagged trees do not


## Train a GBM model

```r

# Convert "yes" to 1, "no" to 0
credit_train$default <- ifelse(credit_train$default == "yes", 1, 0)

# Train a 10000-tree GBM model
set.seed(1)
credit_model <- gbm(formula = default ~ ., 
                    distribution = "bernoulli", 
                    data = credit_train,
                    n.trees = 10000)
                    
# Print the model object                    
print(credit_model)

# summary() prints variable importance
summary(credit_model)

```

Output:

```bash
> # Convert "yes" to 1, "no" to 0
> credit_train$default <- ifelse(credit_train$default == "yes", 1, 0)
> 
> # Train a 10000-tree GBM model
> set.seed(1)
> credit_model <- gbm(formula = default ~ ., 
                      distribution = "bernoulli", 
                      data = credit_train,
                      n.trees = 10000)
> 
> # Print the model object
> print(credit_model)
gbm(formula = default ~ ., distribution = "bernoulli", data = credit_train, 
    n.trees = 10000)
A gradient boosted model with bernoulli loss function.
10000 iterations were performed.
There were 16 predictors of which 16 had non-zero influence.
> 
> # summary() prints variable importance
> summary(credit_model)
                                      var     rel.inf
checking_balance         checking_balance 33.49502510
amount                             amount 11.62938098
months_loan_duration months_loan_duration 11.17113439
credit_history             credit_history 11.15698321
savings_balance           savings_balance  6.44293358
employment_duration   employment_duration  6.06266137
age                                   age  5.73175696
percent_of_income       percent_of_income  3.74219743
other_credit                 other_credit  3.56695375
purpose                           purpose  3.38820798
housing                           housing  1.55169398
years_at_residence     years_at_residence  1.35255308
job                                   job  0.47631930
phone                               phone  0.09142691
existing_loans_count existing_loans_count  0.08924265
dependents                     dependents  0.05152933
> 


```

![ch5plot1](ch5plo1.png)

***

## Prediction using a GBM model

```r

# Since we converted the training response col, let's also convert the test response col
credit_test$default <- ifelse(credit_test$default == "yes", 1, 0)

# Generate predictions on the test set
preds1 <- predict(object = credit_model, 
                  newdata = credit_test,
                  n.trees = 10000)

# Generate predictions on the test set (scale to response)
preds2 <- predict(object = credit_model, 
                  newdata = credit_test,
                  n.trees = 10000,
                  type = "response")

# Compare the range of the two sets of predictions
range(preds1)
range(preds2)

```

Output:

```bash

> # Since we converted the training response col, let's also convert the test response col
> credit_test$default <- ifelse(credit_test$default == "yes", 1, 0)
> 
> # Generate predictions on the test set
> preds1 <- predict(object = credit_model, 
                    newdata = credit_test,
                    n.trees = 10000)
> 
> # Generate predictions on the test set (scale to response)
> preds2 <- predict(object = credit_model, 
                    newdata = credit_test,
                    n.trees = 10000,
                    type = "response")
> 
> # Compare the range of the two sets of predictions
> range(preds1)
[1] -3.210354  2.088293
> range(preds2)
[1] 0.03877796 0.88976007
> 

```

***

## Evaluate test set AUC

```r

# Generate the test set AUCs using the two sets of preditions & compare
auc(actual = credit_test$default, predicted = preds1)  #default
auc(actual = credit_test$default, predicted = preds2)  #rescaled

```
Output:

```bash

> # Generate the test set AUCs using the two sets of preditions & compare
> auc(actual = credit_test$default, predicted = preds1)  #default
[1] 0.7875175
> auc(actual = credit_test$default, predicted = preds2)  #rescaled
[1] 0.7875175
> 

```
***

