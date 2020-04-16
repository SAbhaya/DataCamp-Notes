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

