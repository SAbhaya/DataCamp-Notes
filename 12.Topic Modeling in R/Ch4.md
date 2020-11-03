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
