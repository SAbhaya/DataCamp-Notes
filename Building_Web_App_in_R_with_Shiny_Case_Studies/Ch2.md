# Chapter 2

## More exploration of the Gapminder data

Determine the population of France in year 1972 using the subset() function, and display that number in the text output (line 14).

```r

# Load the gapminder package
library(gapminder)

# Define UI for the application
ui <- fluidPage(
  "The population of France in 1972 was",
  textOutput("answer")
)

# Define the server function
server <- function(input, output) {
  output$answer <- renderText({
    # Determine the population of France in year 1972
    subset(gapminder, country == "France" & year == 1972)$pop
  })
}

# Run the application
shinyApp(ui = ui, server = server)

```

***

## Add a plot title: text input

```r

# Load the ggplot2 package for plotting
library(ggplot2)

# Define UI for the application
ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      # Add a title text input
      textInput("title","Title", value="GDP vs life exp")
    ),
    mainPanel(
      plotOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    ggplot(gapminder, aes(gdpPercap, lifeExp)) +
      geom_point() +
      scale_x_log10() +
      # Use the input value as the plot's title
      ggtitle(input$title)
  })
}

# Run the application
shinyApp(ui = ui, server = server)

```

***

## Change the point size: numeric input

```r
# Define UI for the application
ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      # Add a size numeric input
      numericInput("size", "Point size", value = 1, min = 1)
    ),
    mainPanel(
      plotOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    ggplot(gapminder, aes(gdpPercap, lifeExp)) +
      # Use the size input as the plot point size
      geom_point(size = input$size) +
      scale_x_log10() +
      ggtitle(input$title)
  })
}

# Run the application
shinyApp(ui = ui, server = server)

```

***
## Fit a smooth curve: checkbox input

```r

# Define UI for the application
ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      numericInput("size", "Point size", 1, 1),
      # Add a checkbox for line of best fit
      checkboxInput("fit","Add line of best fit")
    ),
    mainPanel(
      plotOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    p <- ggplot(gapminder, aes(gdpPercap, lifeExp)) +
      geom_point(size = input$size) +
      scale_x_log10() +
      ggtitle(input$title)
    
    # When the "fit" checkbox is checked, add a line
    # of best fit
    if (input$fit) {
      p <- p + geom_smooth(method = "lm")
    }
    p
  })
}

# Run the application
shinyApp(ui = ui, server = server)

```
***

## Add colours to your plot: radio buttons

```r

# Define UI for the application
ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      numericInput("size", "Point size", 1, 1),
      checkboxInput("fit", "Add line of best fit", FALSE),
      # Add radio buttons for colour
      radioButtons("colour", "Point colour", choices = c("blue", "red", "green", "black"))
    ),
    mainPanel(
      plotOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    p <- ggplot(gapminder, aes(gdpPercap, lifeExp)) +
      # Use the value of the colour input as the point colour
      geom_point(size = input$size, col = input$colour) +
      scale_x_log10() +
      ggtitle(input$title)
    
    if (input$fit) {
      p <- p + geom_smooth(method = "lm")
    }
    p
  })
}

# Run the application
shinyApp(ui = ui, server = server)

```
***

## Add a continent selector: select input

```r

ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      numericInput("size", "Point size", 1, 1),
      checkboxInput("fit", "Add line of best fit", FALSE),
      radioButtons("colour", "Point colour",
                   choices = c("blue", "red", "green", "black")),
      # Add a continent dropdown selector
      selectInput("continents", "Continents",
                  choices = levels(gapminder$continent),
                  multiple = TRUE,
                  selected = "Europe")
    ),
    mainPanel(
      plotOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    # Subset the gapminder dataset by the chosen continents
    data <- subset(gapminder,
                   continent %in% input$continents)

    p <- ggplot(data, aes(gdpPercap, lifeExp)) +
      geom_point(size = input$size, col = input$colour) +
      scale_x_log10() +
      ggtitle(input$title)
    
    if (input$fit) {
      p <- p + geom_smooth(method = "lm")
    }
    p
  })
}

shinyApp(ui = ui, server = server)

```

***

## Add a year filter: numeric slider input



```r

ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      numericInput("size", "Point size", 1, 1),
      checkboxInput("fit", "Add line of best fit", FALSE),
      radioButtons("colour", "Point colour",
                   choices = c("blue", "red", "green", "black")),
      selectInput("continents", "Continents",
                  choices = levels(gapminder$continent),
                  multiple = TRUE,
                  selected = "Europe"),
      # Add a slider selector for years to filter
      sliderInput("years", "Years", value = c(1977, 2002), min = min(gapminder$year), max = max(gapminder$year))
    ),
    mainPanel(
      plotOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    # Subset the gapminder data by the chosen years
    data <- subset(gapminder,
                   continent %in% input$continents &
                   year >= input$years[1] & year <= input$years[2])
    
    p <- ggplot(data, aes(gdpPercap, lifeExp)) +
      geom_point(size = input$size, col = input$colour) +
      scale_x_log10() +
      ggtitle(input$title)
    
    if (input$fit) {
      p <- p + geom_smooth(method = "lm")
    }
    p
  })
}

shinyApp(ui = ui, server = server)

```

***

## Add colours to your plot: colour input


```r

# Load the colourpicker package
library(colourpicker)

ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      numericInput("size", "Point size", 1, 1),
      checkboxInput("fit", "Add line of best fit", FALSE),

      # Replace the radio buttons with a colour input
      colourInput("colour", "Point colour", value = "blue"),
      selectInput("continents", "Continents",
                  choices = levels(gapminder$continent),
                  multiple = TRUE,
                  selected = "Europe"),
      sliderInput("years", "Years",
                  min(gapminder$year), max(gapminder$year),
                  value = c(1977, 2002))
    ),
    mainPanel(
      plotOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    data <- subset(gapminder,
                   continent %in% input$continents &
                   year >= input$years[1] & year <= input$years[2])
    
    p <- ggplot(data, aes(gdpPercap, lifeExp)) +
      geom_point(size = input$size, col = input$colour) +
      scale_x_log10() +
      ggtitle(input$title)
    
    if (input$fit) {
      p <- p + geom_smooth(method = "lm")
    }
    p
  })
}

shinyApp(ui = ui, server = server)

```
***

## Making your plot larger

```r
ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      numericInput("size", "Point size", 1, 1),
      checkboxInput("fit", "Add line of best fit", FALSE),
      colourInput("colour", "Point colour", value = "blue"),
      selectInput("continents", "Continents",
                  choices = levels(gapminder$continent),
                  multiple = TRUE,
                  selected = "Europe"),
      sliderInput("years", "Years",
                  min(gapminder$year), max(gapminder$year),
                  value = c(1977, 2002))
    ),
    mainPanel(
      # Make the plot 600 pixels wide and 600 pixels tall
      plotOutput("plot", height = "600px", width = "600px")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    data <- subset(gapminder,
                   continent %in% input$continents &
                     year >= input$years[1] & year <= input$years[2])
    
    p <- ggplot(data, aes(gdpPercap, lifeExp)) +
      geom_point(size = input$size, col = input$colour) +
      scale_x_log10() +
      ggtitle(input$title)
    
    if (input$fit) {
      p <- p + geom_smooth(method = "lm")
    }
    p
  })
}

shinyApp(ui = ui, server = server)

```

***

## Making your plot larger


plotOutput() - the default hight of a plot 400px

```r

ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      numericInput("size", "Point size", 1, 1),
      checkboxInput("fit", "Add line of best fit", FALSE),
      colourInput("colour", "Point colour", value = "blue"),
      selectInput("continents", "Continents",
                  choices = levels(gapminder$continent),
                  multiple = TRUE,
                  selected = "Europe"),
      sliderInput("years", "Years",
                  min(gapminder$year), max(gapminder$year),
                  value = c(1977, 2002))
    ),
    mainPanel(
      # Make the plot 600 pixels wide and 600 pixels tall
      plotOutput("plot", height = "600px", width = "600px")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  output$plot <- renderPlot({
    data <- subset(gapminder,
                   continent %in% input$continents &
                     year >= input$years[1] & year <= input$years[2])
    
    p <- ggplot(data, aes(gdpPercap, lifeExp)) +
      geom_point(size = input$size, col = input$colour) +
      scale_x_log10() +
      ggtitle(input$title)
    
    if (input$fit) {
      p <- p + geom_smooth(method = "lm")
    }
    p
  })
}

shinyApp(ui = ui, server = server)

```
***
## Make your plot interactive

```r

# Load the plotly package
library(plotly)

ui <- fluidPage(
  sidebarLayout(
    sidebarPanel(
      textInput("title", "Title", "GDP vs life exp"),
      numericInput("size", "Point size", 1, 1),
      checkboxInput("fit", "Add line of best fit", FALSE),
      colourInput("colour", "Point colour", value = "blue"),
      selectInput("continents", "Continents",
                  choices = levels(gapminder$continent),
                  multiple = TRUE,
                  selected = "Europe"),
      sliderInput("years", "Years",
                  min(gapminder$year), max(gapminder$year),
                  value = c(1977, 2002))
    ),
    mainPanel(
      # Replace the `plotOutput()` with the plotly version
      plotlyOutput("plot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  # Replace the `renderPlot()` with the plotly version
  output$plot <- renderPlotly({
    # Convert the existing ggplot2 to a plotly plot
    ggplotly({
      data <- subset(gapminder,
                     continent %in% input$continents &
                       year >= input$years[1] & year <= input$years[2])
      
      p <- ggplot(data, aes(gdpPercap, lifeExp)) +
        geom_point(size = input$size, col = input$colour) +
        scale_x_log10() +
        ggtitle(input$title)
      
      if (input$fit) {
        p <- p + geom_smooth(method = "lm")
      }
      p
    })
  })
}

shinyApp(ui = ui, server = server)

```

*End of Chapter 2*

