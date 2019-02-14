# Chapter 3 - Customizing Style

## Create body with row-based layout

creating two rows layout

```
# Row 1
  box(
    width = 12,
    title = "Regular Box, Row 1",
    "Star Wars"
    )
    
```

Second row

```

# Row 2
  box(
    width = 12,
    title = "Regular Box, Row 2",
    "Nothing but Star Wars"
    )
    

```
> max width is 12

```r

library("shiny")

body <- dashboardBody(
  fluidRow(
# Row 1
  box(
    width = 12,
    title = "Regular Box, Row 1",
    "Star Wars"
  
    )
  ),
# Row 2
fluidRow(
  box(
    width = 12,
    title = "Regular Box, Row 2",
    "Nothing but Star Wars"
    )
  )
)

ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = dashboardSidebar(),
                    body = body
                    )
shinyApp(ui, server)

```

***

## Create body with column-based layout

```r

library("shiny")

body <- dashboardBody(
  fluidRow(
# Column 1
    column(width = 6,
      infoBox(
      width = NULL,
      title = "Regular Box, Column 1",
      subtitle = "Gimme those Star Wars"
      )
    
    ),
# Column 2
    column(width = 6,
      infoBox(
      width = NULL,
      title = "Regular Box, Column 2",
      subtitle = "Don't let them end"
      )
    )
  )
)

ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = dashboardSidebar(),
                    body = body
                    )
shinyApp(ui, server)

```

***

## Create body with mixed layout


```r

library("shiny")

body <- dashboardBody(
  fluidRow(
# Row 1
  box(width = 12, title = "Regular Box, Row 1", "Star Wars, nothing but Star Wars")
  ),
  fluidRow(
# Row 2, Column 1
column(width = 6,
    infoBox(
    width = NULL,
    title = "Regular Box, Row 2, Column 1",
    subtitle = "Gimme those Star Wars"
      )
    ),
# Row 2, Column 2
  column(width = 6,
    infoBox(
    width = NULL,
    title = "Regular Box, Row 2, Column 2",
    subtitle = "Don't let them end"
      )
    )
  )
)

ui <- dashboardPage(header = dashboardHeader(),
                    sidebar = dashboardSidebar(),
                    body = body
                    )
shinyApp(ui, server)

```

***


## Change the appearance of the dashboard

```r
# Update the skin
ui <- dashboardPage(
  skin = "purple",
  header = dashboardHeader(),
  sidebar = dashboardSidebar(),
  body = body)
  
# Run the app
shinyApp(ui, server)

```

***

## Customize the body with CSS

Using `tags()` function

```
body <- dashboardBody(
    tags$head(
        tags$style(
            HTML('
            \\Add CSS here
            ')
        )
    )
)

```

Code

```r

library("shiny")

body <- dashboardBody(
# Update the CSS
  tags$head(
        tags$style(
            HTML('
            h3 {
              font-weight: bold;
            }')
        )
    ),
  fluidRow(
    box(
      width = 12,
      title = "Regular Box, Row 1",
      "Star Wars, nothing but Star Wars"
    )
  ),
  fluidRow(
    column(width = 6,
      infoBox(
        width = NULL,
        title = "Regular Box, Row 2, Column 1",
        subtitle = "Gimme those Star Wars"
    )
   ),
    column(width = 6,
      infoBox(
        width = NULL,
        title = "Regular Box, Row 2, Column 2",
        subtitle = "Don't let them end"
    )
  )
 )
)

ui <- dashboardPage(
  skin = "purple",
  header = dashboardHeader(),
  sidebar = dashboardSidebar(),
  body = body)
shinyApp(ui, server)

```

***

## Incorporate icons

Icons can be found from [Font Awesome website](http://fontawesome.io/icons/).
icons can be rendered using the `icon()` function in shiny.

```r
library("shiny") 

header <- dashboardHeader(
  dropdownMenu(
    type = "notifications",
    notificationItem(
      text = "The International Space Station is overhead!",
      icon = icon("rocket")
    )
  )
)
ui <- dashboardPage(header = header,
                    sidebar = dashboardSidebar(),
                    body = dashboardBody()
                    )
shinyApp(ui, server)

```

## Add some life to your layouts

some valid statuses and their corresponding colors are:

* `primary` Blue (sometimes dark blue)
* `success` Green
* `info` Blue
* `warning` Orange
* `danger` Red


```r

library("shiny")

body <- dashboardBody(
  tags$head(
    tags$style(
      HTML('
      h3 {
        font-weight: bold;
      }
      ')
    )
  ),
  fluidRow(
    box(
      width = 12,
      title = "Regular Box, Row 1",
      "Star Wars, nothing but Star Wars",
# Make the box red
      status = "danger"
    )
  ),
  fluidRow(
    column(width = 6,
      infoBox(
        width = NULL,
        title = "Regular Box, Row 2, Column 1",
        subtitle = "Gimme those Star Wars",
# Add a star icon
        icon = icon("star")
    )
   ),
    column(width = 6,
      infoBox(
        width = NULL,
        title = "Regular Box, Row 2, Column 2",
        subtitle = "Don't let them end",
# Make the box yellow
        color = "yellow"
    )
  )
 )
)

ui <- dashboardPage(
  skin = "purple",
  header = dashboardHeader(),
  sidebar = dashboardSidebar(),
  body = body)
shinyApp(ui, server)


```

***

*End of Chapter 3*

