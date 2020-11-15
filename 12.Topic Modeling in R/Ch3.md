# Chapter 3 - Named entity recognition as unsupervised classification

## Same k, different alpha

```r

# Fit a topic model using LDA with Gibbs sampling
mod = LDA(x=dtm, k=2, method="Gibbs", 
          control=list(iter=500, thin=1,
                       seed = 12345,
                       alpha=NULL))

# Display topic prevalance in documents as a table
tidy(mod, "gamma") %>% spread(topic, gamma)

```

Output:

```bash
> # Fit a topic model using LDA with Gibbs sampling
> mod = LDA(x=dtm, k=2, method="Gibbs", 
            control=list(iter=500, thin=1,
                         seed = 12345,
                         alpha=NULL))
> 
> # Display topic prevalance in documents as a table
> tidy(mod, "gamma") %>% spread(topic, gamma)
# A tibble: 5 x 3
  document   `1`   `2`
  <chr>    <dbl> <dbl>
1 d_1      0.492 0.508
2 d_2      0.5   0.5  
3 d_3      0.5   0.5  
4 d_4      0.525 0.475
5 d_5      0.485 0.515
> 


```

***

## Probabilities of words in topics

### 1

```r

# Fit the model for delta = 0.5
mod <- LDA(x=dtm, k=2, method="Gibbs",
         control=list(iter=500, seed=12345, alpha=1, delta=0.1))

# Define which words we want to examine
my_terms = c("loans", "bank", "opened", "pay", "restaurant", "you")

# Make a tidy table
t <- tidy(mod, "beta") %>% filter(term %in% my_terms)

# Make a stacked column chart
ggplot(t, aes(x=term, y=beta)) + geom_col(aes(fill=factor(topic))) +
  theme(axis.text.x=element_text(angle=90))

```

Output:

![ch3plot1](ch3plot1.png)


### 2

```r

# Fit the model for delta = 0.5
mod <- LDA(x=dtm, k=2, method="Gibbs",
         control=list(iter=500, seed=12345, alpha=1, delta=0.5))

# Define which words we want to examine
my_terms = c("loans", "bank", "opened", "pay", "restaurant", "you")

# Make a tidy table
t <- tidy(mod, "beta") %>% filter(term %in% my_terms)

# Make a stacked column chart
ggplot(t, aes(x=term, y=beta)) + geom_col(aes(fill=factor(topic))) +
  theme(axis.text.x=element_text(angle=90))
 
```

Output:

![ch3plot2](ch3plot2.png)

***

## Regex pattern for an entity and word context

### 1

```r

# Regex pattern for an entity and word context
p1 <- "( [a-z]+){2}( (St[.] )?[A-Z][a-z]+)+( [a-z]+){2}"

# Obtain the regex match object from gregexpr
m <- gregexpr(p1, text)

# Get the matches and flatten the list
v <- unlist(regmatches(text, m))

# Find the number of elements in the vector
length(v)

```

Output:

```bash

# Regex pattern for an entity and word context
p1 <- "( [a-z]+){2}( (St[.] )?[A-Z][a-z]+)+( [a-z]+){2}"
# Obtain the regex match object from gregexpr
m <- gregexpr(p1, text)
# Get the matches and flatten the list
v <- unlist(regmatches(text, m))
# Find the number of elements in the vector
length(v)
[1] 1502

```
### 2

```r
# Regex pattern for an entity and word context
p2 <- "( [a-z]+){2}( (St[.] )?[A-Z][a-z]+( (of|the) [A-Z][a-z]+)?)+( [a-z]+){2}"

# Obtain the regex match object from gregexpr
m <- gregexpr(p2, text)

# Get the matches and flatten the list
v <- unlist(regmatches(text, m))

# Find the number of elements in the vector
length(v)

```

Output:
```bash

# Regex pattern for an entity and word context
p2 <- "( [a-z]+){2}( (St[.] )?[A-Z][a-z]+( (of|the) [A-Z][a-z]+)?)+( [a-z]+){2}"
# Obtain the regex match object from gregexpr
m <- gregexpr(p2, text)
# Get the matches and flatten the list
v <- unlist(regmatches(text, m))
# Find the number of elements in the vector
length(v)
[1] 1530
>

```

***

## Making a corpus

### 1

```r

# Print out contents of the `entity_pattern`
entity_pattern

# Remove the named entity from text
v2 <- gsub(entity_pattern, "", v)

# Display the head of v2
head(v2)

```

Output:

```bash

head(v2)
[1] " into the shore of"       " settlers were of the"   
[3] " cities of in the"        " to the to plant"        
[5] " attention of was turned" " of the and the"

```

### 2

```r

# Remove the named entity
v2 <- gsub(entity_pattern, "", v)

# Pattern for inserting suffixes
p <- "\\1_L1 \\2_L2 \\3_R1 \\4_R2"

# Add suffixes to words
context <- gsub("([a-z]+) ([a-z]+) ([a-z]+) ([a-z]+)", p, v2)

```

### 3

```r

# Remove the named entity
v2 <- gsub(entity_pattern, "", v)

# Pattern for inserting suffixes
p <- "\\1_L1 \\2_L2 \\3_R1 \\4_R2"

# Add suffixes to words
context <- gsub("([a-z]+) ([a-z]+) ([a-z]+) ([a-z]+)", p, v2)

# Extract named entity and use it as document ID
re_match <- gregexpr(entity_pattern, v)
doc_id <- unlist(regmatches(v, re_match))

# Make a data frame with columns doc_id and text
corpus <- data.frame(doc_id = doc_id, text = context, stringsAsFactors = F)

```

***

## From dtm to topic model

```r

# Summarize the text to produce a document for each doc_id
corpus2 <- corpus %>% group_by(doc_id) %>% 
	summarise(doc = paste(text, collapse=" "))

# Make a document-term matrix
dtm <- corpus2 %>% unnest_tokens(input=doc, output=word) %>% 
	count(doc_id, word) %>% 
	cast_dtm(document=doc_id, term=word, value=n)

# Fit an LDA model for 3 topics
mod <- LDA(x=dtm, k=3, method="Gibbs", 
          control=list(alpha=1, seed=12345, iter=1000, thin=1))

# Create a table with probabilities of topics in documents
topics <- tidy(mod, matrix="gamma") %>% 
	spread(topic, gamma)
```

***

## Train a topic model

```r
# Set random seed for reproducability
set.seed(12345)

# Take a sample of 20 random integers, without replacement
r <- sample.int(n=nrow(corpus2), size=20, replace=FALSE)

# Generate a document-term matrix
train_dtm <- corpus2[-r, ] %>% unnest_tokens(input=doc, output=word) %>% 
  count(doc_id, word) %>% 
  cast_dtm(document=doc_id, term=word, value=n)

# Fit an LDA topic model for k=3
train_mod <- LDA(x=train_dtm, k=3, method="Gibbs",
                control=list(alpha=1, seed=10001,
                             iter=1000, thin=1)
			     
```
***

## Align corpus

```r
# Get the test row indices
set.seed(12345)
r <- sample.int(n=nrow(corpus2), size=20, replace=FALSE)

# Extract the vocabulary of the training model
model_vocab <- tidy(train_mod, matrix="beta") %>% 
  select(term) %>% distinct()

# Create a table of counts with aligned vocabularies
test_table <- corpus2[r, ] %>% unnest_tokens(input=doc, output=word) %>% 
  count(doc_id, word) %>%
  right_join(model_vocab, by=c("word"="term"))

# Prepare a document-term matrix
test_dtm <- test_table %>% 
  arrange(desc(doc_id)) %>% 
  mutate(doc_id = ifelse(is.na(doc_id), first(doc_id), doc_id),
      n = ifelse(is.na(n), 0, n)) %>% 
  cast_dtm(document=doc_id, term=word, value=n)
  
 ```
 
 ***
 
 ## Classify test data
 
 ```r
 
 # Obtain posterior probabilities for test documents
results <- posterior(object=train_mod, newdata=test_dtm)

# Display the matrix with topic probabilities
results$topics

```

Output:

```bash

# Obtain posterior probabilities for test documents
results <- posterior(object=train_mod, newdata=test_dtm)
# Display the matrix with topic probabilities
results$topics
                           1         2          3
 Visigothic        0.1428571 0.2857143 0.57142857
 Theodorics        0.3000000 0.2000000 0.50000000
 Stephen Lecapenus 0.3333333 0.3333333 0.33333333
 St. Louis         0.2000000 0.6000000 0.20000000
 Po                0.7142857 0.1428571 0.14285714
 Patriarch Sergius 0.2857143 0.4285714 0.28571429
 Parthian          0.2857143 0.1428571 0.57142857
 Paris             0.4000000 0.4000000 0.20000000
 John Cantacuzenus 0.2500000 0.5000000 0.25000000
 Hunnish           0.1666667 0.1666667 0.66666667
 Hun               0.5714286 0.2857143 0.14285714
 Germans           0.7142857 0.2380952 0.04761905
 Gaul              0.1428571 0.7142857 0.14285714
 Gainas            0.2592593 0.6296296 0.11111111
 Empress           0.5714286 0.2857143 0.14285714
 Central Italy     0.4285714 0.4285714 0.14285714
 Cappadocian       0.3333333 0.1666667 0.50000000
 Caesar            0.4000000 0.4000000 0.20000000
 Amalphi           0.5714286 0.1428571 0.28571429
 Abu Obeida        0.2000000 0.2000000 0.60000000
>


```
 
***

*End of Chapter 3*
