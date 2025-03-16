---
author: chrisbeeley
categories:
- Uncategorized
date: "2015-09-17T14:09:47Z"
guid: http://chrisbeeley.net/?p=757
id: 757
title: Single file Shiny app to take GET parameters and return date range input boxes
url: /?p=757
---

I’ve spent quite a long time fiddling around with some code for the [feedback website](http://feedback.nottinghamshirehealthcare.nhs.uk/) for which I am responsible for all the reporting so we can distribute custom URLs to individuals who have specific reports they want to run. We use [Shiny](http://shiny.rstudio.com/) to do custom reports, the application lives [here](http://feedback.nottinghamshirehealthcare.nhs.uk/reports/custom), and many of the people who run the reports (our staff in [Nottinghamshire Healthcare NHS Trust](http://www.nottinghamshirehealthcare.nhs.uk/), mainly) would welcome the ability to have the date range preselected by means of a URL containing GET parameters which they could just click so that the date range is pre-selected.

The code is not very well commented and does not handle bad URLs very well. In time I may fix this or I may just tell people that if they can’t figure out how to write well formed GET querys that I will make some for them.

Nonetheless, I thought it would be useful to share in case anyone else is doing something similar and wants to get started or steal some of the code. It’s all hosted as a single file shiny app on Gist [here](https://gist.github.com/ChrisBeeley/79a592a83ccda153efae) which means that by the magic of Shiny you need only run:

```
<pre class="brush: r; title: ; notranslate" title="">

library(shiny)
runGist(gist = "79a592a83ccda153efae")

```

for a demo. You will need to paste the correct GET parameters in yourself, obviously. The app does one of three things, all selectable using a type=”xxx” GET parameter. There follows a list with a URL that does something for each one. The output to the right of the date widget is just some debugging stuff so I can see what’s happening as I go along, you may wish to keep something like that in anything you write while you’re building it.

You can run it on your own machine or I host it on my server, if you run on your own machine just add to the end of the address bar everything after and including the “?”, or click the link to see it on my server. It looks a bit funny because I have an old version of Shiny on the server, I can’t upgrade at the moment because of various issues with IE7 etc. but hope to soon. It works, anyway, which is the point, and looks fine on the newest version of Shiny (0.12 as I write this).

# type=date&amp;begin=XXXXXXX&amp;ending=XXXXXXXX

This is the simplest one, you just put the date (begin and/ or ending) straight in as dmy format (which UK people will prefer, more on which later) like this:

[http://chrisbeeley.net:8080/shiny/dateGETQuery/?type=date&amp;ending=02032014&amp;begin=03062011](http://chrisbeeley.net:8080/shiny/dateGETQuery/?type=date&ending=02032014&begin=03062011)

Missing out either is fine, if you miss the beginning one it will just go to a year ago and if you miss the end one it picks today.

# type=rolling&amp;window=3

This is for people who want to find the previous n months, rounding to months (e.g., today, in September, n = 3 would bring back the three months previous to September- June, July, and August).

[http://chrisbeeley.net:8080/shiny/dateGETQuery/?type=rolling&amp;window=3](http://chrisbeeley.net:8080/shiny/dateGETQuery/?type=rolling&window=3)

# type=quarter&amp;Q1=1&amp;Y1=2011&amp;Q2=4&amp;Y2=2013

And lastly and most complicated-ly, people who want to use just quarters and years. It took me ages to debug this because the quarter() function from the marvellous [lubridate](https://cran.r-project.org/web/packages/lubridate/index.html) package returns January as 1, April as 2, etc., which apparently is the US way of doing things, whereas in the UK January is 4 (of the previous year, obviously), April is 1, July is 2, etc. I never had any idea there were different systems. In fact, there are different systems all over the [world](https://en.wikipedia.org/wiki/Fiscal_year). Amazing what you learn some days.

[http://chrisbeeley.net:8080/shiny/dateGETQuery/?type=quarter&amp;Q1=1&amp;Y1=2011&amp;Q2=4&amp;Y2=2013](http://chrisbeeley.net:8080/shiny/dateGETQuery/?type=quarter&Q1=1&Y1=2011&Q2=4&Y2=2013)

Converting the output of quarter() to what I want entailed moving from a year that cycled 1, 2, 3, 4 to a year that cycled 4, 1, 2, 3 and so I used our old friend modulo arithmetic to convert between the two (%% in R). I won’t say anything further about that because I’m completely hopeless when it comes to modulo arithmetic and if I’m honest the code came about more through trial and error than anything else.

And that’s basically it. Browse the code, run the application, I hope it’s of some use to someone looking to do something similar.