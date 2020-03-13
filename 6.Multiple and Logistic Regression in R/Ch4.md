# Chapter 4 - Logistic Regression

## Fitting a line to a binary response


```r

# scatterplot with jitter
data_space <- ggplot(MedGPA, aes(x = GPA , y = Acceptance)) + 
  geom_jitter(width = 0, height = 0.05, alpha = 0.5)

# linear regression line
data_space + 
  geom_smooth(method = "lm", se = FALSE)

```

Output:

![ch4plot1](ch4plot1.png)

***

## Fitting a line to a binary response (2)

* limitation to fitting a linear regression model when we have a binary response variable


```r

# filter
MedGPA_middle <- filter(MedGPA, GPA >= 3.375 & GPA <= 3.77)

# scatterplot with jitter
data_space <- ggplot(MedGPA_middle, aes(x = GPA, y = Acceptance)) + 
  geom_jitter(width = 0, height = 0.05, alpha = 0.5)

# linear regression line
data_space + 
  geom_smooth(method = "lm", se = FALSE)
  

```

Output:

![ch4plot2](ch4plot2.png)

***

## Fitting a model

Logistic regression is a special case of a broader class of generalized linear models, often known as GLMs.

![](https://latex.codecogs.com/gif.latex?%5Clog%7B%20%5Cleft%28%20%5Cfrac%7By%7D%7B1-y%7D%20%5Cright%29%20%7D%20%3D%20%5Cbeta_0%20&plus;%20%5Cbeta_1%20%5Ccdot%20x%20&plus;%20%5Cepsilon%20%5C%2C%2C)


```r

# fit model
glm(Acceptance ~ GPA, data = MedGPA, family = binomial)


```

Output:

```bash

> # fit model
> glm(Acceptance ~ GPA, data = MedGPA, family = binomial)

Call:  glm(formula = Acceptance ~ GPA, family = binomial, data = MedGPA)

Coefficients:
(Intercept)          GPA  
    -19.207        5.454  

Degrees of Freedom: 54 Total (i.e. Null);  53 Residual
Null Deviance:	    75.79 
Residual Deviance: 56.84 	AIC: 60.84
> 

```

***

## Using geom_smooth()

```r

# scatterplot with jitter
data_space <- ggplot(MedGPA, aes(x= GPA, y=Acceptance)) + 
  geom_jitter(width = 0, height = 0.05, alpha = .5)

# add logistic curve
data_space +
  geom_smooth(method = "glm", se= FALSE, method.args = list(family = "binomial"))
  

```

Output:

![ch4plot3](ch4plot3.png)


***

## Using bins


Put the observations into bins based on their GPA value. Within each bin, we can compute the proportion of accepted students, and we can visualize our model as a smooth logistic curve through those binned values.


```r

# binned points and line
data_space <- ggplot(MedGPA_binned, aes(x = mean_GPA, y = acceptance_rate )) + geom_point()+ geom_line()

# augmented model
MedGPA_plus <- augment(mod, type.predict = "response")

# logistic model on probability scale
data_space + geom_line(data = MedGPA_plus, aes(x = GPA, y = .fitted), color = "red")

```

Output:
![ch4plot4](ch4plot4.png)

