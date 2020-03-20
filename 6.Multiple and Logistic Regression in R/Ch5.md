# Case Study: Italian restaurants in NYC
## Exploratory data analysis

Quick technique for jump-starting EDA is to examine all of the pairwise scatterplots in your data. 
This can be achieved using the `pairs()`

```r

pairs(nyc)

```


Output:

![ch5plot1](ch5plot1.png)

## SLR models


```r

# Price by Food plot
ggplot(nyc, aes(x= Food, y = Price)) + geom_point()

# Price by Food model
lm(Price ~ Food, data = nyc)

```


Output:

```bash

> # Price by Food plot
> ggplot(nyc, aes(x= Food, y = Price)) + geom_point()
> 
> # Price by Food model
> lm(Price ~ Food, data = nyc)

Call:
lm(formula = Price ~ Food, data = nyc)

Coefficients:
(Intercept)         Food  
    -17.832        2.939
> 


```
![ch5plot2](ch5plot2.png)


