---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-08-15T19:16:14Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=61
id: 61
title: useR conference update
url: /?p=61
---

I’m at the [useR! conference](http://www.warwick.ac.uk/statsdept/user-2011/) so I’ll be blogging every day with at least one thing that I learned.

The first thing, which I think I half-knew and had also half-learned from bitter experience, is that all the R experts seem to use Linux, Ubuntu in the case of the two people who ran the tutorials I attended today. I already dual-boot Windows and Linux and I think over time I’m going to reduce Windows to the operating system on which I play games and check my work email (which I can only get to run on Windows due to security software requirements).

The second thing is a neat way to plot regression when the outcome is binary. I’ve often wondered how I can visualise what’s going on when you have a horrible graph that looks like this:

[![](http://chrisbeeley.net/wp-content/uploads/2011/08/ugly1.png?w=300 "Ugly")](http://chrisbeeley.net/wp-content/uploads/2011/08/ugly1.png)

It’s very simple, and I now know thanks to Douglas Bates’s excellent lme4 tutorial. Draw a graph like this (points are suppressed, you can include them if you want):

[![](http://chrisbeeley.net/wp-content/uploads/2011/08/nice1.png?w=300 "Nice")](http://chrisbeeley.net/wp-content/uploads/2011/08/nice1.png)

Just a few lines of code for the whole kit and caboodle:

```
<pre class="brush: r; title: ; notranslate" title="">
library(lattice)

Outcome=sample(0:1, 100, replace=TRUE) # simulate data

Predictor=runif(100)*100 # simulate data

plot(Predictor, Outcome) # ugly graph

xyplot(Outcome~ Predictor, type = c(&quot;g&quot;, &quot;smooth&quot;), ylab = &quot;Outcome&quot;, xlab = &quot;Predictor&quot;) # useful graph
```

You can simulate the data properly so that there is an actual correlation if you want to (e.g. [here](http://www.biostat.wustl.edu/archives/html/s-news/2002-04/msg00103.html)) but I thought you’d spare me the bother- you get the idea.

Oh yes, I did learn one other thing today- they’re called packages, *not* libraries. One for the pedants among you.