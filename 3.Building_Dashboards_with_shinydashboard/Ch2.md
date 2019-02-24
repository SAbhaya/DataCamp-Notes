# Chapter 2 - Building Dynamic Dashboards

## Review selectInput and sliderInput

```r

sidebar <- dashboardSidebar(
  # Add a slider
  sliderInput(inputId = "height", label = "Height", min = 66,
  max = 264, value = 264)
)

ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = sidebar,
                    body = dashboardBody()
                    )
shinyApp(ui, server) 

```

***

## Reactive expression practice

```r

library(shiny)
sidebar <- dashboardSidebar(
  # Create a select list
  selectInput("name",label = "Name", choices = starwars$name)
)

body <- dashboardBody(
  textOutput("name")
)

ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = sidebar,
                    body = body
                    )

server <- function(input, output) {
  output$name <- renderText({
      input$name
    })
}

shinyApp(ui, server)


```

***

## Read in real-time data

read  data using `reactiveFileReader()` 

`readFunc` set to read.csv.

```r

filepath <- "data.csv"

server <- function(input, output, session) {
  reactive_data <- reactiveFileReader(
    intervalMillis = 1000,
    session = session, 
    filePath = filepath,
    readFunc = read.csv
  )
}

```

Code

```r

library("shiny")

server <- function(input, output, session) {
  reactive_starwars_data <- reactiveFileReader(
         intervalMillis = 1000,
         session = session,
         filePath = starwars_url,
         readFunc = function(filePath) { 
           read.csv(url(filePath))
         }
  )
}

ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = dashboardSidebar(),
                    body = dashboardBody()
                    )
shinyApp(ui, server)

```

***

## View real-time data

```r

library(shiny)

server <- function(input, output, session) {
  reactive_starwars_data <- reactiveFileReader(
        intervalMillis = 1000,
        session = session,
        filePath = starwars_url,
        readFunc = function(filePath) { 
           read.csv(url(filePath))
         }
         )
    
  output$table <- renderTable({
       reactive_starwars_data() 
  })
}

body <- dashboardBody(
  tableOutput("table")
)

ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = dashboardSidebar(),
                    body = body
                    )
shinyApp(ui, server)

```

***

## Optimize this

```r

starwars <- read.csv(starwars_filepath)
server <- function(input, output) { 
   
}

```

***

## Create reactive menu items


```r

server <- function(input, output) {
  output$task_menu <- renderMenu({
      tasks <- apply(task_data,1,function(row){
          taskItem(text = row[["text"]],
                    value = row[["value"]])
      })
      dropdownMenu(type = "tasks", .list = tasks)
  })
}

header <- dashboardHeader(dropdownMenuOutput("task_menu"))

ui <- dashboardPage(header = header,
                    sidebar = dashboardSidebar(),
                    body = dashboardBody()
                    )
shinyApp(ui, server)

```

data frame `task_date` content

```

> task_data
                                           text value
1 find 20 hidden mickeys on the Tower of Terror    60
2       Find a paint brush on Tom Sawyer Island     0
3                                Meet Chewbacca   100
> 

```

***

## Create reactive boxes

```r

library("shiny")
sidebar <- dashboardSidebar(
  actionButton("click", "Update click box")
) 

server <- function(input, output) {
  output$click_box <- renderValueBox({
    valueBox( value = input$click,
      subtitle = "Click Box"
    )
  })
}

body <- dashboardBody(
      valueBoxOutput("click_box")
 )


ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = sidebar,
                    body = body
                    )
shinyApp(ui, server)

```

***

*End of Chapter 2*





