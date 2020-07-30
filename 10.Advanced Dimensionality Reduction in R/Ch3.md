# Chapter 3 - Using t-SNE with Predictive Models
## Exploring credit card fraud dataset


```r
# Look at the data dimensions
dim(creditcard)

# Explore the column names
names(creditcard)

# Explore the structure
str(creditcard)

# Generate a summary
summary(creditcard)

# Plot a histogram of the transaction time
ggplot(creditcard, aes(x = Time)) + 
	geom_histogram()

```
Output:

![ch3plot1](ch3plot1.png)

***

## Generating training and test sets

```r

# Extract positive and negative instances of fraud
creditcard_pos <- creditcard[Class == 1]
creditcard_neg <- creditcard[Class == 0]

# Fix the seed
set.seed(1234)

# Create a new negative balanced dataset by undersampling
creditcard_neg_bal <- creditcard_neg[sample(1:nrow(creditcard_neg), nrow(creditcard_pos))]

# Generate a balanced train set
creditcard_train <- rbind(creditcard_pos, creditcard_neg_bal)


```

***

## 
