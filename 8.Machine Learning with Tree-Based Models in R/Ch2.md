# Chapter 2 - Regression Trees

## Classification vs. regression


* In classification, the response represents a category (e.g. "apples", "oranges", "bananas")
* In regression, the response represents a numeric value (e.g. price of a house).

## Split the data

```r

# Look at the data
str(grade)

# Set seed and create assignment
set.seed(1)
assignment <- sample(1:3, size = nrow(grade), prob = c(0.7,0.15,0.15), replace = TRUE)

# Create a train, validation and tests from the original data frame 
grade_train <- grade[assignment == 1, ]    # subset grade to training indices only
grade_valid <- grade[assignment == 2, ]  # subset grade to validation indices only
grade_test <- grade[assignment == 3, ]   # subset grade to test indices only

```

Output:

```bash

# Look at the data
str(grade)

# Set seed and create assignment
set.seed(1)
assignment <- sample(1:3, size = nrow(grade), prob = c(0.7,0.15,0.15), replace = TRUE)

# Create a train, validation and tests from the original data frame 
grade_train <- grade[assignment == 1, ]    # subset grade to training indices only
grade_valid <- grade[assignment == 2, ]  # subset grade to validation indices only
grade_test <- grade[assignment == 3, ]   # subset grade to test indices only

```

***

## Train a regression tree model

Fitting a classification tree:
```
method = "class"

```
Fitting a regression tree:
```
method = "anova"

```
***

```r

# Train the model
grade_model <- rpart(formula = final_grade ~ ., 
                     data = grade_train, 
                     method = "anova")

# Look at the model output                      
print(grade_model)

# Plot the tree model
rpart.plot(x = grade_model, yesno = 2, type = 0, extra = 0)

```

Output:

```bash
> # Train the model
> grade_model <- rpart(formula = final_grade ~ ., 
                       data = grade_train, 
                       method = "anova")
> 
> # Look at the model output
> print(grade_model)
n= 282 

node), split, n, deviance, yval
      * denotes terminal node

 1) root 282 1519.49700 5.271277  
   2) absences< 0.5 82  884.18600 4.323171  
     4) paid=no 50  565.50500 3.430000  
       8) famsup=yes 22  226.36360 2.272727 *
       9) famsup=no 28  286.52680 4.339286 *
     5) paid=yes 32  216.46880 5.718750  
      10) age>=17.5 10   82.90000 4.100000 *
      11) age< 17.5 22   95.45455 6.454545 *
   3) absences>=0.5 200  531.38000 5.660000  
     6) absences>=13.5 42  111.61900 4.904762 *
     7) absences< 13.5 158  389.43670 5.860759  
      14) schoolsup=yes 23   50.21739 4.847826 *
      15) schoolsup=no 135  311.60000 6.033333  
        30) studytime< 3.5 127  276.30710 5.940945 *
        31) studytime>=3.5 8   17.00000 7.500000 *
> 
> # Plot the tree model
> rpart.plot(x = grade_model, yesno = 2, type = 0, extra = 0)
> 

```

![ch2plot1](ch2plot1.png)









