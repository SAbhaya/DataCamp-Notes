# Chapter 4 - How many topics is enough?

## Preparing the dtm

```r
# Split the Abstract column into tokens
dtm <- df %>% unnest_tokens(input=Abstract, output=word) %>% 
   # Remove stopwords
   anti_join(stop_words) %>% 
   # Count the number of occurrences
   count(AwardNumber, word) %>% 
   # Create a document term matrix
   cast_dtm(document=AwardNumber, term=word, value=n)
```

***

## Filtering by word frequency

remove the words whose corpus-wide frequency is less than 10.

```r
dtm <- df %>% unnest_tokens(input=Abstract, output=word) %>% 
   anti_join(stop_words) %>% 
   # Count occurences within documents
   count(AwardNumber, word) %>%
   # Group the data
   group_by(word) %>% 
   # Filter for corpus wide frequency
   filter(sum(n) >= 10) %>% 
   # Ungroup the data andreate a document term matrix
   ungroup() %>% 
   cast_dtm(document=AwardNumber, term=word, value=n)

```

***
## Fitting the model

```r

# Create a LDA model
mod <- LDA(x=dtm, method="Gibbs", k=3, 
          control=list(alpha=0.5, seed=1234, iter=500, thin=1))

# Retrieve log-likelihood
logLik(mod)

# Find perplexity
perplexity(object=mod, newdata=dtm)

```
***

## Using perplexity to find the best k

### 1

```r
# Display names of elements in the list
names(models[[1]])

# Retrieve the values of k and perplexity, and plot perplexity vs k
x <- sapply(models, '[[', 'k')
y <- sapply(models, '[[', 'perplexity')
plot(x, y, xlab="number of clusters, k", ylab="perplexity score", type="o")

```
Output:

![ch4plot1](ch4plot1.png)

### 2

```r

# Record the new perplexity scores
new_perplexity_score <- numeric(length(models))

# Run each model for 100 iterations
for (i in seq_along(models)) {
  mod2 <- LDA(x=dtm, model=models[[i]]$model,
             control=list(iter=100, seed=12345, thin=1))
  new_perplexity_score[i] <- perplexity(object=mod2, newdata=dtm)
}

```

### 3

```r

# Record the new perplexity scores
new_perplexity_score <- numeric(length(models))

# Run each model for 100 iterations
for (i in seq_along(models)) {
  mod2 <- LDA(x=dtm, model=models[[i]]$model,
             control=list(iter=100, seed=12345, thin=1))
  new_perplexity_score[i] <- perplexity(object=mod2, newdata=dtm)
}

# Specify the possible values of k and build the plot
k <- 2:10
plot(x=k, y=new_perplexity_score, xlab="number of clusters, k", 
     ylab="perplexity score", type="o")


```

Output:

![ch4plot2](ch4plot2.png)

***