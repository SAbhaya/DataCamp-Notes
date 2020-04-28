# Support Vector Machines in R

## Visualizing a sugar content dataset

```r

# Load ggplot2
library(ggplot2)
# Print variable names
colnames(df)

# Plot sugar content along the x-axis
plot_df <- ggplot(data = df, aes(x = sugar_content , y = 0)) + 
    geom_point() + 
    geom_text(aes(label = sugar_content), size = 2.5, vjust = 2, hjust = 0.5)

# Display plot
plot_df

```

Output:

```bash

> # Load ggplot2
> library(ggplot2)
> # Print variable names
> colnames(df)
[1] "sample."       "sugar_content"
> 
> # Plot sugar content along the x-axis
> plot_df <- ggplot(data = df, aes(x = sugar_content , y = 0)) + 
      geom_point() + 
      geom_text(aes(label = sugar_content), size = 2.5, vjust = 2, hjust = 0.5)
> 
> # Display plot
> plot_df
> 


```

![ch1plot1](ch1plot1.png)

***

## Find the maximal margin separator

```r
#The maximal margin separator is at the midpoint of the two extreme points in each cluster.
mm_separator <- (8.9 + 10)/2

```

***

## Visualize the maximal margin separator

```r

#create data frame containing the maximum margin separator
separator <- data.frame(sep = mm_separator)

#add ggplot layer 
plot_sep <- plot_ + geom_point(data = separator, aes(x = sep, y = 0), color = "blue", size = 4)

#display plot
plot_sep

```

Output:

![ch1plot2](ch1plot2.png)

***



