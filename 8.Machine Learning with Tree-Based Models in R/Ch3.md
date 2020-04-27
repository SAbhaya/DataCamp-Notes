# Chapter 3 - Bagged Trees

## Advantages of bagged trees

* Increases the accuracy of the resulting predictions
* Reduces variance by averaging a set of observations

## Train a bagged tree model

`bagging()` function from the `ipred` package


```r
# Bagging is a randomized model, so let's set a seed (123) for reproducibility
set.seed(123)

# Train a bagged model
credit_model <- bagging(formula = default ~ ., 
                        data = credit_train,
                        coob = TRUE)

# Print the model
print(credit_model)

```

Output:

```bash

> # Bagging is a randomized model, so let's set a seed (123) for reproducibility
> set.seed(123)
> 
> # Train a bagged model
> credit_model <- bagging(formula = default ~ ., 
                          data = credit_train,
                          coob = TRUE)
> 
> # Print the model
> print(credit_model)

Bagging classification trees with 25 bootstrap replications 

Call: bagging.data.frame(formula = default ~ ., data = credit_train, 
    coob = TRUE)

Out-of-bag estimate of misclassification error:  0.2788
> 

```
## Prediction and confusion matrix

```r

# Generate predicted classes using the model object
class_prediction <- predict(object = credit_model,    
                            newdata = credit_test,  
                            type = "class")  # return classification labels

# Print the predicted classes
print(class_prediction)

# Calculate the confusion matrix for the test set
confusionMatrix(data = class_prediction,       
                reference = credit_test$default)  
                
                
```

Output:

```bash

> # Generate predicted classes using the model object
> class_prediction <- predict(object = credit_model,    
                              newdata = credit_test,  
                              type = "class")  # return classification labels
> 
> # Print the predicted classes
> print(class_prediction)
  [1] no  yes yes no  no  yes no  yes no  no  no  yes no  yes no  no  no  no 
 [19] no  no  no  no  no  yes no  no  no  yes no  yes yes yes no  no  no  no 
 [37] no  no  no  no  no  no  no  yes no  no  no  yes no  yes yes no  no  yes
 [55] no  no  no  no  no  no  no  no  no  no  no  yes no  no  no  no  yes no 
 [73] no  yes no  no  no  no  no  no  no  no  no  no  no  no  no  no  no  no 
 [91] yes no  yes no  no  no  no  yes no  no  no  no  no  no  yes no  no  no 
[109] no  no  no  no  no  no  no  no  no  no  no  no  no  no  yes no  no  no 
[127] no  no  no  no  no  no  no  no  no  no  no  no  no  no  yes no  yes no 
[145] yes no  no  no  no  no  no  no  yes no  no  no  no  no  no  no  no  yes
[163] no  no  no  no  yes yes no  no  no  no  yes yes no  no  no  no  no  no 
[181] no  yes no  no  no  no  yes no  no  no  no  yes no  no  no  no  yes no 
[199] no  yes
Levels: no yes
> 
> # Calculate the confusion matrix for the test set
> confusionMatrix(data = class_prediction,       
                  reference = credit_test$default)
Confusion Matrix and Statistics

          Reference
Prediction  no yes
       no  126  36
       yes  12  26
                                          
               Accuracy : 0.76            
                 95% CI : (0.6947, 0.8174)
    No Information Rate : 0.69            
    P-Value [Acc > NIR] : 0.0178277       
                                          
                  Kappa : 0.3721          
 Mcnemar's Test P-Value : 0.0009009       
                                          
            Sensitivity : 0.9130          
            Specificity : 0.4194          
         Pos Pred Value : 0.7778          
         Neg Pred Value : 0.6842          
             Prevalence : 0.6900          
         Detection Rate : 0.6300          
   Detection Prevalence : 0.8100          
      Balanced Accuracy : 0.6662          
                                          
       'Positive' Class : no
> 

```

***

## Predict on a test set and compute AUC

```r

# Generate predictions on the test set
pred <- predict(object = credit_model,
                newdata = credit_test,
                type = "prob")

# `pred` is a matrix
class(pred)
                
# Look at the pred format
head(pred)
                
# Compute the AUC (`actual` must be a binary (or 1/0 numeric) vector)
auc(actual = ifelse(credit_test$default == "yes", 1, 0), 
    predicted = pred[,"yes"])                    
    
```

Output:

```bash

> # Generate predictions on the test set
> pred <- predict(object = credit_model,
                  newdata = credit_test,
                  type = "prob")
> 
> # `pred` is a matrix
> class(pred)
[1] "matrix"
> 
> # Look at the pred format
> head(pred)
       no  yes
[1,] 0.96 0.04
[2,] 0.28 0.72
[3,] 0.36 0.64
[4,] 0.76 0.24
[5,] 0.92 0.08
[6,] 0.48 0.52
> 
> # Compute the AUC (`actual` must be a binary (or 1/0 numeric) vector)
> auc(actual = ifelse(credit_test$default == "yes", 1, 0), 
      predicted = pred[,"yes"])
[1] 0.7809724
> 

```

***

## Cross-validate a bagged tree model in caret

```r

# Specify the training configuration
ctrl <- trainControl(method = "cv",     # Cross-validation
                     number = 5,      # 5 folds
                     classProbs = TRUE,                  # For AUC
                     summaryFunction = twoClassSummary)  # For AUC

# Cross validate the credit model using "treebag" method; 
# Track AUC (Area under the ROC curve)
set.seed(1)  # for reproducibility
credit_caret_model <- train(default ~ .,
                            data = credit_train, 
                            method = "treebag",
                            metric = "ROC",
                            trControl = ctrl)

# Look at the model object
print(credit_caret_model)

# Inspect the contents of the model list 
names(credit_caret_model)

# Print the CV AUC
credit_caret_model$results[,"ROC"]

```

Output:

```bash

> # Specify the training configuration
> ctrl <- trainControl(method = "cv",     # Cross-validation
                       number = 5,      # 5 folds
                       classProbs = TRUE,                  # For AUC
                       summaryFunction = twoClassSummary)  # For AUC
> 
> # Cross validate the credit model using "treebag" method;
> # Track AUC (Area under the ROC curve)
> set.seed(1)  # for reproducibility
> credit_caret_model <- train(default ~ .,
                              data = credit_train, 
                              method = "treebag",
                              metric = "ROC",
                              trControl = ctrl)
> 
> # Look at the model object
> print(credit_caret_model)
Bagged CART 

800 samples
 16 predictor
  2 classes: 'no', 'yes' 

No pre-processing
Resampling: Cross-Validated (5 fold) 
Summary of sample sizes: 641, 640, 640, 639, 640 
Resampling results:

  ROC        Sens       Spec     
  0.7203687  0.8275126  0.4417553
> 
> # Inspect the contents of the model list
> names(credit_caret_model)
 [1] "method"       "modelInfo"    "modelType"    "results"      "pred"        
 [6] "bestTune"     "call"         "dots"         "metric"       "control"     
[11] "finalModel"   "preProcess"   "trainingData" "resample"     "resampledCM" 
[16] "perfNames"    "maximize"     "yLimits"      "times"        "levels"      
[21] "terms"        "coefnames"    "contrasts"    "xlevels"
> 
> # Print the CV AUC
> credit_caret_model$results[,"ROC"]
[1] 0.7203687
> 

```
***

## Generate predictions from the caret model

```r

# Generate predictions on the test set
pred <- predict(object = credit_caret_model, 
                newdata = credit_test,
                type = "prob")

# Compute the AUC (`actual` must be a binary (or 1/0 numeric) vector)
auc(actual = ifelse( credit_test$default == "yes", 1, 0), 
                    predicted = pred[,"yes"])
                    
```

Output:

```bash

> # Generate predictions on the test set
> pred <- predict(object = credit_caret_model, 
                  newdata = credit_test,
                  type = "prob")
> 
> # Compute the AUC (`actual` must be a binary (or 1/0 numeric) vector)
> auc(actual = ifelse( credit_test$dependents == "yes", 1, 0), 
                      predicted = pred[,"yes"])
[1] NaN
> # Generate predictions on the test set
> pred <- predict(object = credit_caret_model, 
                  newdata = credit_test,
                  type = "prob")
> 
> # Compute the AUC (`actual` must be a binary (or 1/0 numeric) vector)
> auc(actual = ifelse( credit_test$default == "yes", 1, 0), 
                      predicted = pred[,"yes"])
[1] 0.7762389
> 


```

## Compare test set performance to CV performance

```r

# Print ipred::bagging test set AUC estimate
print(credit_ipred_model_test_auc)

# Print caret "treebag" test set AUC estimate
print(credit_caret_model_test_auc)
                
# Compare to caret 5-fold cross-validated AUC
credit_caret_model$results[,"ROC"]


```

Output:

```bash

> # Print ipred::bagging test set AUC estimate
> print(credit_ipred_model_test_auc)
[1] 0.7809724
> 
> # Print caret "treebag" test set AUC estimate
> print(credit_caret_model_test_auc)
[1] 0.7762389
> 
> # Compare to caret 5-fold cross-validated AUC
> credit_caret_model$results[,"ROC"]
[1] 0.7203687
> 

```




