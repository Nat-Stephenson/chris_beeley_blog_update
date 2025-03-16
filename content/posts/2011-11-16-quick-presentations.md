---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-11-16T16:44:38Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=147
id: 147
title: Quick presentations
url: /?p=147
---

Just a quick one from me today, lots of deadlines colliding, which is handy because that’s what this post is about. I’ve been experimenting with the [beamer](http://en.wikipedia.org/wiki/Beamer_(LaTeX)) class for LaTeX, with Sweave, which is another way of saying that I’ve been trying to save time producing slides for presentations directly from code as analysed by R.

The syntax is very simple, open a document with:

documentclass{beamer}  
usetheme{Berlin}  
title{Title goes here}  
author{Chris Beeley}  
date{today}  
begin{document}

Slides are included with:

begin{frame}  
section{Main findings}  
… slide content goes here  
end{frame}

The main strength, of course, is the ability to include graphics straight from R, and particularly in the analysis I’m doing today, because I’m pulling a lot of variables from an SPSS spreadsheet and making comparisons between the national figures and those for my areas. A lot of the code is the same for each graph.

```
<pre class="brush: r; title: ; notranslate" title="">
<<fig=TRUE, echo=FALSE, height=4, width=7>>=

par(mar=c(10, 4, 4, 2) + 0.1)

temp1=apply(maindata[which(names(maindata)=="dhelp1.01"):
which(names(maindata)=="dhelp1.10")], 2, 
function(m) prop.table(table(m))[2])

temp2=apply(subset(maindata, region=="East Midlands")[which
(names(maindata)=="dhelp1.01"): which(names(maindata)
=="dhelp1.10")], 2, function(m) prop.table(table(m))[2])

graphmat=rbind(temp1, temp2)*100

colnames(graphmat)=c("Personal care", "Physical help",
 "Services/ benefits", "Paperwork/ financial",
 "Other practical", "Company", "Taking out", 
"Giving medicines", "Keeping an eye", "Other")

barx=barplot(graphmat, beside=TRUE, ylim=c(0,100), col=c("red", "green"), las=3)

text(barx[1,], graphmat[1,]+5, paste(round(graphmat[1,]), "%"), cex=.5)
text(barx[2,], graphmat[2,]+5, paste(round(graphmat[2,]), "%"), cex=.5)

legend("topright", c("National", "East Midlands"), fill=c("red", "green"))

@
```

All I need to do is update the names of the variables in the which(names(mydata)==”x”) argument and then put some labels in which will be drawn on the x-axis. I have about 10 of these graphs to make. Just thinking about doing it all by hand makes me feel tired. And if I do decide to change some of the axis labels, or add to or take away from the variables in the graph, it’s just a question of quickly editing the code, and then recompiling to a pdf. Magic.