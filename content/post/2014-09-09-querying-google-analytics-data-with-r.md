---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-09-09T14:14:21Z"
guid: http://chrisbeeley.net/?p=696
id: 696
title: Querying Google analytics data with R
url: /?p=696
---

This post is in response to a question on Twitter, but it may be of interest to others. I produced a Shiny app for querying Google Analytics data for my [book](https://www.packtpub.com/web-development/web-application-development-r-using-shiny). The data on the live version which lives [here](http://chrisbeeley.net:8080/shiny/GA/) is automatically pulled in via the API each time the application is run. That code cannot live on my [GitHub](https://github.com/ChrisBeeley) because it needs the username and password for the API. So I’m reproducing it here. It’s very simple and is based on the wonderful R package rga which can be found [here](https://github.com/skardhamar/rga). Obviously the XXXXXXXs have got real things in the real version, this is the redacted version.

It’s pretty self explanatory really with the documentation for the package, so I won’t labour it by explaining any of the code. The only thing perhaps of note is the where=”ga.rga” instruction in the rga.open() call, this allows you to store a file called ga.rga which stores the authentication credentials of your application and means you don’t need to keep entering the code that Google gives you the first time you run it.

I’m happy to get questions as comments to this blog post or on my Twitter @chrisbeeley.

```
<pre class="brush: r; title: ; notranslate" title="">

library(rga)

rga.open(instance = "ga", where="ga.rga", 
         client.id = "XXXXXX.apps.googleusercontent.com", 
         client.secret = "XXXXXX")

analytics = ga$getData(XXXXXX, batch = TRUE,
                       start.date = "2013-01-01",
                       metrics = "ga:visitors, ga:visits, ga:bounces, ga:timeOnSite",
                       dimensions = "ga:dateHour, ga:networkDomain",
                       sort = "", filters = "", segment = "")

analytics$Date = as.Date(paste0(substr(analytics$dateHour, 1, 4), "/",
                                substr(analytics$dateHour, 5, 6), "/", substr(analytics$dateHour, 7, 8)))

analytics$Hour = as.numeric(substr(analytics$dateHour, 9, 10))

# change domain

analytics$Domain = factor(recode(analytics$networkDomain, '"nhs.uk" = "NHS"; else = "Other"'))

```