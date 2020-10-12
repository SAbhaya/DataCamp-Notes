# Chapter 2 - Wordclouds, stopwords, and control arguments

## Probabilities of words in topics

```r
# Display column names
colnames(dtm)

# Fit an LDA model for 2 topics using Gibbs sampling
mod <- LDA(x=dtm, k=2, method="Gibbs", 
           control=list(alpha=1, seed=10005, thin=1))

# Convert matrix beta into tidy format and filter on topic number and term
tidy(mod, matrix="beta") %>%
  filter(topic==2, term=="opened")

```

Ouput:

```bash

> # Display column names
> colnames(dtm)
[1] "bank"       "fines"      "loans"      "pay"        "new"       
[6] "opened"     "restaurant"
> 
> # Fit an LDA model for 2 topics using Gibbs sampling
> mod <- LDA(x=dtm, k=2, method="Gibbs", 
             control=list(alpha=1, seed=10005, thin=1))
> 
> # Convert matrix beta into tidy format and filter on topic number and term
> tidy(mod, matrix="beta") %>%
    filter(topic==2, term=="opened")
# A tibble: 1 x 3
  topic term     beta
  <int> <chr>   <dbl>
1     2 opened 0.0103
> 

```
***


