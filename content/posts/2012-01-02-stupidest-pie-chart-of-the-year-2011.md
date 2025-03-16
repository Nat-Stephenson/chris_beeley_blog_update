---
author: chrisbeeley
categories:
- Uncategorized
date: "2012-01-02T11:48:49Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=171
id: 171
publicize_results:
- a:1:{s:7:"twitter";a:1:{i:346491435;a:2:{s:7:"user_id";s:11:"ChrisBeeley";s:7:"post_id";s:18:"153804938949042176";}}}
tags:
- pie chart
title: Stupidest pie chart of the year 2011
url: /?p=171
---

I’ve just been looking at the [Stupidest bar chart of the year 2011](http://blog.infoadvisors.com/index.php/2011/12/22/stupidest-bar-chart-of-2011-congrats-klout/) and I’ve been inspired to submit my Stupidest pie chart of the year 2011. I won’t say from where I obtained it and I’ve drawn the data myself to save from annoying the original authors. And here it is (click to enlarge):

[![](http://chrisbeeley.net/wp-content/uploads/2012/01/rplot01.png?w=300 "Stupidest pie chart of the year, 2011")](http://chrisbeeley.net/wp-content/uploads/2012/01/rplot01.png)

It represents the amount of maternity and paternity leave available in different countries. Pie charts are often not a very good idea, but in this case they are the worst possible idea. Pie charts should be used when the whole pie has some meaning. A whole customer base, individuals living in Nottinghamshire, something like that. The whole of these pies represents- what? The total amount of leave in these countries. This has no real world meaning at all, and the whole point of the pie chart is lost.

Even worse, underneath the pie chart they are forced to write “Spare a thought for parents in the USA and Sierra Leone… paid maternity leave 0 weeks, paid paternity leave 0 weeks.” because you cannot represent 0 on a pie chart! This should have set alarm bells ringing. One better way to plot these data:

[![](http://chrisbeeley.net/wp-content/uploads/2012/01/rplot02.png?w=300 "Two minute bar chart")](http://chrisbeeley.net/wp-content/uploads/2012/01/rplot02.png)

This is absolutely factory-fresh out of the box settings, there are many ways to improve this plot and other types of plots which could be used. This plot improves on the previous one by:

1\. Better able to compare levels of leave in each country  
2\. Better able to compare levels of each type of leave  
3\. No need for data labels which spell out number of weeks in each country and contribute to very low data:ink ratio  
4\. Able to display zero points which puts the marginal notes about USA and Sierra Leone on the plot!

Code for both:

```
<pre class="brush: r; title: ; notranslate" title="">

par(mfrow=c(1, 2))

maternity=c(18, 16, 39, 17, 16, 0, 0)
paternity=c(3, 3, 14, 3, 14, 0, 0)
country=c("China", "Holland", "UK", "Greece", "France", "USA", "Sierra Leone")

pie(maternity, labels=paste(country, maternity, "weeks"), main="Maternity leave")

pie(paternity, labels=paste(country, paternity, "weeks"), main="Paternity leave")

# 2 minute bar chart

par(mar=c(10, 4, 4, 2) + 0.1)

barplot(maternity, names.arg=country, ylab="Maternity leave in weeks", las=3)

barplot(paternity, names.arg=country, ylab="Paternity leave in weeks", las=3)

```