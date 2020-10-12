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

## Effect of argument alpha

### 2

```r
# Fit LDA topic model using Gibbs sampling for 2 topics
mod1 <- LDA(x=dtm, k=2, method="Gibbs",
           control=list(alpha=1, seed=10005, thin=1))

# Display the probabilities of topics in documents side by side
tidy(mod1, matrix="gamma") %>% spread(topic, gamma)

```

Output:

```bash

> # Fit LDA topic model using Gibbs sampling for 2 topics
> mod1 <- LDA(x=dtm, k=2, method="Gibbs",
             control=list(alpha=1, seed=10005, thin=1))
> 
> # Display the probabilities of topics in documents side by side
> tidy(mod1, matrix="gamma") %>% spread(topic, gamma)
# A tibble: 5 x 3
  document   `1`    `2`
  <chr>    <dbl>  <dbl>
1 d_1      0.154 0.846 
2 d_2      0.278 0.722 
3 d_3      0.875 0.125 
4 d_4      0.923 0.0769
5 d_5      0.5   0.5
> 

```

### 3

Change alpha to 25.

```r

# Fit LDA topic model using Gibbs sampling for 2 topics
mod2 <- LDA(x=dtm, k=2, method="Gibbs",
           control=list(alpha=25, seed=10005, thin=1))

# Display the probabilities of topics in documents side by side
tidy(mod2, matrix = "gamma") %>% spread(topic, gamma)

```

Output:

```bash
> # Fit LDA topic model using Gibbs sampling for 2 topics
> mod2 <- LDA(x=dtm, k=2, method="Gibbs",
             control=list(alpha=25, seed=10005, thin=1))
> 
> # Display the probabilities of topics in documents side by side
> tidy(mod2, matrix = "gamma") %>% spread(topic, gamma)
# A tibble: 5 x 3
  document   `1`   `2`
  <chr>    <dbl> <dbl>
1 d_1      0.475 0.525
2 d_2      0.530 0.470
3 d_3      0.482 0.518
4 d_4      0.508 0.492
5 d_5      0.5   0.5
> 

```

***


## Making a dtm - refresher


```r

# Create the document-term matrix
dtm <- corpus %>%
  unnest_tokens(output=word, input=text) %>%
  count(id, word) %>%
  cast_dtm(document=id, term=word, value=n)

# Display dtm as a matrix
as.matrix(dtm)

```

Output:

```bash
> # Create the document-term matrix
> dtm <- corpus %>%
    unnest_tokens(output=word, input=text) %>%
    count(id, word) %>%
    cast_dtm(document=id, term=word, value=n)
> 
> # Display dtm as a matrix
> as.matrix(dtm)
     Terms
Docs  agreed bad bank due fines loans pay the to are face if late off will you
  d_1      1   1    1   1     1     1   1   2  2   0    0  0    0   0    0   0
  d_2      0   0    1   0     1     1   1   1  2   1    1  1    1   1    1   2
  d_3      0   0    0   0     0     0   0   0  0   0    0  0    0   0    0   0
  d_4      0   0    0   0     0     0   0   0  0   0    0  0    0   0    0   0
  d_5      0   0    0   0     0     1   1   2  0   0    0  0    0   1    2   3
     Terms
Docs  your a downtown in new opened restaurant is just on street that there
  d_1    0 0        0  0   0      0          0  0    0  0      0    0     0
  d_2    1 0        0  0   0      0          0  0    0  0      0    0     0
  d_3    0 1        1  1   1      1          1  0    0  0      0    0     0
  d_4    0 1        0  0   1      1          1  1    1  1      1    1     1
  d_5    0 0        0  0   0      1          1  0    0  0      0    0     0
     Terms
Docs  warwick for how need want
  d_1       0   0   0    0    0
  d_2       0   0   0    0    0
  d_3       0   0   0    0    0
  d_4       1   0   0    0    0
  d_5       0   1   1    1    1
> 

```
## Removing stopwords

```r
# Create the document-term matrix with stop words removed
dtm <- corpus %>%
  unnest_tokens(output=word, input=text) %>%
  anti_join(stop_words) %>% 
  count(id, word) %>%
  cast_dtm(document=id, term=word, value=n)

# Display the matrix
as.matrix(dtm)

```
Output:

```bash

> # Create the document-term matrix with stop words removed
> dtm <- corpus %>%
    unnest_tokens(output=word, input=text) %>%
    anti_join(stop_words) %>% 
    count(id, word) %>%
    cast_dtm(document=id, term=word, value=n)
Joining, by = "word"
> 
> # Display the matrix
> as.matrix(dtm)
     Terms
Docs  agreed bad bank due fines loans pay late downtown restaurant street
  d_1      1   1    1   1     1     1   1    0        0          0      0
  d_2      0   0    1   0     1     1   1    1        0          0      0
  d_3      0   0    0   0     0     0   0    0        1          1      0
  d_4      0   0    0   0     0     0   0    0        0          1      1
  d_5      0   0    0   0     0     1   1    0        0          1      0
     Terms
Docs  warwick
  d_1       0
  d_2       0
  d_3       0
  d_4       1
  d_5       0
> 

```
