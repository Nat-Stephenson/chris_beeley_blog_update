---
author: chrisbeeley
categories:
- Uncategorized
date: "2012-04-07T10:02:59Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=209
id: 209
publicize_results:
- a:1:{s:7:"twitter";a:1:{i:346491435;a:2:{s:7:"user_id";s:11:"ChrisBeeley";s:7:"post_id";s:18:"188567542342754304";}}}
title: Parallelising with R
url: /?p=209
---

I’ve been fitting some generalised linear mixed effects models with 400,000 rows or so and it takes a while even on my quad core 3.4Ghz behemoth. The time has come for me to learn how to use multiple cores with R. The [parallel](http://stat.ethz.ch/R-manual/R-devel/library/parallel/doc/parallel.pdf) library which comes with the new-ish R 2.14 makes it pretty easy. For my purposes, all I needed to do was pop all of my calculations in a list and then call [mclapply](http://www.rforge.net/doc/packages/multicore/mclapply.html) on them.

This is my first time parallelising so this might not be the best way to do it, if you know better please say in the comments, but if you are interested to give it a try then this should get you started.

```
<pre class="brush: r; title: ; notranslate" title="">

mymodels=list("glmer(AllViol~Clinical*Dir+(1|ID),
              family=binomial, data=testF)",
              "glmer(AllViol~Risk*Dir+(1|ID), 
              family=binomial, data=testF)",
              "glmer(AllViol~Historical*Dir+(1|ID), 
              family=binomial, data=testF)",
              "glmer(SerViol~Clinical*Dir+(1|ID), 
              family=binomial, data=testSer)",
              "glmer(SerViol~Risk*Dir+(1|ID), 
              family=binomial, data=testSer)",
              "glmer(SerViol~Historical*Dir+(1|ID),
              family=binomial, data=testSer)"
              )
  
myfinal=mclapply(mymodels, function(x) eval(parse(text=x)))
```

One thing I would add is that this by no means gets the full speed out of my CPU, which only ran at about 30% and not even all of the time. I think there’s probably more that I could do to get a speed boost, but this has got this job done and got me thinking about parallel processing.