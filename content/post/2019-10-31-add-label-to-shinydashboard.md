---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-10-31T11:38:12Z"
title: Add label to shinydashboard
---

I feel like I’m sticking my neck out a bit here, and there’s a simple way to doing this that I haven’t found, but I’ve looked pretty hard and “add label to sidebar shiny dashboard” has basically no Google juice at all, and I should know because I’ve been staring at it for half an hour.

Sometimes you want to add a simple, static label to a shinydashboard sidebar. If you just add it with p() it isn’t aligned nicely with the rest of the sidebar controls.

You can add something dynamic with sidebarMenuOutput, and you could do that as a long way round, but I got to thinking that there must be a simple way of doing it. I ended up looking at my shinydashboard in developer view in Chrome, and just stealing the markup from there. Once you’ve done that it’s very simple.

```
div(class = "shiny-input-container", 
      p(paste0("Data updated: ", date_update))
```

This code just shows a pre-calculated value of the date when the data was updated. I stole this idea from a colleague because sometimes the cron job that updates the data chokes and nobody notices for a while.