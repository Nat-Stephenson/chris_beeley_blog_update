---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-08-02T15:59:52Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=25
id: 25
tags:
- data visualisation
- patient survey
- text mining
- word clouds
title: Fun with wordclouds
url: /?p=25
---

As always, I’m late to this party, and wordclouds have come under fire in recent times, e.g. here: [drewconway.com](http://www.drewconway.com/zia/?p=2624). From my point of view they’re eye-catching, and I hope that by putting them up on a website or in a report they might cause people to linger and look in more detail at other pieces of data and visualisation. That’s all I’m going to say for now, I’m sure I’ll talk again about what’s attractive to data scientists and statisticians and what’s attractive to the general public, but let’s leave it for now.

I am looking at interesting ways of looking at the patient survey (see previous post) at the moment and I thought I would have a go at a wordcloud. Thanks to the wonderful people producing packages for R (the tm and wordcloud packages, many thanks to both!), it’s easy. I nicked a bit of code from another blog (thanks [One R tip a day](http://onertipaday.blogspot.com/2011/07/word-cloud-in-r.html)!) and pretty soon I had my own. It’s from two areas of the Trust, featuring the things people like about our services.

Here’s the code:

```
<pre class="brush: r; title: ; notranslate" title="">

mydata=subset(mydatafirst, Var1==0) # slice the data by area

mycorpus=Corpus(DataframeSource(data.frame(mydata[-is.na(mydata$Best),34]))) # make a text corpus for the tm package out of a data frame, removing missing values at the same time

mycorpus &lt;- tm_map(mycorpus, removePunctuation) # remove all the puncuation
mycorpus &lt;- tm_map(mycorpus, tolower) # make everything lower case
mycorpus &lt;- tm_map(mycorpus, function(x) removeWords(x, c(stopwords(&quot;english&quot;), &quot;none&quot;, &quot;ive&quot;, &quot;dont&quot;, &quot;etc&quot;))) # remove words without meaning, many of which are provided by the stopwords(&quot;english&quot;) function

# these next steps take the corpus and turn it into a dataframe ready for the wordcloud function

tdm &lt;- TermDocumentMatrix(mycorpus)
m &lt;- as.matrix(tdm)
v &lt;- sort(rowSums(m),decreasing=TRUE)
d &lt;- data.frame(word = names(v),freq=v)

par(mfrow=c(1,2))

wordcloud(d$word,d$freq,c(3.5,.5),2,100,TRUE,.15, terrain.colors(5),vfont=c(&quot;sans serif&quot;,&quot;plain&quot;))

# ...same steps again for the other area

wordcloud(d2$word,d2$freq,scale=c(4,.5),2,100,TRUE,.15, terrain.colors(5),vfont=c(&quot;sans serif&quot;,&quot;plain&quot;))

```

I adjusted by eye the scale which you can see in the 3rd parameter of the wordcloud function (making the second area a bit larger). There’s probably a better way, I will investigate further as I look more at what to do with all this data.

Here’s the word cloud:

[Rplot02](http://chrisbeeley.net/wp-content/uploads/2011/08/rplot02.pdf)

Not bad for a few hours’ work! I’m hoping it will draw people in to look at more of the reporting that we do at any rate. Let me know what you think of it, and word clouds generally, in the comments.