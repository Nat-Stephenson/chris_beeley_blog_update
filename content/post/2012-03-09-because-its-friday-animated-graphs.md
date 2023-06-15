---
author: chrisbeeley
categories:
- Uncategorized
date: "2012-03-09T12:33:22Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=189
id: 189
publicize_results:
- a:1:{s:7:"twitter";a:1:{i:346491435;a:2:{s:7:"user_id";s:11:"ChrisBeeley";s:7:"post_id";s:18:"178096180394340352";}}}
tags:
- animation
- ggplot2
- Visualisation
title: Because it&#8217;s Friday- animated graphs
url: /?p=189
---

I’ve been analysing some data and we’re a bit unsure, as we often are, as to its quality. We have a variety of clinicians rating C.tot and R.tot over a four year period for a set of patients. To summarise, there was some concern that people are rating C.tot consistently and well but not so with R.tot (I’m not going to explain the variables or where they come from because it’s not relevant and it’s an internal dataset we’re not ready to share yet).

I’m sure there is a statistical approach (essentially we need to check to see if the R.tot scores are jumping around) but I thought a graphical approach might suffice for now.

So I talk all the individuals who had been in the dataset for long enough, and put each of their score trajectories on a graph, and then animated so you see the first patient, then the second and the first, then the first second and third, etc.

Actually the R.tot scores look pretty sensible looking at the animation. Have a look and see what you think (click to animate).

[![](http://chrisbeeley.net/wp-content/uploads/2012/03/animation.gif?w=300 "animation")](http://chrisbeeley.net/wp-content/uploads/2012/03/animation.gif)

So easy to achieve with the mighty ggplot and animation packages from R, here’s all the code

```
<pre class="brush: r; title: ; notranslate" title="">

library(animation)
library(ggplot2)

mydf=melt(mytemp, measure.vars=c("R.tot", "C.tot"), id.vars=c("ID", "Week"))

for (i in unique(mydf$ID)) {
  
  png(filename=paste(i, ".png", sep=""))
  
  print(
    ggplot(subset(mydf, ID &lt;= i), aes(x=Week, y=value)) + 
      geom_line(aes(group=ID)) +
      xlab("Week") + 
      opts(title = "Clinical and risk score over time") +
      facet_wrap(~variable, ncol=1)
    )
  dev.off()
  
}

## change outdir to the current working directory
ani.options(outdir = getwd(), interval=0.05)

im.convert(paste(unique(mydf$ID), ".png", sep=""), output = "animation.gif")

```