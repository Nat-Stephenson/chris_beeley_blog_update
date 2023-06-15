---
author: chrisbeeley
categories:
- Uncategorized
date: "2011-09-01T06:36:40Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=99
id: 99
tags:
- mean
- median
title: Misleading means and medians
url: /?p=99
---

Over at [this](http://exploringdatablog.blogspot.com/2011/08/some-additional-thoughts-on-useless.html) excellent blog there is an interesting discussion about times when means and medians can be deceptive, particularly in the case where two variables with equal means have very different distributions. I chimed in myself and mentioned some of the examples which I come across in my work. Here is a particularly egregious example, measurement of self-esteem in patients on psychiatric wards in England and Belgium-

England mean  
3.4  
Belgium mean  
3.2  
England median  
3.3  
Belgium median  
3.2  
England sd  
1.2  
Belgium sd  
0.9

Looks pretty similar on the face of it. Let’s have a look at the actual distribution (click to enlarge).

[![](http://chrisbeeley.net/wp-content/uploads/2011/09/selfesteem.png?w=300 "SelfEsteem")](http://chrisbeeley.net/wp-content/uploads/2011/09/selfesteem.png)

Pretty different. Quite interesting to consider why the two are so different. It would appear on the face of it that the measure works better in Belgium, producing a nice normal distribution, and not so well in England, where many individuals are selecting the maximum response across all the items in the scale.

Too often, I think, we talk about non-normal distributions in terms of their median, when as you can see here, many sins can be hidden in this way. I don’t know why the self-esteem measure is behaving like this in England, but we haven’t finished with these data so look out for more on the blog as we have a more thorough look.

R code:

```
<pre class="brush: r; title: ; notranslate" title="">
mygraph=function(x){
  
par(mfrow=c(1,2))  

a=subset(mydata, country==1)
b=subset(mydata, country==2)

print(&quot;England mean&quot;)
print(mean(a[,x], na.rm=TRUE))
print(&quot;Belgium mean&quot;)
print(mean(b[,x], na.rm=TRUE))

print(&quot;England median&quot;)
print(median(a[,x], na.rm=TRUE))
print(&quot;Belgium median&quot;)
print(median(b[,x], na.rm=TRUE))

print(&quot;England sd&quot;)
print(sd(a[,x], na.rm=TRUE))
print(&quot;Belgium sd&quot;)
print(sd(b[,x], na.rm=TRUE))

hist(a[,x], main=&quot;England&quot;, xlab=&quot;Self esteem score&quot;, breaks=seq(0,5,by=.2), freq=FALSE)
lines(density(a[,x], na.rm=TRUE))
hist(b[,x], main=&quot;Belgium&quot;, xlab=&quot;Self esteem score&quot;, breaks=seq(0,5,by=.2), freq=FALSE)
lines(density(b[,x], na.rm=TRUE))
}

mygraph(&quot;Selfesteem&quot;)
```