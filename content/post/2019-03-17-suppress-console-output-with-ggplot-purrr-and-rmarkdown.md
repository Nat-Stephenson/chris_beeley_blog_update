---
author: chrisbeeley
categories:
- Uncategorized
date: "2019-03-17T18:27:09Z"
guid: https://chrisbeeley.net/?p=1198
id: 1198
title: Suppress console output with ggplot, purrr, and RMarkdown
url: /?p=1198
---

So [I posted a while back](https://chrisbeeley.net/?p=1143) about producing several plots at once with RMarkdown and purrr and how to suppress the console output in the document.

Well, I just spotted someone on Twitter having a similar problem and it turns out that the solution actually doesn’t work in ggplot! Interesting…

For ggplot, you need to excellent function walk() which is like map() except it’s called for its side effects (like disk access) rather than for its output per se.

Bish bash bosh. Easy

```
<pre class="brush: r; title: ; notranslate" title="">

```{r, message = FALSE, echo = FALSE}

library(tidyverse)
walk(c("Plot1", "Plot 2", "Plot 3"), function(x) {
  
  p <- iris %>%
    ggplot(aes(x = Sepal.Length)) + geom_histogram() +
    ggtitle(x)
  
  print(p)
})
```

```