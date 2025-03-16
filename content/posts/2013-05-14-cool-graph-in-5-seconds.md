---
author: chrisbeeley
categories:
- Uncategorized
date: "2013-05-14T11:06:21Z"
guid: http://chrisbeeleyimh.wordpress.com/?p=398
id: 398
publicize_reach:
- a:2:{s:7:"twitter";a:1:{i:906009;i:50;}s:2:"wp";a:1:{i:0;i:3;}}
publicize_twitter_user:
- ChrisBeeley
title: Cool graph in 5 seconds
url: /?p=398
---

Cool graph in 5 seconds. Open R. Type

```
<pre class="brush: r; title: ; notranslate" title="">

library(rgl)

with(mtcars, plot3d(wt, disp, mpg, col=&quot;red&quot;, size=3))

```

Spin the graph with your mouse. Cool!

(Hat-tip to the mighty [Quick-R](http://www.statmethods.net/graphs/scatterplot.html) from whom I stole the code)