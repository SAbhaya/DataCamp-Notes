# Chapter 1 - Quick introduction to the workflow
## Topic prevalence

```bash

> document_topics
            1          2
d_1 0.2307692 0.76923077
d_2 0.1666667 0.83333333
d_3 0.8750000 0.12500000
d_4 0.9230769 0.07692308
d_5 0.3333333 0.66666667
> 

```


Topic 2 has a higher proportion than topic 1

***

## Probabilities of words belonging to topics


```r

# Display the column names
colnames(word_topics)

# Display the probability
word_topics[1, "street"]

```

Output:

```bash
> # Display the column names
> colnames(word_topics)
 [1] "agreed"     "bad"        "bank"       "due"        "fines"     
 [6] "loans"      "pay"        "the"        "to"         "are"       
[11] "face"       "if"         "late"       "off"        "will"      
[16] "you"        "your"       "a"          "downtown"   "in"        
[21] "new"        "opened"     "restaurant" "is"         "just"      
[26] "on"         "street"     "that"       "there"      "warwick"   
[31] "for"        "how"        "need"       "want"
> 
> # Display the probability
> word_topics[1, "street"]
[1] 0.03741497
> 

```
***

## Word frequencies

```r

# Specify the input column
word_freq <- chapters %>% 
  unnest_tokens(output=word, 
                input=text, 
                token="words", 
                format="text") %>% 
  # Obtain word frequencies
  count(chapter, word) 

# Test equality
word_freq %>% filter(word == "after")

```

Output:

```bash

> # Specify the input column
> word_freq <- chapters %>% 
    unnest_tokens(output=word, 
                  input=text, 
                  token="words", 
                  format="text") %>% 
    # Obtain word frequencies
    count(chapter, word)
> 
> # Test equality
> word_freq %>% filter(word == "after")
# A tibble: 2 x 3
  chapter word      n
    <int> <chr> <int>
1       1 after    11
2       2 after     4
> 

```
## Our first LDA model


LDA - Latent Dirichlet Allocation

```r

dtm <- corpus %>% 
    # Specify the input column
	unnest_tokens(input=text, output=word, drop=TRUE) %>% 
	count(id, word) %>% 
    # Specify the token
	cast_dtm(document=id, term=word, value=n)

mod = LDA(x=dtm, k=2, method="Gibbs", control=list(alpha=1, delta=0.1, seed=10005))

posterior(mod)$topics

```

Output:

```bash
> dtm <- corpus %>% 
      # Specify the input column
  	unnest_tokens(input=text, output=word, drop=TRUE) %>% 
  	count(id, word) %>% 
      # Specify the token
  	cast_dtm(document=id, term=word, value=n)
> 
> mod = LDA(x=dtm, k=2, method="Gibbs", control=list(alpha=1, delta=0.1, seed=10005))
> 
> posterior(mod)$topics
            1         2
d_1 0.2307692 0.7692308
d_2 0.1666667 0.8333333
d_3 0.8750000 0.1250000
d_4 0.8461538 0.1538462
d_5 0.2777778 0.7222222
> 


```

***

