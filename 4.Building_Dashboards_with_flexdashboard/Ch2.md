# Chapter 2 - Data Visualization for Dashboards

## Static Graph

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
	library(lubridate)
	library(ggplot2)
	library(tidyverse)

	trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
	```


	Overview
	===================================== 

	Column {data-width=650}
	-----------------------------------------------------------------------

	### Origins

	```{r}

	```

	Column {data-width=350}
	-----------------------------------------------------------------------


	### Trips by Start Time


	### Trip Durations

	```{r static_plot}

	trips_df %>%
	  mutate(`Trip Duration (min)` = duration_sec / 60) %>%
	  filter(`Trip Duration (min)` <= 60) %>%
	  ggplot(aes(x = `Trip Duration (min)`)) +
	  theme_bw() +
	  geom_histogram(binwidth = 1) +
	  ylab('# Trips')

	```

***

## Graph Sizing

Resizing the static plot.


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
	library(lubridate)
	library(ggplot2)
	library(tidyverse)

	trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
	```


	Overview
	===================================== 

	Column {data-width=650}
	-----------------------------------------------------------------------

	### Origins

	```{r}

	```

	Column {data-width=350}
	-----------------------------------------------------------------------


	### Trips by Start Time


	### Trip Durations


	```{r fig.width = 10, fig.height = 6, static_plot}

	trips_df %>%
	  mutate(`Trip Duration (min)` = duration_sec / 60) %>%
	  filter(`Trip Duration (min)` <= 60) %>%
	  ggplot(aes(x = `Trip Duration (min)`)) +
	  theme_bw() +
	  geom_histogram(binwidth = 1) +
	  ylab('# Trips')

	```
***

## Make a plot web-friendly

Using `plotly`

use ggplot graph and use `ggplotly(graph)`

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
	library(lubridate)
	library(ggplot2)
	library(tidyverse)
	library(plotly)

	trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
	```


	Overview
	===================================== 

	Column {data-width=650}
	-----------------------------------------------------------------------

	### Origins

	```{r}

	```

	Column {data-width=350}
	-----------------------------------------------------------------------


	### Trips by Start Time


	### Trip Durations

	```{r}

	duration_gg <- trips_df %>%
	  mutate(`Trip Duration (min)` = duration_sec / 60) %>%
	  filter(`Trip Duration (min)` <= 60) %>%
	  ggplot(aes(x = `Trip Duration (min)`)) +
	  theme_bw() +
	  geom_histogram(binwidth = 1) +
	  ylab('# Trips')

	ggplotly(duration_gg)

	```



***

## Explore web-friendly features

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
	library(lubridate)
	library(ggplot2)
	library(tidyverse)
	library(plotly)

	trips_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/sanfran_bikeshare_joined_oneday.csv')
	```

	Column 
	-----------------------------------------------------------------------

	### Station Usage

	```{r}

	station_df <- trips_df %>%
	  select(start_station_name, end_station_name) %>%
	  rename(Start = start_station_name, End = end_station_name) %>%
	  gather(key = Usage, value = Station)

	station_gg <- ggplot(station_df,
		             aes(x = Station, fill = Usage)) +
		             geom_bar(position = 'stack') +
		             theme_bw() +
		             ylab('Trips') +
		             xlab('') +
		             theme(axis.text.x = element_text(angle = 45, hjust = 1))
		        
	ggplotly(station_gg)

	```

***

## Add a leaflet map

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
	library(leaflet)

	stations_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/stations_data.csv')
	```


	Column {data-width=650}
	-----------------------------------------------------------------------

	### Stations Map

	```{r}
	leaflet() %>%
	addTiles() %>%
	addMarkers(lng = stations_df$longitude, lat = stations_df$latitude)
	```

***

## Add a leaflet map directly from dataframe

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
	library(leaflet)

	stations_df <- read_csv('http://s3.amazonaws.com/assets.datacamp.com/production/course_6355/datasets/stations_data.csv')
	```



	Column {data-width=650}
	-----------------------------------------------------------------------

	### Stations Map

	```{r}
	leaflet(stations_df) %>%
	  addTiles() %>%
	  addMarkers()
	```


***

*End of Chapter 2*

















