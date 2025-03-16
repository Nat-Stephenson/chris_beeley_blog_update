---
author: chrisbeeley
categories:
- Uncategorized
date: "2013-11-08T17:00:44Z"
guid: http://chrisbeeley.net/?p=537
id: 537
title: Free and open source software. Come for the free, stay for the awesome
url: /?p=537
---

I just wrote this for the Institute of Mental Health [blog](http://imhblog.wordpress.com/) so I thought I may as well cross-post here. I promise I will talk about something other than Shiny soon 🙂

Open source software refers to software whose code is available for anybody to view, revise and reuse. This should be contrasted with closed or proprietary code which is known only to the manufacturer, and who will often charge a fee for individuals to use the software (for example, Microsoft Word, or Adobe Photoshop).

There are many types of open source licence but the principle of viewing, revising, and reusing underpins them all. On the face of it, this does not seem like a very exciting proposition, and newcomers to the concept often wonder what difference such an apparently obscure part of software licensing could make.

However, because open source software (often, although not always, available at no cost, and referred to as Free and Open Source Software or FOSS) can be modified by anybody it offers a number of advantages over traditional proprietary software.

Firstly, FOSS is free. I’m writing this blog post on a computer which contains absolutely no software of any kind that costs any money whatsoever. I shall repost it on my self-hosted blog which runs on a server which itself contains no software of any kind that costs any money whatsover. In a time of austerity, the availability of free software that can perform all the tasks of paid software should be a deafening clarion call across the public sector. And indeed FOSS is starting to gain traction in [education](http://opensource.com/education/13/9/back-to-school-with-open-source) and [health](http://jamia.bmj.com/content/early/2013/06/05/amiajnl-2013-001671.full.pdf?keytype=ref&ijkey=ZhXd44bC9z9a7gr).

However, not only this, but FOSS is often better than its paid counterpart. This is because anybody can change and improve it, and whole communities exist around popular tools, adding new features and fixing bugs continuously with everyone inside and outside of the community benefiting. The FOSS operating system Linux basically [runs the internet](http://www.pcmech.com/article/would-the-internet-exist-without-linux/). The closed source Microsoft Excel, notoriously, features bugs that Microsoft can’t or won’t fix (e.g. [here](http://andrewgelman.com/2013/04/17/excel-bashing/), [here](http://homepage.cs.uiowa.edu/~jcryer/JSMTalk2001.pdf)). Even the US Military are making ever larger use of [open source sotware](http://news.cnet.com/8301-13505_3-9872263-16.html).

The statistical programming language [R](http://cran.r-project.org/) is another success story from the FOSS world. Precisely because it is free and open source, a huge community of statisticians, programmers, data visualisers and analysts all contribute to its development. At the time of writing there are nearly 5000 user contributed packages within R, which help users with tasks as diverse as computational chemistry and physics, finance, clinical trials, medical imaging, psychometrics, machine learning, statistical methods, and the production of extremely powerful and flexible statistical graphics. R is rapidly becoming the [lingua franca](http://www.nytimes.com/2009/01/07/technology/business-computing/07program.html?_r=0) of analytics and is widely used in many leading [data science departments](http://www.revolutionanalytics.com/companies-using-r), perhaps most notably Google.

I have made extensive use of R in my work on Nottinghamshire Healthcare NHS Trust’s new [patient feedback site](http://feedback.nottinghamshirehealthcare.nhs.uk/), using R to serve both static quarterly [reports](http://feedback.nottinghamshirehealthcare.nhs.uk/report) as well as interactive, searchable analysis of all of our [feedback](http://feedback.nottinghamshirehealthcare.nhs.uk/reports/custom) past and present.

The searchable reports are made possible by the [Shiny](http://www.rstudio.com/shiny/) package which makes it ridiculously easy to allow users to interact with a dataset. Shiny handles all of the hard work involved in programming a graphical user interface and lets analysts such as myself concentrate on the content of what is delivered to the user.

Although Shiny is never going to replace other methods of programming for very large, fully featured analytics platforms (for example, Google Analytics) it has certainly proved its worth on the patient feedback website and I would hope that R and Shiny would find more and more use in NHS settings up and down the country to allow Trusts to better communicate their data to the communities and service users whom they serve.

There are some demonstrations of what can be done with Shiny on my [website](http://chrisbeeley.net/website/), some are little examples I wrote just for the book but some are more fully featured than that and hopefully demonstrate some of the things which can be achieved using Shiny.

Over the next 10 years I hope to see more and more use of FOSS, not only for the Free, but also for the Awesome.