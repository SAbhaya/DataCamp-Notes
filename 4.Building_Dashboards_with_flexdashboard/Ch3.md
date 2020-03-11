# Chapter 3 - Dashboard Components

## Create value box

	---
	title: "Bike Shares Daily"
	output: 
  	flexdashboard::flex_dashboard:
    	orientation: columns
    	vertical_layout: fill
	---

	```{r setup, include=FALSE}
	library(flexdashboard)
	library(readr)
	library(tidyverse)
	library(lubridate)

	trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
	```

	Column {data-width=650}
	-----------------------------------------------------------------------

	### Origins


	Column {data-width=350}
	-----------------------------------------------------------------------

	### Median Trip Length

	```{r}

	median_min <- median(trips_df$duration_sec / 60) %>% round(digits = 1)

	valueBox(median_min,
    	caption = "Median Trip Duration (Minutes)",
    	icon = "fa-clock-o")
	```

	### % Short Trips


	### Trips by Start Time


***

## Create gauge

---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(tidyverse)
library(lubridate)

trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
```

Column {data-width=650}
-----------------------------------------------------------------------

### Origins


Column {data-width=350}
-----------------------------------------------------------------------

### Median Trip Length


### % Short Trips

```{r}

num_trips <- nrow(trips_df)
short_trips <- sum(trips_df$duration_sec < 600) # under 10 min
pct_short_trips <- round(100 * short_trips / num_trips, 0)

gauge(value = pct_short_trips,
      min = 0,
      max = 100)
```

### Trips by Start Time

***

## Color indicators

---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(tidyverse)
library(lubridate)

trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
```

Column {data-width=650}
-----------------------------------------------------------------------

### Origins


Column {data-width=350}
-----------------------------------------------------------------------

### Median Trip Length


### % Short Trips

```{r}

num_trips <- nrow(trips_df)
short_trips <- sum(trips_df$duration_sec < 600) # under 10 min
pct_short_trips <- round(100 * short_trips / num_trips, 0)

gauge(value = pct_short_trips,
      min = 0,
      max = 100,
      sectors = gaugeSectors(
                  success = c(67,100),
                  warning = c(33,66),
                  danger = c(0,32)
        ),
     symbol = '%' )

```

### Trips by Start Time




***

## Linking

Use lowercase with '-' for spaces

'#trip-duration' link to page

Trip Duration



---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(tidyverse)
library(lubridate)
library(plotly)

trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
```

Overview
====================================================

Column {data-width=650}
-----------------------------------------------------------------------

### Origins


Column {data-width=350}
-----------------------------------------------------------------------

### Median Trip Length

```{r}

median_min <- median(trips_df$duration_sec / 60) %>% round(digits = 1)

valueBox(median_min, 
         caption = 'Median Trip Duration (Minutes)', 
         icon = 'fa-clock-o',
         href = '#trip-duration')


```

### % Short Trips


### Trips by Start Time


Trip Duration
====================================================

### Trip Durations

```{r}

duration_plot <- trips_df %>%
  mutate(`Trip Duration (min)` = duration_sec / 60) %>%
  ggplot(aes(x = `Trip Duration (min)`)) +
  theme_bw() +
  geom_histogram(binwidth = 1) +
  ylab('# Trips')

duration_plot %>% ggplotly()

```

***

## Static table

---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(tidyverse)
library(lubridate)
library(plotly)
library(knitr)
library(DT)

trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
```

Column {data-width=650}
-----------------------------------------------------------------------

### Station Usage

```{r}

station_trips_df <- trips_df %>%
  select(start_station_name, end_station_name) %>%
  gather(key = Type, value = Station) %>%
  group_by(Station, Type) %>%
  summarize(n_trips = n()) %>% 
  mutate(Type = ifelse(Type == 'start_station_name', 'Trip Starts', 'Trip Ends')) %>%
  spread(key = Type, value = n_trips) %>%
  replace_na(list(`Trip Starts` = 0, `Trip Ends` = 0)) %>%
  mutate(Gap = `Trip Ends` - `Trip Starts`)
  
kable(station_trips_df)

```


Column {data-width=350}
-----------------------------------------------------------------------

### Median Trip Length


### % Short Trips


### Trips by Start Time


***

## Web-friendly table

Using `DT` library and `datatable()`



---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(tidyverse)
library(lubridate)
library(plotly)
library(knitr)
library(DT)

trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
```

Column {data-width=650}
-----------------------------------------------------------------------

### Station Usage

```{r}

station_trips_df <- trips_df %>%
  select(start_station_name, end_station_name) %>%
  gather(key = Type, value = Station) %>%
  group_by(Station, Type) %>%
  summarize(n_trips = n()) %>% 
  mutate(Type = ifelse(Type == 'start_station_name', 'Trip Starts', 'Trip Ends')) %>%
  spread(key = Type, value = n_trips) %>%
  replace_na(list(`Trip Starts` = 0, `Trip Ends` = 0)) %>%
  mutate(Gap = `Trip Ends` - `Trip Starts`)

datatable(station_trips_df)

```


Column {data-width=350}
-----------------------------------------------------------------------

### Median Trip Length


### % Short Trips


### Trips by Start Time


***

## DT::datatable

---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(tidyverse)
library(lubridate)
library(plotly)
library(knitr)
library(DT)

trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
```

Column {data-width=650}
-----------------------------------------------------------------------

### Station Usage

```{r}

station_trips_df <- trips_df %>%
  select(start_station_name, end_station_name) %>%
  gather(key = Type, value = Station) %>%
  group_by(Station, Type) %>%
  summarize(n_trips = n()) %>% 
  mutate(Type = ifelse(Type == 'start_station_name', 'Trip Starts', 'Trip Ends')) %>%
  spread(key = Type, value = n_trips) %>%
  replace_na(list(`Trip Starts` = 0, `Trip Ends` = 0)) %>%
  mutate(Gap = `Trip Ends` - `Trip Starts`)

datatable(station_trips_df,
extensions = 'Buttons',
options = list(dom = 'Bfrtip', buttons = c('copy', 'csv', 'excel', 'pdf', 'print'))

)

```


Column {data-width=350}
-----------------------------------------------------------------------

### Median Trip Length


### % Short Trips


### Trips by Start Time

***



## Captions

Using > after the R block

```{r}

```

> Caption


---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(leaflet)
library(DT)
library(tidyverse)
library(lubridate)

trips_df <- read_csv('https://assets.datacamp.com/production/repositories/1448/datasets/1f12031000b09ad096880bceb61f6ca2fd95e2eb/sanfran_bikeshare_joined_oneday.csv')
```

Column 
-----------------------------------------------------------------------
### Origins

```{r}

trips_df %>%
  rename(latitude = start_latitude,
         longitude = start_longitude) %>%
  group_by(start_station_id, latitude, longitude) %>%
  count() %>%
  leaflet() %>%
  addTiles() %>%
  addCircles(radius = ~n)

```
> Source: Bay Area Bike Share

***

## Captions with Inline Code



---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(leaflet)
library(DT)
library(tidyverse)
library(lubridate)

trips_df <- read_csv('https://assets.datacamp.com/production/repositories/1448/datasets/1f12031000b09ad096880bceb61f6ca2fd95e2eb/sanfran_bikeshare_joined_oneday.csv')

data_date <- unique(as.Date(trips_df$start_date))
```

Column 
-----------------------------------------------------------------------
### Origins

```{r}

trips_df %>%
  rename(latitude = start_latitude,
         longitude = start_longitude) %>%
  group_by(start_station_id, latitude, longitude) %>%
  count() %>%
  leaflet() %>%
  addTiles() %>%
  addCircles(radius = ~n)

```

> Source: Bay Area Bike Share, Date:  `r data_date`


***

## Storyboards

Change the header and include 

`storyboard: true`

---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
    storyboard: true
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(leaflet)
library(DT)
library(tidyverse)
library(lubridate)
library(plotly)

trips_df <- read_csv('https://assets.datacamp.com/production/repositories/1448/datasets/1f12031000b09ad096880bceb61f6ca2fd95e2eb/sanfran_bikeshare_joined_oneday.csv')
```

### Most bikes are used only a few times, but a few are used a lot

```{r}

trips_per_bike_df <- trips_df %>%
  group_by(bike_number) %>%
  summarize(n_trips = n()) %>%
  arrange(desc(n_trips)) 

bike_plot <- trips_per_bike_df %>%
  ggplot(aes(x = n_trips)) +
  geom_histogram(binwidth = 1) +
  ylab('') +
  xlab('Trips per bike') 

ggplotly(bike_plot)

```

### Where did the most used bike go?

```{r}

trips_df %>%
  filter(bike_number == trips_per_bike_df$bike_number[1]) %>%
  rename(latitude = start_latitude,
         longitude = start_longitude) %>%
  group_by(start_station_id, latitude, longitude) %>%
  count() %>%
  leaflet() %>%
  addTiles() %>%
  addMarkers()

```

***


## Storyboard Commentary

R chunk ending, add a blank line, three asterisks, another blank line, then add required comments, bullet points. etc.

---
title: "Bike Shares Daily"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    vertical_layout: fill
    storyboard: true
---

```{r setup, include=FALSE}
library(flexdashboard)
library(readr)
library(leaflet)
library(DT)
library(tidyverse)
library(lubridate)
library(plotly)

trips_df <- read_csv('https://assets.datacamp.com/production/repositories/1448/datasets/1f12031000b09ad096880bceb61f6ca2fd95e2eb/sanfran_bikeshare_joined_oneday.csv')
```


### Most bikes are used only a few times, but a few are used a lot

```{r}

trips_per_bike_df <- trips_df %>%
  group_by(bike_number) %>%
  summarize(n_trips = n()) %>%
  arrange(desc(n_trips)) 

bike_plot <- trips_per_bike_df %>%
  ggplot(aes(x = n_trips)) +
  geom_histogram(binwidth = 1) +
  ylab('') +
  xlab('Trips per bike') 

ggplotly(bike_plot)

```

### Where did the most used bike go?

```{r}

most_used_bike_df <- trips_df %>%
  filter(bike_number == trips_per_bike_df$bike_number[1])

most_used_bike_df %>%
  rename(latitude = start_latitude,
         longitude = start_longitude) %>%
  group_by(start_station_id, latitude, longitude) %>%
  count() %>%
  leaflet() %>%
  addTiles() %>%
  addMarkers()

```

***

* Bike `r most_used_bike_df$bike_number[1]` made its first trip from `r most_used_bike_df$start_station_name[1]` and ended its day at `r most_used_bike_df$end_station_name[nrow(most_used_bike_df)]`.
* Its longest trip was `r max(most_used_bike_df$duration_sec)/60` minutes long.


***

*End of Chapter 3*




















