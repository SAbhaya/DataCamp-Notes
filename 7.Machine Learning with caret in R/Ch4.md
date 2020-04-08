# Preprocessing your data
## Apply median imputation

`train()` function in `caret` contains an argument called `preProcess`

x is an object with samples in rows and features in columns (without outcomes)
y is a numeric or factor vector containing the outcomes.

```r
# Apply median imputation: median_model
median_model <- train(
  x = breast_cancer_x, 
  y = breast_cancer_y,
  method = "glm",
  trControl = myControl,
  preProcess = "medianImpute"
)

# Print median_model to console
median_model

```
Outcome:

```bash
> # Apply median imputation: median_model
> median_model <- train(
    x = breast_cancer_x, 
    y = breast_cancer_y,
    method = "glm",
    trControl = myControl,
    preProcess = "medianImpute"
  )
Warning message: The metric "Accuracy" was not in the result set. ROC will be used instead.
+ Fold01: parameter=none 
- Fold01: parameter=none 
+ Fold02: parameter=none 
- Fold02: parameter=none 
+ Fold03: parameter=none 
- Fold03: parameter=none 
+ Fold04: parameter=none 
- Fold04: parameter=none 
+ Fold05: parameter=none 
- Fold05: parameter=none 
+ Fold06: parameter=none 
- Fold06: parameter=none 
+ Fold07: parameter=none 
- Fold07: parameter=none 
+ Fold08: parameter=none 
- Fold08: parameter=none 
+ Fold09: parameter=none 
- Fold09: parameter=none 
+ Fold10: parameter=none 
- Fold10: parameter=none 
Aggregating results
Fitting final model on full training set
> 
> # Print median_model to console
> median_model
Generalized Linear Model 

699 samples
  9 predictor
  2 classes: 'benign', 'malignant' 

Pre-processing: median imputation (9) 
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 629, 629, 629, 629, 629, 629, ... 
Resampling results:

  ROC       Sens      Spec     
  0.992755  0.967343  0.9378333
> 

```
***

## Use KNN imputation

Advanced form of imputation where missing values are replaced with values from other rows that are similar to the current row.

```r

# Apply KNN imputation: knn_model
knn_model <- train(
  x = breast_cancer_x, 
  y = breast_cancer_y,
  method = "glm",
  trControl = myControl,
  preProcess = "knnImpute"
)

# Print knn_model to console
knn_model

```

Output:

```bash

> # Apply KNN imputation: knn_model
> knn_model <- train(
    x = breast_cancer_x, 
    y = breast_cancer_y,
    method = "glm",
    trControl = myControl,
    preProcess = "knnImpute"
  )
Warning message: The metric "Accuracy" was not in the result set. ROC will be used instead.
+ Fold01: parameter=none 
- Fold01: parameter=none 
+ Fold02: parameter=none 
- Fold02: parameter=none 
+ Fold03: parameter=none 
- Fold03: parameter=none 
+ Fold04: parameter=none 
- Fold04: parameter=none 
+ Fold05: parameter=none 
- Fold05: parameter=none 
+ Fold06: parameter=none 
- Fold06: parameter=none 
+ Fold07: parameter=none 
- Fold07: parameter=none 
+ Fold08: parameter=none 
- Fold08: parameter=none 
+ Fold09: parameter=none 
- Fold09: parameter=none 
+ Fold10: parameter=none 
- Fold10: parameter=none 
Aggregating results
Fitting final model on full training set
> 
> # Print knn_model to console
> knn_model
Generalized Linear Model 

699 samples
  9 predictor
  2 classes: 'benign', 'malignant' 

Pre-processing: nearest neighbor imputation (9), centered (9), scaled (9) 
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 629, 629, 630, 630, 629, 629, ... 
Resampling results:

  ROC        Sens       Spec     
  0.9924396  0.9715459  0.9416667
> 

```
***

## Compare KNN and median imputation

```r

dotplot(resamples, metric = "ROC")

```

Output:

![ch4plot1](ch4plot1.png)

***

## Combining preprocessing methods

## 1

```r

# Fit glm with median imputation
model <- train(
  x = breast_cancer_x, 
  y = breast_cancer_y,
  method = "glm",
  trControl = myControl,
  preProcess = "medianImpute"
)

# Print model
model

```
Output:

```bash

> # Fit glm with median imputation
> model <- train(
    x = breast_cancer_x, 
    y = breast_cancer_y,
    method = "glm",
    trControl = myControl,
    preProcess = "medianImpute"
  )
Warning message: The metric "Accuracy" was not in the result set. ROC will be used instead.
+ Fold01: parameter=none 
- Fold01: parameter=none 
+ Fold02: parameter=none 
- Fold02: parameter=none 
+ Fold03: parameter=none 
- Fold03: parameter=none 
+ Fold04: parameter=none 
- Fold04: parameter=none 
+ Fold05: parameter=none 
- Fold05: parameter=none 
+ Fold06: parameter=none 
- Fold06: parameter=none 
+ Fold07: parameter=none 
- Fold07: parameter=none 
+ Fold08: parameter=none 
- Fold08: parameter=none 
+ Fold09: parameter=none 
- Fold09: parameter=none 
+ Fold10: parameter=none 
- Fold10: parameter=none 
Aggregating results
Fitting final model on full training set
> 
> # Print model
> model
Generalized Linear Model 

699 samples
  9 predictor
  2 classes: 'benign', 'malignant' 

Pre-processing: median imputation (9) 
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 629, 630, 629, 630, 629, 628, ... 
Resampling results:

  ROC        Sens      Spec     
  0.9929992  0.967343  0.9461667
> 

```
## 2

```r

# Update model with standardization
model <- train(
  x = breast_cancer_x, 
  y = breast_cancer_y,
  method = "glm",
  trControl = myControl,
  preProcess = c("medianImpute","center","scale")
)

# Print updated model
model

```

Output:

```bash

> # Update model with standardization
> model <- train(
    x = breast_cancer_x, 
    y = breast_cancer_y,
    method = "glm",
    trControl = myControl,
    preProcess = c("medianImpute","center","scale")
  )
Warning message: The metric "Accuracy" was not in the result set. ROC will be used instead.
+ Fold01: parameter=none 
- Fold01: parameter=none 
+ Fold02: parameter=none 
- Fold02: parameter=none 
+ Fold03: parameter=none 
- Fold03: parameter=none 
+ Fold04: parameter=none 
- Fold04: parameter=none 
+ Fold05: parameter=none 
- Fold05: parameter=none 
+ Fold06: parameter=none 
- Fold06: parameter=none 
+ Fold07: parameter=none 
- Fold07: parameter=none 
+ Fold08: parameter=none 
- Fold08: parameter=none 
+ Fold09: parameter=none 
- Fold09: parameter=none 
+ Fold10: parameter=none 
- Fold10: parameter=none 
Aggregating results
Fitting final model on full training set
> 
> # Print updated model
> model
Generalized Linear Model 

699 samples
  9 predictor
  2 classes: 'benign', 'malignant' 

Pre-processing: median imputation (9), centered (9), scaled (9) 
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 630, 629, 629, 630, 629, 629, ... 
Resampling results:

  ROC        Sens       Spec     
  0.9916832  0.9694203  0.9418333
> 

```
***

## Remove near zero variance predictors

Using `caret` function `nearZeroVar()`

`nearZeroVar()` takes data `x`

ratio of the most common value to the second most common value: `freqCut`

and the percentage of distinct values out of the number of total samples, `uniqueCut`

default:

```
freqCut = 19
uniqueCut = 10

```

recommended:

```
freqCut = 2
uniqueCut = 20
```

```r

# Identify near zero variance predictors: remove_cols
remove_cols <- nearZeroVar(bloodbrain_x, names = TRUE, 
                           freqCut = 2, uniqueCut = 20)

# Get all column names from bloodbrain_x: all_cols
all_cols <-  names(bloodbrain_x)

# Remove from data: bloodbrain_x_small
bloodbrain_x_small <- bloodbrain_x[ , setdiff(all_cols, remove_cols)]

```

***

## preProcess() and nearZeroVar()

use the `preProcess` argument in caret to remove near-zero variance predictors
Set the `preProcess` argument equal to `"nzv"`.


***

## Fit model on reduced blood-brain data

```r

# Fit model on reduced data: model
model <- train(
  x = bloodbrain_x_small, 
  y = bloodbrain_y, 
  method = "glm"
)

# Print model to console
model

```

Output:

```bash
> # Fit model on reduced data: model
> model <- train(
    x = bloodbrain_x_small, 
    y = bloodbrain_y, 
    method = "glm"
  )
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
Warning message: prediction from a rank-deficient fit may be misleading
> 
> # Print model to console
> model
Generalized Linear Model 

208 samples
112 predictors

No pre-processing
Resampling: Bootstrapped (25 reps) 
Summary of sample sizes: 208, 208, 208, 208, 208, 208, ... 
Resampling results:

  RMSE      Rsquared   MAE     
  1.583861  0.1134041  1.052184
> 

```

***

## Using PCA as an alternative to nearZeroVar()

```r

# Fit glm model using PCA: model
model <- train(
  x = bloodbrain_x, 
  y = bloodbrain_y,
  method = "glm", 
  preProcess = "pca"
)

# Print model to console
model

```

Output:

```bash

> # Fit glm model using PCA: model
> model <- train(
    x = bloodbrain_x, 
    y = bloodbrain_y,
    method = "glm", 
    preProcess = "pca"
  )
> 
> # Print model to console
> model
Generalized Linear Model 

208 samples
132 predictors

Pre-processing: principal component signal extraction (132), centered
 (132), scaled (132) 
Resampling: Bootstrapped (25 reps) 
Summary of sample sizes: 208, 208, 208, 208, 208, 208, ... 
Resampling results:

  RMSE       Rsquared   MAE      
  0.6116041  0.4192338  0.4617475
> 

```
***
