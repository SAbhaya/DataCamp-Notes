# Polynomial Kernels
## Generating a 2d radially separable dataset


```r

#set number of variables and seed
n <- 400
set.seed(1)

#Generate data frame with two uniformly distributed predictors, x1 and x2
df <- data.frame(x1 = runif(n, min = -1, max = 1), 
                 x2 = runif(n, min = -1, max = 1))

#We want a circular boundary. Set boundary radius 
radius <- 0.8
radius_squared <- radius^2

#create dependent categorical variable, y, with value -1 or 1 depending on whether point lies
#within or outside the circle.
df$y <- factor(ifelse(df$x2^2 + df$x1^2 < radius_squared, -1, 1), levels = c(-1, 1))

```

***

## Visualizing the dataset

```r

#load ggplot
library(ggplot2)

#build scatter plot, distinguish class by color
scatter_plot <- ggplot(data = df, aes(x = x1, y = x2, color = y)) + 
    geom_point() +
    scale_color_manual(values = c("red", "blue"))

#display plot
scatter_plot

```

Output:

![ch3plot1.png](ch3plot1.png)


