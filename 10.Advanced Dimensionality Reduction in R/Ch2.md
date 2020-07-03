# Chapter 2 - Introduction to t-SNE

## Computing t-SNE

```r

# Compute t-SNE without doing the PCA step
tsne_output <- Rtsne(mnist_sample[,-1], PCA = FALSE, dim = 3)

# Show the obtained embedding coordinates
head(tsne_output$Y)

# Store the first two coordinates and plot them 
tsne_plot <- data.frame(tsne_x = tsne_output$Y[, 1], tsne_y = tsne_output$Y[, 2], 
                        digit = as.factor(mnist_sample$label))

# Plot the coordinates
ggplot(tsne_plot, aes(x = tsne_x, y = tsne_y, color = digit)) + 
	ggtitle("t-SNE of MNIST sample") + 
	geom_text(aes(label = digit)) + 
	theme(legend.position = "none")
  
```
Output:

![ch2plot1](ch2plot1.png)

***

## Understanding t-SNE output

### 1

```r

# Inspect the output object's structure
str(tsne_output)

# Show total costs after each 50th iteration
tsne_output$itercosts

# Plot the evolution of the KL divergence at each 50th iteration
plot(tsne_output$itercosts, type = "l")

```

Output:

```bash
> # Inspect the output object's structure
> str(tsne_output)
List of 13
 $ theta              : num 0.5
 $ perplexity         : num 30
 $ N                  : int 200
 $ origD              : int 50
 $ Y                  : num [1:200, 1:3] -8.02 -11.56 1.53 -14.81 2.34 ...
 $ costs              : num [1:200] 0.002645 0.001583 0.000228 0.00227 0.002102 ...
 $ itercosts          : num [1:20] 53.9 53.2 53.6 53.6 53.2 ...
 $ stop_lying_iter    : int 250
 $ mom_switch_iter    : int 250
 $ momentum           : num 0.5
 $ final_momentum     : num 0.8
 $ eta                : num 200
 $ exaggeration_factor: num 12
> 
> # Show total costs after each 50th iteration
> tsne_output$itercosts
 [1] 53.8745981 53.1551737 53.6059482 53.6002894 53.2182397  0.8566485
 [7]  0.6141291  0.5568897  0.5034298  0.4971228  0.4963416  0.4958101
[13]  0.4943287  0.4941738  0.4922756  0.4894444  0.4906178  0.4893572
[19]  0.4858486  0.4874892
> 
> # Plot the evolution of the KL divergence at each 50th iteration
> plot(tsne_output$itercosts, type = "l")

```


![ch2plot3](ch2plot2.png)

### 2

```r

# Inspect the output object's structure
str(tsne_output)

# Show the K-L divergence of each record after the final iteration
tsne_output$costs

# Plot the K-L divergence of each record after the final iteration
plot(tsne_output$costs, type = "l")

```

Output:

![ch2plot3](ch2plot3.png)
