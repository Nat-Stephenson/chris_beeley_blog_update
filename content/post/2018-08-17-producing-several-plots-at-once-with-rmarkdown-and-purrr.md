---
author: chrisbeeley
categories:
- Uncategorized
date: "2018-08-17T15:06:06Z"
title: Producing several plots at once with RMarkdown and purrr
---

This is a very simple example of using purrr and RMarkdown to produce several plots all at once.

```
invisible( # suppress console output from hist()
  map(c(5, 6, 7), function(x) { # values to feed to filter function
    
    iris %>%
      filter(Sepal.Length < x) %>% # just Sepal.Length < 5, 6, and 7
      pull(Petal.Width) %>% # extract Petal.Width vector
      hist(breaks = 20, main = paste0("Histogram of < ", x)) # graph
  })
)
```


