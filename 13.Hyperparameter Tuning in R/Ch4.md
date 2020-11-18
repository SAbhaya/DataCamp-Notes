# Hyperparameter tuning with h2o
## Prepare data for modelling with h2o

```r
# Initialise h2o cluster
h2o.init()

# Convert data to h2o frame
seeds_train_data_hf <- as.h2o(seeds_train_data)

# Identify target and features
y <- "seed_type"
x <- setdiff(colnames(seeds_train_data_hf), y)

# Split data into train & validation sets
sframe <- h2o.splitFrame(seeds_train_data_hf, seed = 42)
train <- sframe[[1]]
valid <- sframe[[2]]

# Calculate ratio of the target variable in the training set
summary(train$seed_type, exact_quantiles = TRUE)

```

Output:

```bash

# Calculate ratio of the target variable in the training set
summary(train$seed_type, exact_quantiles = TRUE)
 seed_type  
 Min.   :1  
 1st Qu.:1  
 Median :2  
 Mean   :2  
 3rd Qu.:3  
 Max.   :3  
>


```
***

## Modeling with h2o

```r

# Train random forest model
rf_model <- h2o.randomForest(x = x,
                             y = y,
                             training_frame = train,
                             validation_frame = valid)

# Calculate model performance
perf <- h2o.performance(rf_model, valid = TRUE)

# Extract confusion matrix
h2o.confusionMatrix(perf)

# Extract logloss
h2o.logloss(perf)

```

Output:

```bash

# Extract confusion matrix
h2o.confusionMatrix(perf)
Confusion Matrix: Row labels: Actual class; Column labels: Predicted class
       1 2 3  Error     Rate
1      8 0 1 0.1111 =  1 / 9
2      0 5 0 0.0000 =  0 / 5
3      1 0 8 0.1111 =  1 / 9
Totals 9 5 9 0.0870 = 2 / 23
# Extract logloss
h2o.logloss(perf)
[1] 0.1717484

```

***

## Grid search with h2o

```r

# Define hyperparameters
dl_params <- list(hidden = list(c(50,50), c(100,100)),
                  epochs = c(5, 10, 15),
                  rate = c(0.001, 0.005, 0.01))

```

Which argument of the h2o.grid() function takes the hyperparameter grid as input?

> `hyper_params`

## Random search with h2o

```r

# Define search criteria
search_criteria <- list(strategy = "RandomDiscrete", 
                        max_runtime_secs = 10, # this is way too short & only used to keep runtime short!
                        seed = 42)

# Train with random search
dl_grid <- h2o.grid("deeplearning", 
                    grid_id = "dl_grid",
                    x = x, 
                    y = y,
                    training_frame = train,
                    validation_frame = valid,
                    seed = 42,
                    hyper_params = dl_params,
                    search_criteria = search_criteria)

```

***

## Stopping criteria


```r

# Define early stopping
stopping_params <- list(strategy = "RandomDiscrete", 
                        stopping_metric = "misclassification",
                        stopping_rounds = 2, 
                        stopping_tolerance = 0.1,
                        seed = 42)
 
 
 ```
 Which argument of the h2o.grid() function takes the stopping criteria as input?
 
 > search_criteria
 
 ***
 
 ## AutoML in h2o
 
 leaderboard be calculated on 3-fold cross-validated data.
 
 ```r
 
 # Run automatic machine learning
automl_model <- h2o.automl(x = x, 
                           y = y,
                           training_frame = train,
                           max_runtime_secs = 10,
                           sort_metric = "mean_per_class_error",
                           nfolds = 3,
                           seed = 42)
                           
```
 
Leaderboard be calculated based on the validation data 

```r

# Run automatic machine learning
automl_model <- h2o.automl(x = x, 
                           y = y,
                           training_frame = train,
                           max_runtime_secs = 10,
                           sort_metric = "mean_per_class_error",
                           leaderboard_frame  = valid,
                           seed = 42)
                           
 ```
 
 ***
 
 ## Extract h2o models and evaluate performance
 
 ```r
 
 # Extract the leaderboard
lb <- automl_model@leaderboard
head(lb)

# Assign best model new object name
aml_leader <- automl_model@leader

# Look at best model
summary(aml_leader)

```

Output:

```bash
# Extract the leaderboard
lb <- automl_model@leaderboard
head(lb)
                                    model_id mean_per_class_error    logloss
1  GLM_grid_0_AutoML_20181128_150051_model_0           0.02576996 0.09886840
2  GBM_grid_0_AutoML_20181128_150051_model_3           0.02782194 0.11786664
3 GBM_grid_0_AutoML_20181128_150051_model_39           0.02782194 0.27645219
4 GBM_grid_0_AutoML_20181128_150051_model_30           0.02782194 0.09548072
5  GBM_grid_0_AutoML_20181128_150051_model_1           0.02782194 0.11786664
6 GBM_grid_0_AutoML_20181128_150051_model_14           0.02782194 0.22283511
       rmse        mse
1 0.1613415 0.02603107
2 0.1706372 0.02911705
3 0.1629829 0.02656343
4 0.1633872 0.02669538
5 0.1706372 0.02911705
6 0.1622069 0.02631108

# Assign best model new object name
aml_leader <- automl_model@leader

# Look at best model
summary(aml_leader)

Model Details:
==============

H2OMultinomialModel: glm
Model Key:  GLM_grid_0_AutoML_20181128_150051_model_0 
GLM Model: summary
       family        link              regularization
1 multinomial multinomial Ridge ( lambda = 0.005697 )
                                                                   lambda_search
1 nlambda = 30, lambda.max = 41.47, lambda.min = 0.005697, lambda.1se = 0.007827
  number_of_predictors_total number_of_active_predictors number_of_iterations
1                         24                          21                   84
                   training_frame
1 automl_training_RTMP_sid_ad28_2

H2OMultinomialMetrics: glm
** Reported on training data. **

Training Set Metrics: 
=====================

Extract training frame with `h2o.getFrame("automl_training_RTMP_sid_ad28_2")`
MSE: (Extract with `h2o.mse`) 0.01293963
RMSE: (Extract with `h2o.rmse`) 0.1137525
Logloss: (Extract with `h2o.logloss`) 0.06450459
Mean Per-Class Error: 0
Null Deviance: (Extract with `h2o.nulldeviance`) 249.3896
Residual Deviance: (Extract with `h2o.residual_deviance`) 14.70705
R^2: (Extract with `h2o.r2`) 0.981488
AIC: (Extract with `h2o.aic`) NaN
Confusion Matrix: Extract with `h2o.confusionMatrix(<model>,train = TRUE)`)
=========================================================================
Confusion Matrix: Row labels: Actual class; Column labels: Predicted class
        1  2  3  Error      Rate
1      37  0  0 0.0000 =  0 / 37
2       0 34  0 0.0000 =  0 / 34
3       0  0 43 0.0000 =  0 / 43
Totals 37 34 43 0.0000 = 0 / 114

Hit Ratio Table: Extract with `h2o.hit_ratio_table(<model>,train = TRUE)`
=======================================================================
Top-3 Hit Ratios: 
  k hit_ratio
1 1  1.000000
2 2  1.000000
3 3  1.000000


H2OMultinomialMetrics: glm
** Reported on validation data. **

Validation Set Metrics: 
=====================

Extract validation frame with `h2o.getFrame("automl_validation_RTMP_sid_ad28_2")`
MSE: (Extract with `h2o.mse`) 0.03160225
RMSE: (Extract with `h2o.rmse`) 0.1777702
Logloss: (Extract with `h2o.logloss`) 0.1122946
Mean Per-Class Error: 0.02564103
Null Deviance: (Extract with `h2o.nulldeviance`) 81.62208
Residual Deviance: (Extract with `h2o.residual_deviance`) 8.085209
R^2: (Extract with `h2o.r2`) 0.940122
AIC: (Extract with `h2o.aic`) NaN
Confusion Matrix: Extract with `h2o.confusionMatrix(<model>,valid = TRUE)`)
=========================================================================
Confusion Matrix: Row labels: Actual class; Column labels: Predicted class
        1  2 3  Error     Rate
1      12  1 0 0.0769 = 1 / 13
2       0 16 0 0.0000 = 0 / 16
3       0  0 7 0.0000 =  0 / 7
Totals 12 17 7 0.0278 = 1 / 36

Hit Ratio Table: Extract with `h2o.hit_ratio_table(<model>,valid = TRUE)`
=======================================================================
Top-3 Hit Ratios: 
  k hit_ratio
1 1  0.972222
2 2  1.000000
3 3  1.000000


H2OMultinomialMetrics: glm
** Reported on cross-validation data. **
** 3-fold cross-validation on training data (Metrics computed for combined holdout predictions) **

Cross-Validation Set Metrics: 
=====================

Extract cross-validation frame with `h2o.getFrame("automl_training_RTMP_sid_ad28_2")`
MSE: (Extract with `h2o.mse`) 0.02603107
RMSE: (Extract with `h2o.rmse`) 0.1613415
Logloss: (Extract with `h2o.logloss`) 0.0988684
Mean Per-Class Error: 0.02576996
Null Deviance: (Extract with `h2o.nulldeviance`) 249.5903
Residual Deviance: (Extract with `h2o.residual_deviance`) 22.54199
R^2: (Extract with `h2o.r2`) 0.9627587
AIC: (Extract with `h2o.aic`) NaN
Hit Ratio Table: Extract with `h2o.hit_ratio_table(<model>,xval = TRUE)`
=======================================================================
Top-3 Hit Ratios: 
  k hit_ratio
1 1  0.973684
2 2  1.000000
3 3  1.000000


Cross-Validation Metrics Summary: 
                               mean           sd  cv_1_valid  cv_2_valid
accuracy                  0.9736842  0.015193428   0.9736842         1.0
err                      0.02631579  0.015193428  0.02631579         0.0
err_count                       1.0   0.57735026         1.0         0.0
logloss                 0.092005394  0.016610252  0.10174069 0.059631035
max_per_class_error      0.07936508   0.04827589 0.071428575         0.0
mean_per_class_accuracy  0.97354496  0.016091965  0.97619045         1.0
mean_per_class_error    0.026455026  0.016091965 0.023809524         0.0
mse                     0.025128312 0.0065916637  0.02879986 0.012327144
null_deviance              83.19676   0.13967834    83.24118   83.413414
r2                       0.96423197   0.00914085   0.9594273  0.98191017
residual_deviance           6.99241    1.2623792    7.732292   4.5319586
rmse                     0.15527396  0.022564467  0.16970521  0.11102767
                         cv_3_valid
accuracy                 0.94736844
err                      0.05263158
err_count                       2.0
logloss                  0.11464447
max_per_class_error      0.16666667
mean_per_class_accuracy   0.9444444
mean_per_class_error    0.055555556
mse                     0.034257933
null_deviance              82.93571
r2                       0.95135844
residual_deviance          8.712979
rmse                     0.18508899

Scoring History: 
            timestamp   duration iteration lambda predictors deviance_train
1 2018-11-28 15:00:54  0.000 sec         2  ,41E2         24          2.109
2 2018-11-28 15:00:54  0.004 sec         4   ,3E2         24          2.081
3 2018-11-28 15:00:54  0.007 sec         6  ,22E2         24          2.045
4 2018-11-28 15:00:54  0.010 sec         8  ,16E2         24          1.998
5 2018-11-28 15:00:54  0.013 sec        10  ,12E2         24          1.940
  deviance_test deviance_xval deviance_se
1         2.187         2.136       0.004
2         2.159         2.117       0.004
3         2.122         2.091       0.004
4         2.075         2.058       0.004
5         2.016         2.014       0.005

---
             timestamp   duration iteration lambda predictors deviance_train
25 2018-11-28 15:00:54  0.101 sec        67  ,2E-1         24          0.225
26 2018-11-28 15:00:54  0.105 sec        70 ,15E-1         24          0.196
27 2018-11-28 15:00:54  0.110 sec        73 ,11E-1         24          0.170
28 2018-11-28 15:00:54  0.114 sec        76 ,78E-2         24          0.148
29 2018-11-28 15:00:54  0.120 sec        80 ,57E-2         24          0.129
30 2018-11-28 15:00:54  0.126 sec        84 ,41E-2         24          0.112
   deviance_test deviance_xval deviance_se
25         0.301         0.291       0.026
26         0.275         0.261       0.026
27         0.253         0.236       0.028
28         0.237         0.215       0.029
29         0.225         0.198       0.031
30         0.216         0.000       0.000

Variable Importances: (Extract with `h2o.varimp`) 
=================================================

       variable relative_importance scaled_importance percentage
1 kernel_groove            4.039518                NA         NA
2  kernel_width            2.473810                NA         NA
3     perimeter            2.402274                NA         NA
4          area            2.188085                NA         NA
5 kernel_length            2.071675                NA         NA
6     asymmetry            1.441562                NA         NA
7   compactness            1.408899                NA         NA
8                                NA                NA         NA

```

***

*End of Chapter 4*

 

