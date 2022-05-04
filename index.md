---
title       : Developing Data Products Week 4 Assignment
subtitle    : 05/04/22
author      : Ariel Gershman
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---


## Coursera Peer Graded Assignment Reproducible Pitch

### You can find the details of the assignment from the web adress:

-https://www.coursera.org/learn/data-products/peer/tMYrn/course-project-shiny-application-and-reproducible-pitch

-In this assignment, I prepared a shiny application and the link is :
https://gmoneyomics.shinyapps.io/Developing-Data-Products/

-Also, the codes of server.R and ui.R are on the link: https://github.com/gmoneyomics/Developing-Data-Products-Week-4-Assignment

--- 

## Data used for the Shiny Application

### The data used for the app is the USArrests datasets:


```r
head(USArrests)
```

```
##            Murder Assault UrbanPop Rape
## Alabama      13.2     236       58 21.2
## Alaska       10.0     263       48 44.5
## Arizona       8.1     294       80 31.0
## Arkansas      8.8     190       50 19.5
## California    9.0     276       91 40.6
## Colorado      7.9     204       78 38.7
```

---

## UI Code

```r
library(shiny)
# Define UI for app that draws a plot 
ui <- fluidPage(
  
  # App title ----
  titlePanel("Which crimes are more correlated with murder rates in the US?"),
  
  # Sidebar layout with input and output definitions
  sidebarLayout(
    
    # Sidebar panel for inputs 
    sidebarPanel(
      
    selectInput(
      inputId = "picked",
      label = "Crime:",
      choices = c("Assault", "Rape"),
      selected = "Assault",
      multiple = FALSE)
    ),
    
    # Main panel for displaying outputs
    mainPanel(
      plotOutput(outputId = "distPlot")
      
    )
  )
)
```

---

## Server Code

```r
library(ggpmisc)

server <- function(input, output) {

  output$distPlot <- renderPlot({
    req(input$picked)
    ggplot(USArrests, aes(x=Murder, y=.data[[input$picked]]))+geom_point()+stat_smooth(method="lm")+
      stat_poly_eq(formula =  y ~ x, 
                   aes(label = paste(..eq.label.., ..rr.label.., sep = "~~~")), 
                   parse = T)  
    
    
  })
  
}
```






