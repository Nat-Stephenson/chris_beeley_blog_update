---
author: chrisbeeley
categories:
- Uncategorized
date: "2013-06-18T08:48:31Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=414
id: 414
publicize_reach:
- a:2:{s:7:"twitter";a:1:{i:906009;i:50;}s:2:"wp";a:1:{i:0;i:3;}}
publicize_twitter_user:
- ChrisBeeley
title: Shiny application in a morning
url: /?p=414
---

Sort of apropos of nothing, really, this, but I was doing some research with some colleagues the other day and to help out I built a [shiny](http://www.rstudio.com/shiny/) application in a morning. Munging the data etc. took ages as it always does but once I actually had everything clean and tidy and working laying an interface on top was the easy bit. What a fantastic tool it really is, completely changed the way we work with data now we can just throw stuff on the internet and let people play around just in the morning. My sincere thanks to all the devs at [RStudio.](http://www.rstudio.com/)

Nothing Earth shattering really, just a few tick boxes and some binomial distribution numbers, but should prove useful. It’s [here](http://chrisbeeley.net:8080/shiny/Diagnoses/), in case you’re curious, source code below (I’ll just post the server.R and ui.R files on top of each other for simplicity, you get the idea.

```
<pre class="brush: r; title: ; notranslate" title="">

### server.R

library(shiny)
library(xtable)

rm(list=ls())

load("shiny.Rdata")

variables = names(shinyData)[4:19]

shinyServer(function(input, output) {
  
  myValues = reactive({
    
    if(length(input$prison) > 0){
      
      subData = subset(shinyData, Prison %in% input$prison)
      
    } else {
      
      subData = shinyData
      
    }
    
    if(length(input$traits) > 0){
      
      for(z in input$traits){
        
        subData = subData[which(subData[,z] == 1),]
        
      }
      
    }
    
    subData
    
  })
  
  output$outTable <- renderTable({
    
    Results = sapply(variables, function(x) rbind(prop.table(table(factor(myValues()[,x], levels=0:1))) * 100))[2,]
    
    CI = 100-sapply(variables, function(y) binom.test(table(factor(myValues()[,y], levels = 0:1)))[["conf.int"]])*100
    
    finalTab = data.frame("Estimate" = Results, t(CI[2:1,]))
    
    names(finalTab)[2:3] = c("Low CI", "High CI")
    
    xtable(finalTab, digits=1)
    
  })
  
  output$nResponses = renderText({
    
    paste("n = ", nrow(myValues()))
    
  })
  
})

### ui.R

library(shiny)

shinyUI(pageWithSidebar(
  
  # Application title
  headerPanel("Summary of database results"),
  
  sidebarPanel(
    
    checkboxGroupInput("prison",
                       "Area",
                       c("A", "B", "C"),
                       selected = c("A", "B", "C")
    ),
    
    checkboxGroupInput("traits",
                       "Characteristics",
                       c("Drug", "Psychosis", "Depression", 
                         "Anxiety", "BPD", "Alcohol", "PTSD", "OCD", "Bipolar", "Paranoid.PD", 
                         "Dependent.PD", "Dissocial.PD", "PD", "Referral", "Any", "NonDrug"),
                       selected = c(NULL)
    )
        
  ),
  
  mainPanel(
   tableOutput("outTable"),
    textOutput("nResponses")
  )
))
```