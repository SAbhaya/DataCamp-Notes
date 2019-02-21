# Chapter 1

## Simple Text

```r
# Load the shiny package
library(shiny)

# Define UI for the application
ui <- fluidPage(
  # Add the text "Shiny is fun"
  "Shiny is fun"
)

# Define the server logic
server <- function(input, output) {}

# Run the application
shinyApp(ui = ui, server = server)
```

***

## Formatted text

```r

# Load the shiny package
library(shiny)

# Define UI for the application
ui <- fluidPage(
  # "DataCamp" as a primary header
  h1("DataCamp"),
  # "Shiny use cases course" as a secondary header
  h2("Shiny use cases course"),
  # "Shiny" in italics
  em("Shiny"),
  # "is fun" as bold text
  strong("is fun")
)

# Define the server logic
server <- function(input, output) {}

# Run the application
shinyApp(ui = ui, server = server)

```

***

## Adding structure to your app

```r

# Load the shiny package
library(shiny)

# Define UI for the application
ui <- fluidPage(
  # Add a sidebar layout to the application
  sidebarLayout(
    # Add a sidebar panel around the text and inputs
    sidebarPanel(
      h4("Plot parameters"),
      textInput("title", "Plot title", "Car speed vs distance to stop"),
      numericInput("num", "Number of cars to show", 30, 1, nrow(cars)),
      sliderInput("size", "Point size", 1, 5, 2, 0.5)
    ),
    # Add a main panel around the plot and table
    mainPanel(
      plotOutput("plot"),
      tableOutput("table")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    plot(cars[1:input$num, ], main = input$title, cex = input$size)
  })
  output$table <- renderTable({
    cars[1:input$num, ]
  })
}

# Run the application
shinyApp(ui = ui, server = server)

```

***

## Adding inputs

```r

library(shiny)

# Define UI for the application
ui <- fluidPage(
  # Create a numeric input with ID "age" and label of
  # "How old are you?"
  numericInput("age", "How old are you?", value = 20),
  
  # Create a text input with ID "name" and label of 
  # "What is your name?"
  textInput(inputId = "name", label = "What is your name?")
)

# Define the server logic
server <- function(input, output) {}

# Run the application
shinyApp(ui = ui, server = server)

```
***

## Adding placeholders for outputs

```r
library(shiny)

# Define UI for the application
ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      # Create a text input with an ID of "name"
      textInput("name", "What is your name?", "Dean"),
      numericInput("num", "Number of flowers to show data for",
                   10, 1, nrow(iris))
    ),
    mainPanel(
      # Add a placeholder for a text output with ID "greeting"
      textOutput(outputId = "greeting"),
      # Add a placeholder for a plot with ID "cars_plot"
      plotOutput("cars_plot"),
      # Add a placeholder for a table with ID "iris_table"
      tableOutput("iris_table")
    )
  )
)

# Define the server logic
server <- function(input, output) {}

# Run the application
shinyApp(ui = ui, server = server)

```
***

## Constructing output objects

```r

# Load the shiny package
library(shiny)

# Define UI for the application
ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("name", "What is your name?", "Dean"),
      numericInput("num", "Number of flowers to show data for",
                   10, 1, nrow(iris))
    ),
    mainPanel(
      textOutput("greeting"),
      plotOutput("cars_plot"),
      tableOutput("iris_table")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  # Create a plot of the "cars" dataset 
  output$cars_plot <- renderPlot({
    plot(cars)
  })
  
  # Render a text greeting as "Hello <name>"
  output$greeting <- renderText({
    paste("Hello", input$name)
  })
  
  # Show a table of the first n rows of the "iris" data
  output$iris_table <- renderTable({
    data <- iris[1:input$num, ]
    data
  })
}

# Run the application
shinyApp(ui = ui, server = server)

```

***

## Reactive contexts


Reactive values are special constructs in Shiny; they are not seen anywhere else in R programming. 
As such, they cannot be used in just any R code, reactive values can only be accessed within a reactive context.

```r

ui <- fluidPage(
  numericInput("num1", "Number 1", 5),
  numericInput("num2", "Number 2", 10),
  textOutput("result")
)

server <- function(input, output) {
  # Calculate the sum of the inputs
  my_sum <- reactive({
    input$num1 + input$num2
  })

  # Calculate the average of the inputs
  my_average <- reactive({
    my_sum() / 2
  })
  
  output$result <- renderText({
    paste(
      # Print the calculated sum
      "The sum is", my_sum(),
      # Print the calculated average
      "and the average is", my_average()
    )
  })
}

shinyApp(ui, server)

```

***

*End of Chapter 1*

