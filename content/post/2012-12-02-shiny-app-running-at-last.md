---
author: chrisbeeley
categories:
- Uncategorized
date: "2012-12-02T21:37:09Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=293
id: 293
publicize_twitter_user:
- ChrisBeeley
title: Shiny app running at last
url: /?p=293
---

I’m currently taking part in the NHS Institute for Innovation supported Patient Feedback challenge with colleagues at Nottinghamshire Healthcare NHS Trust. We’re delivering our patient feedback in a web system and I have been working with the web developers, the lovely people over at [Numiko](http://numiko.com/#/home), they’re doing all the web type stuff and I’m writing R code, running over [FastRWeb](http://www.rforge.net/FastRWeb/), which is fantastic and will get a well deserved blog post some time soon.  
In order to make sure that my code keeps pace with the interface they design and there’s no nasty surprises when the launch of the website gets near I’ve been doing some prototyping with the wonderful [Shiny](http://www.rstudio.com/shiny/) package from the amazing people over at RStudio, whose IDE I sincerely love.

I’ve done a little video about it for the challenge which I thought I might as well share on here. I’m blown away by how easy it is to use and although I’m using it for prototyping in this case I can imagine building a whole tool with it and getting something really useful and attractive, maybe with a little bit of JavaScript jiggery-pokery.

[Video](https://vimeo.com/54727705)

I won’t share all the server side code because it’s a bit of a mess at the moment, I’ll put a cleaner version up on GitHub once it’s a bit further along, but here’s the UI code just to demonstrate how simple it is (really the server side just picks up one or two of the variables and then does various graphs etc. based on code I already had).

```
<pre class="brush: r; title: ; notranslate" title="">

library(shiny)
# Define UI
shinyUI(pageWithSidebar(

# Application title
headerPanel("SUCE results"),

# first set up All/ Division results

sidebarPanel(
selectInput("Division", "Select division", list("Trust" = 9, "Local"= 0, "Forensic"=1, "HP" = 2)),
conditionalPanel(
condition = "input.Division != 9",
uiOutput("divControls")
),
textInput("keyword", "Keyword search: (e.g. food, staff)"),
selectInput("start", "From: ", list("Apr - Jun 11" = 9, "Jul - Sep 11" = 10, "Oct - Dec 11" = 11,
"Jan - Mar 12"= 12, "Apr - Jun 12" = 13, "Jul - Sep 12" = 14)),
selectInput("end", "To: ", list("Apr - Jun 11" = 9, "Jul - Sep 11" = 10, "Oct - Dec 11" = 11,
"Jan - Mar 12"= 12, "Apr - Jun 12" = 13, "Jul - Sep 12" = 14),
selected = "Jul - Sep 12"),
checkboxInput("custom", "Advanced controls", value=FALSE),
conditionalPanel(
condition = "input.custom == true",
selectInput("responder", "Responder type", list("All" = 9, "Service user"= 0, "Carer"=1)),
selectInput("sex", "Gender", list("All" = "All", "Men"= "M", "Women"= "F"))
)

),

# Show the caption and plot of the requested variable
mainPanel(
h3(textOutput("Title")),
tabsetPanel(
tabPanel("Stacked plot", plotOutput("StackPlot")), 
tabPanel("Trend", plotOutput("TrendPlot")), 
tabPanel("Responses", tableOutput("TableResponses"))
))
))
 

```