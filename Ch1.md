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

***

## Create notification menus


```r

header <- dashboardHeader(
  # Create a notification drop down menu
  dropdownMenu(
    type = "notifications",
    notificationItem(
      text="The International Space Station is overhead!"
    
    )
  )
)

# Use the new header
ui <- dashboardPage(header = header,
                    sidebar = dashboardSidebar(),
                    body = dashboardBody()
                    )
shinyApp(ui, server)

```

***

## Create task menus

Here we can show task %

```r

header <- dashboardHeader(
  # Create a tasks drop down menu
  dropdownMenu(
    type = "tasks",
    taskItem(
        text = "Mission Learn Shiny Dashboard",
        value = 10
      )
    )
)

# Use the new header
ui <- dashboardPage(header = header,
                    sidebar = dashboardSidebar(),
                    body = dashboardBody()
                    )
shinyApp(ui, server)

```

***

## Create Sidebar tabs

```r

sidebar <- dashboardSidebar(
  sidebarMenu(
  # Create two `menuItem()`s, "Dashboard" and "Inputs"
    menuItem(text = "Dashboard", tabName = "dashboard"),
    menuItem(text = "Inputs", tabName = "inputs")
  )
)

# Use the new sidebar
ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = sidebar,
                    body = dashboardBody()
                    )
shinyApp(ui, server)

```

***

## Create Body tabs

```r

body <- dashboardBody(
  tabItems(
  # Add two tab items, one with tabName "dashboard" and one with tabName "inputs"
    tabItem(tabName = "dashboard"),
    tabItem(tabName = "inputs")
  )
)

# Use the new body
ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = sidebar,
                    body = body
                    )
shinyApp(ui, server)

```
***

## Create tab boxes

```r

library("shiny")
body <- dashboardBody(
  # Create a tabBox
  tabItems(
    tabItem(
      tabName = "dashboard",
      tabBox(title = "International Space Station Fun Facts",
        tabPanel("Fun Fact 1"),
        tabPanel("Fun Fact 2")
      
      )
    ),
    tabItem(tabName = "inputs")
  )
)

ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = sidebar,
                    body = body
                    )
shinyApp(ui, server)

```

***

*End of Chapter 1*

