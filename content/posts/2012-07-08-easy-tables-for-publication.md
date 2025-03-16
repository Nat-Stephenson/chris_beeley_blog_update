---
author: chrisbeeley
categories:
- Uncategorized
date: "2012-07-08T13:34:32Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=230
id: 230
title: Easy tables for publication
url: /?p=230
---

I’m writing for a journal article today and it’s the familiar problem of how to export all of the beautiful tables and graphs from LaTeX to Word (using R, of course).

It’s surprisingly easy to get a table from R into Word (or Open Office, or…). Just export the table to a text file and use tab-delimiting:

```
<pre class="brush: r; title: ; notranslate" title="">
write.table(mytable1, "Summary.txt", sep="t")
```

Then simply copy the table into the file, select “Convert text to table”, make sure “tab-delimiting” is selected, which it probably is, and voila.

You can be even sneakier than that though without too much effort.

I’ve been pasting together the familiar “mean (sd)” format used in tables quite easily like so:

```
<pre class="brush: r; title: ; notranslate" title="">
varNames=c("H.Tot", "C.tot", "R.tot", "HCRtot")

sapply(varNames, function(m) rbind(paste(round(tapply(mydata[,m], mydata[,"Dir"], mean, na.rm=TRUE), 2),
    " (", round(tapply(mydata[,m], mydata[,"Dir"], sd, na.rm=TRUE), 2), ")", sep="")))
```

Somehow coming up with generic solutions for transferring tables is a lot less depressing than sitting typing them out or manually tidying up SPSS output.

Apologies for the poor code formatting, incidentally, I can’t seem to work out how to format correctly on the blog. My version on RStudio is a lot more readable.

EDIT:

I’m glad I did this, because I’ve just been told that I put a variable in that I shouldn’t have done and so I have to re-do all the tables! Looks like I was right about generic solutions and typing!