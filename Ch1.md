# Chapter 1 - Building Static Dashboards

## Create empty Header, Sidebar, and Body

```r

library(shinydashboard)

# Create an empty header
header <- dashboardHeader()

# Create an empty sidebar
sidebar <- dashboardSidebar()

# Create an empty body
body <- dashboardBody()

```

***

## Create an empty Shiny Dashboard

```r

header <- dashboardHeader()
sidebar <- dashboardSidebar()
body <- dashboardBody()

# Create the UI using the header, sidebar, and body
ui <- dashboardPage(header, sidebar, body)

server <- function(input, output) {}

shinyApp(ui, server)

```

***

## Create message menus

You can add menus to your header using the `dropdownMenu()` function. 


```r
header <- dashboardHeader(
  dropdownMenu(type = "messages")
)

Code

```r

header <- dashboardHeader(
  dropdownMenu(
    type = "messages",
    messageItem(
        from = "Lucy",
        message = "You can view the International Space Station!",
        href = "https://spotthestation.nasa.gov/sightings/"
        ),
    # Add a second messageItem() 
    messageItem(
        from = "Lucy",
        message = "Learn more about the International Space Station",
        href = "https://spotthestation.nasa.gov/faq.cfm"
      )
    )
)

ui <- dashboardPage(header = header,
                    sidebar = dashboardSidebar(),
                    body = dashboardBody()
                    )
shinyApp(ui, server)

```



