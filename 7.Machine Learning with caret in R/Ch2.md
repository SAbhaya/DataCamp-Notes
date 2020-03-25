# Chapter 2 - Classification models: fitting them and evaluating their performance
## Try a 60/40 split

```r

# Get the number of observations
n_obs <- nrow(Sonar)

# Shuffle row indices: permuted_rows
permuted_rows <- sample(n_obs)
 
# Randomly order data: Sonar - Sonar_shuffled
Sonar_shuffled <- Sonar[permuted_rows,]

# Identify row to split on: split
split <- round(n_obs * 0.6)

# Create train
train <- Sonar_shuffled[1:split,]

# Create test
test <- Sonar_shuffled[(split + 1):nrow(Sonar_shuffled),]

```

***

## Fit a logistic regression model


```r

# Fit glm model: model
model <- glm(Class ~ .,family = "binomial", train)

# Predict on test: p
p <-  predict(model, test, type = "response")

```
***

## Calculate a confusion matrix

Before you make your confusion matrix, you need to "cut" your predicted probabilities at a given threshold to turn probabilities into a factor of class predictions. Combine ifelse() with factor() as follows:

```r
pos_or_neg <- ifelse(probability_prediction > threshold, positive_class, negative_class)
p_class <- factor(pos_or_neg, levels = levels(test_values))

confusionMatrix(p_class, test_values)

```

```r

# If p exceeds threshold of 0.5, M else R: m_or_r
m_or_r <- ifelse( p > 0.5, "M","R")

# Convert to factor: p_class

p_class <- factor(m_or_r, levels = levels(test[["Class"]]))

# Create confusion matrix
confusionMatrix(p_class, test[["Class"]])

```

Output:

```bash

> # If p exceeds threshold of 0.5, M else R: m_or_r
> m_or_r <- ifelse( p > 0.5, "M","R")
> 
> # Convert to factor: p_class
> 
> p_class <- factor(m_or_r, levels = levels(test[["Class"]]))
> 
> # Create confusion matrix
> confusionMatrix(p_class, test[["Class"]])
Confusion Matrix and Statistics

          Reference
Prediction  M  R
         M 40 17
         R  8 18
                                          
               Accuracy : 0.6988          
                 95% CI : (0.5882, 0.7947)
    No Information Rate : 0.5783          
    P-Value [Acc > NIR] : 0.01616         
                                          
                  Kappa : 0.3602          
                                          
 Mcnemar's Test P-Value : 0.10960         
                                          
            Sensitivity : 0.8333          
            Specificity : 0.5143          
         Pos Pred Value : 0.7018          
         Neg Pred Value : 0.6923          
             Prevalence : 0.5783          
         Detection Rate : 0.4819          
   Detection Prevalence : 0.6867          
      Balanced Accuracy : 0.6738          
                                          
       'Positive' Class : M
> 


```


