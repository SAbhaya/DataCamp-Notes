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
