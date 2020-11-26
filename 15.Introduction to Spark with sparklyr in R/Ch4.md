# Chapter 4 - Case Study: Learning to be a Machine: Running Machine Learning Models on Spark
## Machine learning functions

You can see the list of all the machine learning functions using ls()

> ls("package:sparklyr", pattern = "^ml")

```r
a_tibble %>%
  ml_some_model("response", c("a_feature", "another_feature"), some_other_args)

```

What arguments do all the machine learning model functions take?

> A tibble, a string naming the response field, and a character vector naming the explanatory fields.

***

## (Hey you) What's that sound?
