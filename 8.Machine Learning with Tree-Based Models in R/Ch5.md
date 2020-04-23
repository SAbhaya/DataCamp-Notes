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

## Early stopping in GBMs

```r

# Optimal ntree estimate based on OOB
ntree_opt_oob <- gbm.perf(object = credit_model, 
                          method = "OOB", 
                          oobag.curve = TRUE)

# Train a CV GBM model
set.seed(1)
credit_model_cv <- gbm(formula = default ~ ., 
                       distribution = "bernoulli", 
                       data = credit_train,
                       n.trees = 10000,
                       cv.folds = 2)

# Optimal ntree estimate based on CV
ntree_opt_cv <- gbm.perf(object = credit_model_cv, method = "cv")
 
# Compare the estimates                         
print(paste0("Optimal n.trees (OOB Estimate): ", ntree_opt_oob))                         
print(paste0("Optimal n.trees (CV Estimate): ", ntree_opt_cv))

```

Output:

```bash

> # Optimal ntree estimate based on OOB
> ntree_opt_oob <- gbm.perf(object = credit_model, 
                            method = "OOB", 
                            oobag.curve = TRUE)
Warning message: OOB generally underestimates the optimal number of iterations although predictive performance is reasonably competitive. Using cv.folds>0 when calling gbm usually results in improved predictive performance.
> 
> # Train a CV GBM model
> set.seed(1)
> credit_model_cv <- gbm(formula = default ~ ., 
                         distribution = "bernoulli", 
                         data = credit_train,
                         n.trees = 10000,
                         cv.folds = 2)
> 
> # Optimal ntree estimate based on CV
> ntree_opt_cv <- gbm.perf(object = credit_model_cv, method = "cv")
> 
> # Compare the estimates
> print(paste0("Optimal n.trees (OOB Estimate): ", ntree_opt_oob))
[1] "Optimal n.trees (OOB Estimate): 3233"
> print(paste0("Optimal n.trees (CV Estimate): ", ntree_opt_cv))
[1] "Optimal n.trees (CV Estimate): 7889"
> 

```
![ch5plot2](ch5plo2.png)

***

## OOB vs CV-based early stopping

```r

# Generate predictions on the test set using ntree_opt_oob number of trees
preds1 <- predict(object = credit_model, 
                  newdata = credit_test,
                  n.trees = ntree_opt_oob)
                  
# Generate predictions on the test set using ntree_opt_cv number of trees
preds2 <- predict(object = credit_model, 
                  newdata = credit_test,
                  n.trees = ntree_opt_cv)   

# Generate the test set AUCs using the two sets of preditions & compare
auc1 <- auc(actual = credit_test$default, predicted = preds1)  #OOB
auc2 <- auc(actual = credit_test$default, predicted = preds2)  #CV 

# Compare AUC 
print(paste0("Test set AUC (OOB): ", auc1))                         
print(paste0("Test set AUC (CV): ", auc2))

```

Ouput:

```bash

> # Generate predictions on the test set using ntree_opt_oob number of trees
> preds1 <- predict(object = credit_model, 
                    newdata = credit_test,
                    n.trees = ntree_opt_oob)
> 
> # Generate predictions on the test set using ntree_opt_cv number of trees
> preds2 <- predict(object = credit_model, 
                    newdata = credit_test,
                    n.trees = ntree_opt_cv)
> 
> # Generate the test set AUCs using the two sets of preditions & compare
> auc1 <- auc(actual = credit_test$default, predicted = preds1)  #OOB
> auc2 <- auc(actual = credit_test$default, predicted = preds2)  #CV
> 
> # Compare AUC
> print(paste0("Test set AUC (OOB): ", auc1))
[1] "Test set AUC (OOB): 0.777816736792894"
> print(paste0("Test set AUC (CV): ", auc2))
[1] "Test set AUC (CV): 0.785530621785881"
> 

```
***

## Compare all models based on AUC

```r

# Generate the test set AUCs using the two sets of predictions & compare
actual <- credit_test$default
dt_auc <- auc(actual = actual, predicted = dt_preds)
bag_auc <- auc(actual = actual, predicted = bag_preds)
rf_auc <- auc(actual = actual, predicted = rf_preds)
gbm_auc <- auc(actual = actual, predicted = gbm_preds)

# Print results
sprintf("Decision Tree Test AUC: %.3f", dt_auc)
sprintf("Bagged Trees Test AUC: %.3f", bag_auc)
sprintf("Random Forest Test AUC: %.3f", rf_auc)
sprintf("GBM Test AUC: %.3f", gbm_auc)

```
Output:

```bash

> # Generate the test set AUCs using the two sets of predictions & compare
> actual <- credit_test$default
> dt_auc <- auc(actual = actual, predicted = dt_preds)
> bag_auc <- auc(actual = actual, predicted = bag_preds)
> rf_auc <- auc(actual = actual, predicted = rf_preds)
> gbm_auc <- auc(actual = actual, predicted = gbm_preds)
> 
> # Print results
> sprintf("Decision Tree Test AUC: %.3f", dt_auc)
[1] "Decision Tree Test AUC: 0.627"
> sprintf("Bagged Trees Test AUC: %.3f", bag_auc)
[1] "Bagged Trees Test AUC: 0.781"
> sprintf("Random Forest Test AUC: %.3f", rf_auc)
[1] "Random Forest Test AUC: 0.804"
> sprintf("GBM Test AUC: %.3f", gbm_auc)
[1] "GBM Test AUC: 0.786"
> 

```
## Plot & compare ROC curves


```r

# List of predictions
preds_list <- list(dt_preds, bag_preds, rf_preds, gbm_preds)

# List of actual values (same for all)
m <- length(preds_list)
actuals_list <- rep(list(credit_test$default), m)

# Plot the ROC curves
pred <- prediction(preds_list, actuals_list)
rocs <- performance(pred, "tpr", "fpr")
plot(rocs, col = as.list(1:m), main = "Test Set ROC Curves")
legend(x = "bottomright", 
       legend = c("Decision Tree", "Bagged Trees", "Random Forest", "GBM"),
       fill = 1:m)
       
```

Output:

![ch5plot3](ch5plo3.png)


***

*End Of Chapter 5*
