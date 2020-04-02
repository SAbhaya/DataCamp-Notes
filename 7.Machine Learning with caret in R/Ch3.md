# Chapter 3 - Tuning model parameters to improve performance
## Fit a random forest


```r

# Fit random forest: model
model <- train(
  quality ~ .,
  tuneLength = 1,
  data = wine, 
  method = "ranger",
  trControl = trainControl(
    method = "cv", 
    number = 5, 
    verboseIter = TRUE
  )
)

# Print model to console
model

```

Output:

```bash

> # Fit random forest: model
> model <- train(
    quality ~ .,
    tuneLength = 1,
    data = wine, 
    method = "ranger",
    trControl = trainControl(
      method = "cv", 
      number = 5, 
      verboseIter = TRUE
    )
  )
+ Fold1: mtry=3, min.node.size=5, splitrule=variance 
- Fold1: mtry=3, min.node.size=5, splitrule=variance 
+ Fold1: mtry=3, min.node.size=5, splitrule=extratrees 
- Fold1: mtry=3, min.node.size=5, splitrule=extratrees 
+ Fold2: mtry=3, min.node.size=5, splitrule=variance 
- Fold2: mtry=3, min.node.size=5, splitrule=variance 
+ Fold2: mtry=3, min.node.size=5, splitrule=extratrees 
- Fold2: mtry=3, min.node.size=5, splitrule=extratrees 
+ Fold3: mtry=3, min.node.size=5, splitrule=variance 
- Fold3: mtry=3, min.node.size=5, splitrule=variance 
+ Fold3: mtry=3, min.node.size=5, splitrule=extratrees 
- Fold3: mtry=3, min.node.size=5, splitrule=extratrees 
+ Fold4: mtry=3, min.node.size=5, splitrule=variance 
- Fold4: mtry=3, min.node.size=5, splitrule=variance 
+ Fold4: mtry=3, min.node.size=5, splitrule=extratrees 
- Fold4: mtry=3, min.node.size=5, splitrule=extratrees 
+ Fold5: mtry=3, min.node.size=5, splitrule=variance 
- Fold5: mtry=3, min.node.size=5, splitrule=variance 
+ Fold5: mtry=3, min.node.size=5, splitrule=extratrees 
- Fold5: mtry=3, min.node.size=5, splitrule=extratrees 
Aggregating results
Selecting tuning parameters
Fitting mtry = 3, splitrule = variance, min.node.size = 5 on full training set
> 
> # Print model to console
> model
Random Forest 

100 samples
 12 predictor

No pre-processing
Resampling: Cross-Validated (5 fold) 
Summary of sample sizes: 79, 81, 80, 80, 80 
Resampling results across tuning parameters:

  splitrule   RMSE       Rsquared   MAE      
  variance    0.6472815  0.3129299  0.4958393
  extratrees  0.6790625  0.2429503  0.5139353

Tuning parameter 'mtry' was held constant at a value of 3
Tuning
 parameter 'min.node.size' was held constant at a value of 5
RMSE was used to select the optimal model using the smallest value.
The final values used for the model were mtry = 3, splitrule = variance
 and min.node.size = 5.
> 

```
***

## Try a longer tune length

```r

# Fit random forest: model
model <- train(
  quality ~ .,
  tuneLength = 3,
  data = wine, 
  method = "ranger",
  trControl = trainControl(
    method = "cv", 
    number = 5, 
    verboseIter = TRUE
  )
)

# Print model to console
model

# Plot model
plot(model)

```

Output:

```bash

> # Fit random forest: model
> model <- train(
    quality ~ .,
    tuneLength = 3,
    data = wine, 
    method = "ranger",
    trControl = trainControl(
      method = "cv", 
      number = 5, 
      verboseIter = TRUE
    )
  )
+ Fold1: mtry= 2, min.node.size=5, splitrule=variance 
- Fold1: mtry= 2, min.node.size=5, splitrule=variance 
+ Fold1: mtry= 7, min.node.size=5, splitrule=variance 
- Fold1: mtry= 7, min.node.size=5, splitrule=variance 
+ Fold1: mtry=12, min.node.size=5, splitrule=variance 
- Fold1: mtry=12, min.node.size=5, splitrule=variance 
+ Fold1: mtry= 2, min.node.size=5, splitrule=extratrees 
- Fold1: mtry= 2, min.node.size=5, splitrule=extratrees 
+ Fold1: mtry= 7, min.node.size=5, splitrule=extratrees 
- Fold1: mtry= 7, min.node.size=5, splitrule=extratrees 
+ Fold1: mtry=12, min.node.size=5, splitrule=extratrees 
- Fold1: mtry=12, min.node.size=5, splitrule=extratrees 
+ Fold2: mtry= 2, min.node.size=5, splitrule=variance 
- Fold2: mtry= 2, min.node.size=5, splitrule=variance 
+ Fold2: mtry= 7, min.node.size=5, splitrule=variance 
- Fold2: mtry= 7, min.node.size=5, splitrule=variance 
+ Fold2: mtry=12, min.node.size=5, splitrule=variance 
- Fold2: mtry=12, min.node.size=5, splitrule=variance 
+ Fold2: mtry= 2, min.node.size=5, splitrule=extratrees 
- Fold2: mtry= 2, min.node.size=5, splitrule=extratrees 
+ Fold2: mtry= 7, min.node.size=5, splitrule=extratrees 
- Fold2: mtry= 7, min.node.size=5, splitrule=extratrees 
+ Fold2: mtry=12, min.node.size=5, splitrule=extratrees 
- Fold2: mtry=12, min.node.size=5, splitrule=extratrees 
+ Fold3: mtry= 2, min.node.size=5, splitrule=variance 
- Fold3: mtry= 2, min.node.size=5, splitrule=variance 
+ Fold3: mtry= 7, min.node.size=5, splitrule=variance 
- Fold3: mtry= 7, min.node.size=5, splitrule=variance 
+ Fold3: mtry=12, min.node.size=5, splitrule=variance 
- Fold3: mtry=12, min.node.size=5, splitrule=variance 
+ Fold3: mtry= 2, min.node.size=5, splitrule=extratrees 
- Fold3: mtry= 2, min.node.size=5, splitrule=extratrees 
+ Fold3: mtry= 7, min.node.size=5, splitrule=extratrees 
- Fold3: mtry= 7, min.node.size=5, splitrule=extratrees 
+ Fold3: mtry=12, min.node.size=5, splitrule=extratrees 
- Fold3: mtry=12, min.node.size=5, splitrule=extratrees 
+ Fold4: mtry= 2, min.node.size=5, splitrule=variance 
- Fold4: mtry= 2, min.node.size=5, splitrule=variance 
+ Fold4: mtry= 7, min.node.size=5, splitrule=variance 
- Fold4: mtry= 7, min.node.size=5, splitrule=variance 
+ Fold4: mtry=12, min.node.size=5, splitrule=variance 
- Fold4: mtry=12, min.node.size=5, splitrule=variance 
+ Fold4: mtry= 2, min.node.size=5, splitrule=extratrees 
- Fold4: mtry= 2, min.node.size=5, splitrule=extratrees 
+ Fold4: mtry= 7, min.node.size=5, splitrule=extratrees 
- Fold4: mtry= 7, min.node.size=5, splitrule=extratrees 
+ Fold4: mtry=12, min.node.size=5, splitrule=extratrees 
- Fold4: mtry=12, min.node.size=5, splitrule=extratrees 
+ Fold5: mtry= 2, min.node.size=5, splitrule=variance 
- Fold5: mtry= 2, min.node.size=5, splitrule=variance 
+ Fold5: mtry= 7, min.node.size=5, splitrule=variance 
- Fold5: mtry= 7, min.node.size=5, splitrule=variance 
+ Fold5: mtry=12, min.node.size=5, splitrule=variance 
- Fold5: mtry=12, min.node.size=5, splitrule=variance 
+ Fold5: mtry= 2, min.node.size=5, splitrule=extratrees 
- Fold5: mtry= 2, min.node.size=5, splitrule=extratrees 
+ Fold5: mtry= 7, min.node.size=5, splitrule=extratrees 
- Fold5: mtry= 7, min.node.size=5, splitrule=extratrees 
+ Fold5: mtry=12, min.node.size=5, splitrule=extratrees 
- Fold5: mtry=12, min.node.size=5, splitrule=extratrees 
Aggregating results
Selecting tuning parameters
Fitting mtry = 7, splitrule = extratrees, min.node.size = 5 on full training set
> 
> # Print model to console
> model
Random Forest 

100 samples
 12 predictor

No pre-processing
Resampling: Cross-Validated (5 fold) 
Summary of sample sizes: 81, 80, 81, 79, 79 
Resampling results across tuning parameters:

  mtry  splitrule   RMSE       Rsquared   MAE      
   2    variance    0.7085709  0.4461494  0.5953364
   2    extratrees  0.7132270  0.4591507  0.5883883
   7    variance    0.7214439  0.4197318  0.6093089
   7    extratrees  0.6933306  0.4774437  0.5802985
  12    variance    0.7343570  0.4089651  0.6237680
  12    extratrees  0.6938384  0.4718821  0.5806953

Tuning parameter 'min.node.size' was held constant at a value of 5
RMSE was used to select the optimal model using the smallest value.
The final values used for the model were mtry = 7, splitrule = extratrees
 and min.node.size = 5.
> 
> # Plot model
> plot(model)
> 

```

![ch3plot1](ch3plot1.png)

***

## Fit a random forest with custom tuning

### 1
```r
# Define the tuning grid: tuneGrid
tuneGrid <- data.frame(
  .mtry = c(2,3,7),
  .splitrule = "variance",
  .min.node.size = 5
)

```
### 2

```r

# From previous step
tuneGrid <- data.frame(
  .mtry = c(2, 3, 7),
  .splitrule = "variance",
  .min.node.size = 5
)

# Fit random forest: model
model <- train(
  quality ~ .,
  tuneGrid = tuneGrid,
  data = wine, 
  method = "ranger",
  trControl = trainControl(
    method = "cv", 
    number = 5, 
    verboseIter = TRUE
  )
)

# Print model to console
model

# Plot model
plot(model)

```

Output:

```bash
> # From previous step
> tuneGrid <- data.frame(
    .mtry = c(2, 3, 7),
    .splitrule = "variance",
    .min.node.size = 5
  )
> 
> # Fit random forest: model
> model <- train(
    quality ~ .,
    tuneGrid = tuneGrid,
    data = wine, 
    method = "ranger",
    trControl = trainControl(
      method = "cv", 
      number = 5, 
      verboseIter = TRUE
    )
  )
+ Fold1: mtry=2, splitrule=variance, min.node.size=5 
- Fold1: mtry=2, splitrule=variance, min.node.size=5 
+ Fold1: mtry=3, splitrule=variance, min.node.size=5 
- Fold1: mtry=3, splitrule=variance, min.node.size=5 
+ Fold1: mtry=7, splitrule=variance, min.node.size=5 
- Fold1: mtry=7, splitrule=variance, min.node.size=5 
+ Fold2: mtry=2, splitrule=variance, min.node.size=5 
- Fold2: mtry=2, splitrule=variance, min.node.size=5 
+ Fold2: mtry=3, splitrule=variance, min.node.size=5 
- Fold2: mtry=3, splitrule=variance, min.node.size=5 
+ Fold2: mtry=7, splitrule=variance, min.node.size=5 
- Fold2: mtry=7, splitrule=variance, min.node.size=5 
+ Fold3: mtry=2, splitrule=variance, min.node.size=5 
- Fold3: mtry=2, splitrule=variance, min.node.size=5 
+ Fold3: mtry=3, splitrule=variance, min.node.size=5 
- Fold3: mtry=3, splitrule=variance, min.node.size=5 
+ Fold3: mtry=7, splitrule=variance, min.node.size=5 
- Fold3: mtry=7, splitrule=variance, min.node.size=5 
+ Fold4: mtry=2, splitrule=variance, min.node.size=5 
- Fold4: mtry=2, splitrule=variance, min.node.size=5 
+ Fold4: mtry=3, splitrule=variance, min.node.size=5 
- Fold4: mtry=3, splitrule=variance, min.node.size=5 
+ Fold4: mtry=7, splitrule=variance, min.node.size=5 
- Fold4: mtry=7, splitrule=variance, min.node.size=5 
+ Fold5: mtry=2, splitrule=variance, min.node.size=5 
- Fold5: mtry=2, splitrule=variance, min.node.size=5 
+ Fold5: mtry=3, splitrule=variance, min.node.size=5 
- Fold5: mtry=3, splitrule=variance, min.node.size=5 
+ Fold5: mtry=7, splitrule=variance, min.node.size=5 
- Fold5: mtry=7, splitrule=variance, min.node.size=5 
Aggregating results
Selecting tuning parameters
Fitting mtry = 2, splitrule = variance, min.node.size = 5 on full training set
> 
> # Print model to console
> model
Random Forest 

100 samples
 12 predictor

No pre-processing
Resampling: Cross-Validated (5 fold) 
Summary of sample sizes: 79, 80, 81, 80, 80 
Resampling results across tuning parameters:

  mtry  RMSE       Rsquared   MAE      
  2     0.6737366  0.2261653  0.5110236
  3     0.6737869  0.2260317  0.5094642
  7     0.6776795  0.2210202  0.5125127

Tuning parameter 'splitrule' was held constant at a value of variance

Tuning parameter 'min.node.size' was held constant at a value of 5
RMSE was used to select the optimal model using the smallest value.
The final values used for the model were mtry = 2, splitrule = variance
 and min.node.size = 5.
> 
> # Plot model
> plot(model)
> 

```

![ch3plot2](ch3plot2.png)

***

## Introduction to glmnet
## Advantage of glmnet

> glmnet models place constraints on your coefficients, which helps prevent overfitting.

## Make a custom trainControl

Classification problems -> provide a custom summaryFunction to the train() function to use the AUC metric.

1. make custom trainControl
2. set classProbs = TRUE
3. set summaryFunction = twoClassSummary

```r
# Create custom trainControl: myControl
myControl <- trainControl(
  method = "cv", 
  number = 10,
  summaryFunction = twoClassSummary,
  classProbs = TRUE, # IMPORTANT!
  verboseIter = TRUE
)

```

***

## Fit glmnet with custom trainControl

```r

# Fit glmnet model: model
model <- train(
  y ~ ., 
  overfit,
  method = "glmnet",
  trControl = myControl
)

# Print model to console

model

# Print maximum ROC statistic
max(model[["results"]]$ROC)

```




Output:

```bash

> # Fit glmnet model: model
> model <- train(
    y ~ ., 
    overfit,
    method = "glmnet",
    trControl = myControl
  )
Warning message: The metric "Accuracy" was not in the result set. ROC will be used instead.
+ Fold01: alpha=0.10, lambda=0.01013 
- Fold01: alpha=0.10, lambda=0.01013 
+ Fold01: alpha=0.55, lambda=0.01013 
- Fold01: alpha=0.55, lambda=0.01013 
+ Fold01: alpha=1.00, lambda=0.01013 
- Fold01: alpha=1.00, lambda=0.01013 
+ Fold02: alpha=0.10, lambda=0.01013 
- Fold02: alpha=0.10, lambda=0.01013 
+ Fold02: alpha=0.55, lambda=0.01013 
- Fold02: alpha=0.55, lambda=0.01013 
+ Fold02: alpha=1.00, lambda=0.01013 
- Fold02: alpha=1.00, lambda=0.01013 
+ Fold03: alpha=0.10, lambda=0.01013 
- Fold03: alpha=0.10, lambda=0.01013 
+ Fold03: alpha=0.55, lambda=0.01013 
- Fold03: alpha=0.55, lambda=0.01013 
+ Fold03: alpha=1.00, lambda=0.01013 
- Fold03: alpha=1.00, lambda=0.01013 
+ Fold04: alpha=0.10, lambda=0.01013 
- Fold04: alpha=0.10, lambda=0.01013 
+ Fold04: alpha=0.55, lambda=0.01013 
- Fold04: alpha=0.55, lambda=0.01013 
+ Fold04: alpha=1.00, lambda=0.01013 
- Fold04: alpha=1.00, lambda=0.01013 
+ Fold05: alpha=0.10, lambda=0.01013 
- Fold05: alpha=0.10, lambda=0.01013 
+ Fold05: alpha=0.55, lambda=0.01013 
- Fold05: alpha=0.55, lambda=0.01013 
+ Fold05: alpha=1.00, lambda=0.01013 
- Fold05: alpha=1.00, lambda=0.01013 
+ Fold06: alpha=0.10, lambda=0.01013 
- Fold06: alpha=0.10, lambda=0.01013 
+ Fold06: alpha=0.55, lambda=0.01013 
- Fold06: alpha=0.55, lambda=0.01013 
+ Fold06: alpha=1.00, lambda=0.01013 
- Fold06: alpha=1.00, lambda=0.01013 
+ Fold07: alpha=0.10, lambda=0.01013 
- Fold07: alpha=0.10, lambda=0.01013 
+ Fold07: alpha=0.55, lambda=0.01013 
- Fold07: alpha=0.55, lambda=0.01013 
+ Fold07: alpha=1.00, lambda=0.01013 
- Fold07: alpha=1.00, lambda=0.01013 
+ Fold08: alpha=0.10, lambda=0.01013 
- Fold08: alpha=0.10, lambda=0.01013 
+ Fold08: alpha=0.55, lambda=0.01013 
- Fold08: alpha=0.55, lambda=0.01013 
+ Fold08: alpha=1.00, lambda=0.01013 
- Fold08: alpha=1.00, lambda=0.01013 
+ Fold09: alpha=0.10, lambda=0.01013 
- Fold09: alpha=0.10, lambda=0.01013 
+ Fold09: alpha=0.55, lambda=0.01013 
- Fold09: alpha=0.55, lambda=0.01013 
+ Fold09: alpha=1.00, lambda=0.01013 
- Fold09: alpha=1.00, lambda=0.01013 
+ Fold10: alpha=0.10, lambda=0.01013 
- Fold10: alpha=0.10, lambda=0.01013 
+ Fold10: alpha=0.55, lambda=0.01013 
- Fold10: alpha=0.55, lambda=0.01013 
+ Fold10: alpha=1.00, lambda=0.01013 
- Fold10: alpha=1.00, lambda=0.01013 
Aggregating results
Selecting tuning parameters
Fitting alpha = 0.55, lambda = 0.0101 on full training set
> 
> # Print model to console
> 
> model
glmnet 

250 samples
200 predictors
  2 classes: 'class1', 'class2' 

No pre-processing
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 226, 225, 226, 225, 224, 226, ... 
Resampling results across tuning parameters:

  alpha  lambda        ROC        Sens  Spec     
  0.10   0.0001012745  0.4525362  0     0.9742754
  0.10   0.0010127448  0.4590580  0     0.9827899
  0.10   0.0101274483  0.4632246  0     0.9913043
  0.55   0.0001012745  0.4198370  0     0.9523551
  0.55   0.0010127448  0.4240942  0     0.9567029
  0.55   0.0101274483  0.4675725  0     0.9784420
  1.00   0.0001012745  0.3983696  0     0.9222826
  1.00   0.0010127448  0.4134964  0     0.9353261
  1.00   0.0101274483  0.4096014  0     0.9827899

ROC was used to select the optimal model using the largest value.
The final values used for the model were alpha = 0.55 and lambda = 0.01012745.
> 
> # Print maximum ROC statistic
> max(model[["results"]]$ROC)
[1] 0.4675725
> 

```
***

## glmnet with custom trainControl and tuning

The glmnet model actually fits many models at once

Exmple tuning grid for `glmnet`

```r
expand.grid(
  alpha = 0:1,
  lambda = seq(0.0001, 1, length = 100)
)

```

For fewer models:

lambda = seq(0.0001, 1, length = 10)

```r

# Train glmnet with custom trainControl and tuning: model
model <- train(
  y ~., 
  overfit,
  tuneGrid = expand.grid(
    alpha = 0:1,
    lambda = seq(0.0001, 1, length = 20)
  ),
  method = "glmnet",
  trControl = myControl
)

# Print model to console

model

# Print maximum ROC statistic
max(model[["results"]][["ROC"]])

```

Output:

```bash

> # Train glmnet with custom trainControl and tuning: model
> model <- train(
    y ~., 
    overfit,
    tuneGrid = expand.grid(
      alpha = 0:1,
      lambda = seq(0.0001, 1, length = 20)
    ),
    method = "glmnet",
    trControl = myControl
  )
Warning message: The metric "Accuracy" was not in the result set. ROC will be used instead.
+ Fold01: alpha=0, lambda=1 
- Fold01: alpha=0, lambda=1 
+ Fold01: alpha=1, lambda=1 
- Fold01: alpha=1, lambda=1 
+ Fold02: alpha=0, lambda=1 
- Fold02: alpha=0, lambda=1 
+ Fold02: alpha=1, lambda=1 
- Fold02: alpha=1, lambda=1 
+ Fold03: alpha=0, lambda=1 
- Fold03: alpha=0, lambda=1 
+ Fold03: alpha=1, lambda=1 
- Fold03: alpha=1, lambda=1 
+ Fold04: alpha=0, lambda=1 
- Fold04: alpha=0, lambda=1 
+ Fold04: alpha=1, lambda=1 
- Fold04: alpha=1, lambda=1 
+ Fold05: alpha=0, lambda=1 
- Fold05: alpha=0, lambda=1 
+ Fold05: alpha=1, lambda=1 
- Fold05: alpha=1, lambda=1 
+ Fold06: alpha=0, lambda=1 
- Fold06: alpha=0, lambda=1 
+ Fold06: alpha=1, lambda=1 
- Fold06: alpha=1, lambda=1 
+ Fold07: alpha=0, lambda=1 
- Fold07: alpha=0, lambda=1 
+ Fold07: alpha=1, lambda=1 
- Fold07: alpha=1, lambda=1 
+ Fold08: alpha=0, lambda=1 
- Fold08: alpha=0, lambda=1 
+ Fold08: alpha=1, lambda=1 
- Fold08: alpha=1, lambda=1 
+ Fold09: alpha=0, lambda=1 
- Fold09: alpha=0, lambda=1 
+ Fold09: alpha=1, lambda=1 
- Fold09: alpha=1, lambda=1 
+ Fold10: alpha=0, lambda=1 
- Fold10: alpha=0, lambda=1 
+ Fold10: alpha=1, lambda=1 
- Fold10: alpha=1, lambda=1 
Aggregating results
Selecting tuning parameters
Fitting alpha = 1, lambda = 0.0527 on full training set
> 
> # Print model to console
> 
> model
glmnet 

250 samples
200 predictors
  2 classes: 'class1', 'class2' 

No pre-processing
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 226, 225, 225, 224, 225, 226, ... 
Resampling results across tuning parameters:

  alpha  lambda      ROC        Sens  Spec     
  0      0.00010000  0.3859601  0.00  0.9782609
  0      0.05272632  0.3898551  0.00  1.0000000
  0      0.10535263  0.4026268  0.00  1.0000000
  0      0.15797895  0.4025362  0.00  1.0000000
  0      0.21060526  0.4024457  0.00  1.0000000
  0      0.26323158  0.4131341  0.00  1.0000000
  0      0.31585789  0.4153080  0.00  1.0000000
  0      0.36848421  0.4152174  0.00  1.0000000
  0      0.42111053  0.4130435  0.00  1.0000000
  0      0.47373684  0.4153080  0.00  1.0000000
  0      0.52636316  0.4174819  0.00  1.0000000
  0      0.57898947  0.4196558  0.00  1.0000000
  0      0.63161579  0.4196558  0.00  1.0000000
  0      0.68424211  0.4175725  0.00  1.0000000
  0      0.73686842  0.4175725  0.00  1.0000000
  0      0.78949474  0.4175725  0.00  1.0000000
  0      0.84212105  0.4240036  0.00  1.0000000
  0      0.89474737  0.4240036  0.00  1.0000000
  0      0.94737368  0.4240036  0.00  1.0000000
  0      1.00000000  0.4240036  0.00  1.0000000
  1      0.00010000  0.3312500  0.05  0.9402174
  1      0.05272632  0.5282609  0.00  1.0000000
  1      0.10535263  0.5000000  0.00  1.0000000
  1      0.15797895  0.5000000  0.00  1.0000000
  1      0.21060526  0.5000000  0.00  1.0000000
  1      0.26323158  0.5000000  0.00  1.0000000
  1      0.31585789  0.5000000  0.00  1.0000000
  1      0.36848421  0.5000000  0.00  1.0000000
  1      0.42111053  0.5000000  0.00  1.0000000
  1      0.47373684  0.5000000  0.00  1.0000000
  1      0.52636316  0.5000000  0.00  1.0000000
  1      0.57898947  0.5000000  0.00  1.0000000
  1      0.63161579  0.5000000  0.00  1.0000000
  1      0.68424211  0.5000000  0.00  1.0000000
  1      0.73686842  0.5000000  0.00  1.0000000
  1      0.78949474  0.5000000  0.00  1.0000000
  1      0.84212105  0.5000000  0.00  1.0000000
  1      0.89474737  0.5000000  0.00  1.0000000
  1      0.94737368  0.5000000  0.00  1.0000000
  1      1.00000000  0.5000000  0.00  1.0000000

ROC was used to select the optimal model using the largest value.
The final values used for the model were alpha = 1 and lambda = 0.05272632.
> 
> # Print maximum ROC statistic
> max(model[["results"]][["ROC"]])
[1] 0.5282609
> 

```


