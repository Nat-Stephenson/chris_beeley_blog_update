---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-10-02T13:55:24Z"
guid: https://chrisbeeley.net/?p=1157
id: 1157
title: The shinyverse
url: /?p=1157
---

I’m pretty sure I’ve made up the word shinyverse, to refer to all the packages that either use or complement Shiny. This is a non-exhaustive list of shinyverse packages, mainly for my benefit when I inevitably forget, but if it’s useful for you too then that’s good too.

[shiny.semantic](https://cran.r-project.org/web/packages/shiny.semantic/index.html) adds support for the Semantic UI library to Shiny. Useful if you want your Shiny apps to look a bit different or there’s something in the library that you need.

[shinyaframe](https://cran.r-project.org/web/packages/shinyaframe/index.html) doesn’t particularly sound like the kind of thing that everyone reading this will drop everything to do on a rainy weekend, but it’s awesome so it’s worth mentioning here. It allows you to use Mozilla A-Frame to create virtual reality visualisations with Shiny!

[shinycssloaders](https://cran.r-project.org/web/packages/shinycssloaders/index.html) does very nice simple loading animations for while your Shiny application is thinking about how to draw an output.

[shinydashboard](https://cran.r-project.org/web/packages/shinydashboard/index.html) is so commonly used that I tend to think of it as what you might call “base-Shiny” (another terms I made up). If you’ve never made a Shiny dashboard I suggest you have a look.

[shinyWidgets](https://rstudio.github.io/crosstalk/) is a lovely package that extends the number of widgets you can use. I’ve used the date picker in the past but there are plenty to choose from.

[shinythemes](https://cran.r-project.org/web/packages/shinythemes/index.html) is another one that feels like base-Shiny. Change the appearance of you Shiny apps very easily by selecting one of several themes in the UI preamble.

[shinytest](https://cran.r-project.org/web/packages/shinytest/shinytest.pdf) is for testing Shiny applications.

[shinymaterial](https://cran.r-project.org/web/packages/shinymaterial/index.html) allows you to use a UI design based on the Google Material look.

[shinyLP](https://cran.r-project.org/web/packages/shinyLP/index.html) allows you to make a landing page for Shiny apps, either to give more information or to allow a user to select one of several applications.

[shinyjs](https://cran.r-project.org/web/packages/shinyjs/index.html). Another base-Shiny one. Easily add canned or custom JavaScript

[shinyFiles](https://cran.r-project.org/web/packages/shinyFiles/index.html) allows the user to navigate the file system on the server.

[bsplus](https://cran.r-project.org/web/packages/bsplus/index.html) allows you to add lots of things which are in Bootstrap but not in Shiny to your apps (and to RMarkdown, to boot).

[rintrojs](https://cran.r-project.org/web/packages/rintrojs/) allows you to make JavaScript powered introductions to your app. Pretty neat.

[crosstalk](https://rstudio.github.io/crosstalk/using.html) is seriously neat and allows you to use Shiny widgets that have been programmed to interact with each other, or write this interaction yourself if you’re a package author. Once you or someone else has done this, Shiny (or RMarkdown) outputs can automatically react to each other’s state (filtering, brushing etc.)