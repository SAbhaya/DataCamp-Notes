# Chapter 1 - Dashboard Layouts

## Generating a dashboard

    ---
    title: "Bikeshare"
    output: 
    flexdashboard::flex_dashboard
    ---

    ```{r setup, include=FALSE}
    library(flexdashboard)
    ```

    Column
    -----------------------------------------------------------------------

    ### Chart A

    ```{r}

    ```

    Column
    -----------------------------------------------------------------------

    ### Chart B

    ```{r}

     ```

    ### Chart C

    ```{r}

    ```

***


## Adding charts

	---
	title: "Bikeshare"
	output: 
	  flexdashboard::flex_dashboard
	---


	```{r setup, include=FALSE}
	library(flexdashboard)
	library(readr)
	library(ggplot2)
	library(dplyr)
	library(lubridate)
	```

	```{r load_data, include = FALSE}
	trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_5875/datasets/sanfran_bikeshare_joined_oneday.csv')
	```



	Column
	-----------------------------------------------------------------------

	### Chart A

	```{r}

	```

	Column
	-----------------------------------------------------------------------

	### Chart B

	```{r plot1}

	trips_df %>%
	  mutate(Hour = hour(start_date)) %>%
	  group_by(Hour) %>%
	  summarize(`Trips Started` = n()) %>%
	  ggplot(aes(x = Hour, y = `Trips Started`)) +
	  theme_bw() +
	  geom_bar(stat = 'identity')

	```
	### Chart C

	```{r}

	```


***

## Layout Basics

## Column widths

Total column width = 1000

Adjust the column widths so that the first column is three times as wide as the second.


	---
	title: "Bikeshare"
	output: 
	  flexdashboard::flex_dashboard:
	    orientation: columns
	---

	```{r setup, include=FALSE}
	library(flexdashboard)
	```

	Column{data-width=750}
	-----------------------------------------------------------------------

	### Chart A

	```{r}

	```

	Column{data-width=250}
	-----------------------------------------------------------------------

	### Chart B

	```{r}

	```

	### Chart C

	```{r}

	```

***

## Row orientation

---
title: "Bikeshare"
output: 
  flexdashboard::flex_dashboard:
    orientation: rows
---

```{r setup, include=FALSE}
library(flexdashboard)
```

Column
-----------------------------------------------------------------------

### Chart A

```{r}

```

Column
-----------------------------------------------------------------------

### Chart B

```{r}

```

### Chart C

```{r}

```

***

## Tabsets

---
title: "Bikeshare"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
---

```{r setup, include=FALSE}
library(flexdashboard)
```

Column{.tabset}
-----------------------------------------------------------------------

### Chart A

```{r}

```

### Chart B

```{r}

```
Column
-----------------------------------------------------------------------

### Chart C

```{r}

```

***

## Pages

---
title: "Bikeshare"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
---

```{r setup, include=FALSE}
library(flexdashboard)
```

Overview
====================================

Column
-----------------------------------------------------------------------

### Chart A

```{r}

```

Column
-----------------------------------------------------------------------

### Chart B

```{r}

```

Details
==============================================

### Chart C

```{r}

```



***

## Pages

---
title: "Bikeshare"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
---

```{r setup, include=FALSE}
library(flexdashboard)
```

Overview
=====================================

Column
-----------------------------------------------------------------------

### Chart A

```{r}

```

Column
-----------------------------------------------------------------------

### Chart B

```{r}

```

### Chart C

```{r}

```

Details{data-navmenu=More}
=====================================

### Chart AA

```{r}

```

Data{data-navmenu=More}
=====================================

### Chart BB

```{r}

```

***

*End of Chapter 1*










