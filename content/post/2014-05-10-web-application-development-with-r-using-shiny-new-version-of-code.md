---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-05-10T20:54:21Z"
guid: http://chrisbeeley.net/?p=631
id: 631
title: Web application development with R using Shiny new version of code
url: /?p=631
---

The new version of Shiny (0.9) is wonderful, hopefully I will talk about it again soon, but it does break the code in my book, [Web application development with R using Shiny](http://www.packtpub.com/web-application-development-with-r-using-shiny/book). It’s only a small difference, the selectInput() function used to ask for preselected elements to be given as the name of a list (i.e. the friendly version that appears to users in the UI, “Egg Salad”) whereas now it asks for the actual value (“eggSalad”, e.g.).

I wrote the book with 0.7 and I want to stay true to what’s in the book so I’ve added a couple of lines in to the [Google Analytics Advanced](https://github.com/ChrisBeeley/GoogleAnalyticsAdvanced) application to make it work with 0.9 and 0.7.

The code now starts with a version check like this:

```
<pre class="brush: r; title: ; notranslate" title="">

# test - are they using the older version of Shiny?

packageCheck = unlist(packageVersion("shiny"))

if(packageCheck[1] == 0 & packageCheck[2] < 9){
  
  shinyOld = TRUE
} else {
  shinyOld = FALSE
}

```

And there is one place in server.R and one in ui.R that checks whether that is true or false and changes its behaviour accordingly. It’s probably quicker for you to just control-F to shinyOld once in each codefile than have me tell you where they are.

As far as I can tell it now works beautifully on 0.9, please do let me know on here or on Twitter @chrisbeeley and I’ll be glad to have a look. Or if you have any other questions or comments I’m always glad to talk about them.