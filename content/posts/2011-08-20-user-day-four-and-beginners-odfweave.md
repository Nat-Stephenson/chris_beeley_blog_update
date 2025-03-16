---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-08-20T07:05:32Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=87
id: 87
title: useR day four and beginner&#8217;s odfWeave
url: /?p=87
---

I’m a bit late with the report from the last day of the useR conference, I was very busy Thursday getting home and catching up with the housework on Friday. I once again favoured sessions about graphics and best in show must go to [Easy interactive ggplots](http://4dpiecharts.com/2011/08/17/user2011-easy-interactive-ggplots-talk/), particularly for a bit of a coding novice like myself. I’m going to take some of the ideas from this talk and use them at work, it will blow everyone’s mind (well, doing it for free with blow everyone’s mind, anyway) so a big thank you to Richie for sharing his ideas and his code.

Since I got home I’ve been manfully struggling with odfWeave again, so I’m going to give a quick guide for new starters like myself in the hope that someone’s Google search for “odfWeave problems” or the suchlike will point them here. It’s for beginners, so if you’re not a beginner then you may as well skip this bit and go for a walk or something.

1\) DON’T use windows. Even if you can get the zip/ unzip bit working, which I did eventually, you’ll hit a problem when you want to use an older version of the XML library (which you have to, see below)- unless you’re going to compile it yourself. If you’re going to compile it yourself, you clearly have a different definition of the word “beginner” than I do, and congratulations you on being so modest.

2\) DO downgrade your XML version (or if you haven’t installed it yet, never install 3.4 in the first place). It’s dead easy to do, just go [here](http://cran.r-project.org/src/contrib/Archive/XML) and download the 3.2 version, which works beautifully. (use the install.packages() command with a pointer to the file, or if you use the fabulous [RStudio](http://rstudio.org/), which I deeply love, then they have an option to use a downloaded file after their install packages button).

I think you should be pretty okay after that. I still got confused by some of the code snippets, so let’s just have a quick round-up of stuff you might want to do and how to do it.

Inline code is just like Sweave, so it’s

Sexpr{version$version.string}

Blocks of code, also, are just like Sweave. Just in case you get in a muddle, which to be honest I did:

1\) Code you want to print (for presentations etc.:

```
<pre class="brush: r; title: ; notranslate" title="">
&lt;&lt;echo=TRUE&gt;=
rnorm(100)
@
```

Code chunk finished with @

2\) Code you want a graphic out of:

```
<pre class="brush: r; title: ; notranslate" title="">
&lt;&lt;fig=TRUE, echo=FALSE&gt;&gt;=
hist(rnorm(100))
@
```

DON’T forget to encapsulate ggplot2 commands like ggplot and qplot in a print() because of some technical reason that essentially means “that’s just how you do it”. Code chunk finished with @

3\) Code you want a table out of:

```
<pre class="brush: r; title: ; notranslate" title="">
&lt;&lt;Table1,echo=FALSE,results=xml&gt;&gt;=
tempmat=matrix(with(mydata, table(Ethnicity, Control)), ncol=2)
row.names(tempmat)=row.names(with(mydata, table(Ethnicity, Control)))

odfTable(tempmat, useRowNames=TRUE, colnames=c(&quot;&quot;, &quot;Control&quot;, &quot;Treatment&quot;))
@
```

You’ll use the odfTable command, which does NOT work on table() objects, just turn them into a matrix or a dataframe and they work fine. Notice how I’ve put the row and column names in as well, it does mention it in the help file, but that’s how you do it anyway. Code chunk finished with @

I think that should be enough to get you started. I wish I’d read this post about 3 months ago, because I’ve been fiddling with odfWeave on and off since then (I started on Windows, I think that was my main mistake really, couldn’t get the zip and unzip commands to work for ages).

Here’s the first bit of the code from a report I’m writing to give you an idea.

```
<pre class="brush: r; title: ; notranslate" title="">
Age
This was compared by comparing the distribution and median of the age of individuals in each group.

&lt;&lt;fig=FALSE, echo=FALSE&gt;&gt;=

rm(list=ls())
library(ggplot2)
library(car)

mydata=read.csv(&quot;/home/chris/Dropbox/R-files/GCAMT/TCComparison.csv&quot;)
mydata$Control=factor(mydata$Control, labels=c(&quot;Control&quot;, &quot;Treatment&quot;))

@

&lt;&lt;fig=TRUE, echo=FALSE&gt;&gt;=

# DOB

mydata$DOB2=as.Date(mydata$DOB, format=&quot;%Y-%m-%d&quot;, origin=&quot;1899-12-30&quot;)

print(

qplot(DOB2, data=mydata, geom=&quot;density&quot;, colour= Control, main=&quot;Median DOB marked blue for treatment, red for control&quot;, xlab=&quot;Date of birth&quot;, ylab=&quot;Density&quot;) + 
	geom_vline(xintercept = median(subset(mydata, Control==&quot;Treatment&quot;)$DOB-25569), col=&quot;blue&quot;, lty=2) + 
	geom_vline(xintercept = median(subset(mydata, Control==&quot;Control&quot;)$DOB-25569), col=&quot;red&quot;, lty=2) 

	)


@
Ethnicity
Ethnicity within treatment and control is shown below.

&lt;&lt;Table1,echo=FALSE,results=xml&gt;&gt;=
# &quot;Ethnicity&quot;

tempmat=matrix(with(mydata, table(Ethnicity, Control)), ncol=2)
row.names(tempmat)=row.names(with(mydata, table(Ethnicity, Control)))

odfTable(tempmat, useRowNames=TRUE, colnames=c(&quot;&quot;, &quot;Control&quot;, &quot;Treatment&quot;))

@
```