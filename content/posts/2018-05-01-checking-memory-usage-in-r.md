---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-05-01T09:11:34Z"
title: Checking memory usage in R
---

Wow, I cannot believe I didn’t blog this. I just tried to find it on my own blog and I must have forgotten. Anyway, it’s [an answer I gave on Stack Overflow](https://stackoverflow.com/a/48953571/486245) a little while ago, to do with measuring how much memory your R objects are using.

```
<pre class="brush: r; title: ; notranslate" title="">

install.packages("pryr")

library(pryr)

object_size(1:10)
## 88 B

object_size(mean)
## 832 B

object_size(mtcars)
## 6.74 kB

```

This is from Hadley Wickham’s [advanced R](http://adv-r.had.co.nz/memory.html).