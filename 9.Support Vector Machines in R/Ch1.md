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



